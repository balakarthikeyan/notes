## Question 65: What caching layers does Next.js have in the App Router?
Next.js features four distinct caching layers in the App Router:

* **Request Memoization:** Server-side, per-request cache that deduplicates identical `fetch` requests within a single component render tree.
* **Data Cache:** Persistent server-side cache that stores `fetch` results across incoming requests and deployments.
* **Full Route Cache:** Server-side cache that stores rendered static HTML and React Server Component (RSC) payloads during build time or revalidation.
* **Router Cache:** Client-side in-memory cache that stores visited route segments in the browser to enable instant back/forward navigation.

The two most frequently tuned layers are the Data Cache (via `fetch` cache options and `revalidate`) and the Full Route Cache (by choosing static or dynamic rendering).

**Workflow/Architecture**

```text
Browser Navigation
       │
       ▼
[ Client Router Cache ] ──(Hit)──► Return In-Memory RSC
       │ (Miss)
       ▼
[ Full Route Cache ]    ──(Hit)──► Return Pre-Rendered HTML/RSC
       │ (Miss)
       ▼
[ Request Memoization ] ──(Hit)──► Deduplicate Duplicate Calls
       │ (Miss)
       ▼
[ Data Cache ]          ──(Hit)──► Return Persisted Fetch Data

```

**Example**

```tsx
// app/products/page.tsx
export default async function ProductsPage() {
  // Data Cache: Persistent across requests, revalidated every 3600 seconds
  const res = await fetch('https://api.example.com/products', {
    next: { revalidate: 3600, tags: ['products'] },
  });
  const products = await res.json();

  return <div>Total Products: {products.length}</div>;
}

```

**Output**
Subsequent requests within 3600 seconds hit the Data Cache and Full Route Cache instantly without making upstream HTTP calls to `api.example.com`.

---

## Question 66: What are route groups and parallel routes?

* **Route Groups `(folder)`:** Folder names wrapped in parentheses that allow developers to organize route files or apply shared layouts without modifying the public URL path structure. For instance, `app/(shop)/cart/page.js` maps directly to `/cart`.
* **Parallel Routes `@slot`:** Named slots that render multiple page segments simultaneously within the same layout. They power complex UIs like dynamic dashboards with independent panels or split-screen views.
* **Intercepting Routes `(.)folder`:** Allow rendering a route within a modal overlay while keeping the underlying page URL and context intact.

**Workflow/Architecture**

```text
app/
  ├── (shop)/               --> Route group (Does not affect URL)
  │   └── cart/page.tsx     --> URL Route: /cart
  └── dashboard/
      ├── layout.tsx        --> Layout receiving slots as props
      ├── page.tsx
      ├── @feed/page.tsx    --> Parallel Route Slot 1
      └── @modal/page.tsx   --> Parallel Route Slot 2

```

**Example**

```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({
  children,
  feed,
  modal,
}: {
  children: React.ReactNode;
  feed: React.ReactNode;
  modal: React.ReactNode;
}) {
  return (
    <div>
      <main>{children}</main>
      <section>{feed}</section>
      <div>{modal}</div>
    </div>
  );
}

```

**Output**
Renders `children`, `@feed`, and `@modal` routes in parallel inside a unified dashboard view.

---

## Question 67: How does Next.js optimize fonts?
`next/font` downloads Google Fonts during build time and self-hosts them locally alongside static application assets. This eliminates external browser network requests to Google servers at runtime, improves privacy, and prevents Cumulative Layout Shift (CLS) by generating matching fallback font metrics. Local fonts function identically using `next/font/local`.

**Example**

```tsx
// app/layout.tsx
import { Inter } from "next/font/google";

const inter = Inter({ subsets: ["latin"] });

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body className={inter.className}>{children}</body>
    </html>
  );
}

```

**Output**
Injects pre-calculated local font files and CSS declarations directly into pre-rendered page output with zero external Google Font HTTP calls.

---

## Question 68: What is the difference between revalidatePath and revalidateTag?
Both functions perform on-demand cache invalidation following data mutations, typically called within Server Actions or Route Handlers:

* **`revalidatePath('/todos')`:** Path-scoped invalidation that purges the cache for a single specific URL path route.
* **`revalidateTag('todos')`:** Data-scoped invalidation that purges every `fetch` request across all application routes tagged with `'todos'`.

**Example**

```tsx
// app/actions.ts
'use server';

import { revalidatePath, revalidateTag } from "next/cache";

export async function addTodo(formData: FormData) {
  await saveTodoToDatabase(formData.get('title'));

  // Invalidate specific route path
  revalidatePath("/todos");

  // Invalidate all fetches across the app tagged with "todos"
  revalidateTag("todos");
}

```

**Output**
Purges corresponding Data Cache entries immediately, forcing fresh data fetches on the next request.

---

## Question 69: How do you read cookies and headers in a Server Component?
Import `cookies()` and `headers()` from `next/headers` and invoke them inside an async Server Component, Server Action, or Route Handler. In modern Next.js versions, both functions are asynchronous and must be awaited. Calling either function opts the route segment into dynamic request-time rendering because output becomes dependent on request details.

**Example**

```tsx
// app/page.tsx
import { cookies, headers } from "next/headers";

export default async function Page() {
  const cookieStore = await cookies();
  const headerList = await headers();

  const theme = cookieStore.get("theme")?.value ?? "light";
  const userAgent = headerList.get("user-agent");

  return (
    <main data-theme={theme}>
      <p>Browser User Agent: {userAgent}</p>
    </main>
  );
}

```

**Output**
Dynamic server-rendered HTML reflecting the user's active theme cookie and browser user-agent header.

---

## Question 70: How do React Server Components actually work under the hood?
Server Components render on the server into a serialized stream called the **RSC Payload**, rather than raw HTML strings. The RSC Payload contains a structured representation of the rendered component tree and references to Client Component bundles with their passed props. On the client, React parses the payload to reconcile the DOM tree and hydrate Client Components.

**Workflow/Architecture**

```text
[ Server ]
  │ Render React Component Tree
  ▼
[ RSC Payload ] ──► (JSON-like stream containing tree structure & props)
  │ Sent over HTTP
  ▼
[ Client React Runtime ] ──► Reconciles DOM Tree & Hydrates Client Components

```

**Example**

```tsx
// Server Component output concept:
// RSC Payload string sent to client:
// M1:{"id":"./ClientWidget.js","name":"default","chunks":[]}
// J0:["$","div",null,{"children":[["$","h1",null,{"children":"Title"}],["$","$L1",null,{"user":"Alex"}]]}]

```

**Output**
Server Component JavaScript and heavy backend dependencies are kept completely off the client browser.

---

## Question 71: What is Partial Prerendering (PPR) and what problem does it solve?
Partial Prerendering (PPR) solves the binary trade-off between fully static and fully dynamic rendering. It pre-renders a static page shell at build time and serves it instantly from a CDN, while streaming dynamic, dynamic components inside React `Suspense` boundaries into the same HTTP response as they resolve.

**Workflow/Architecture**

```text
HTTP Request
  │
  ├─► [ Static Shell ] ──► Served Instantly from Edge CDN
  │
  └─► [ Suspense Slots ] ──► Streamed in parallel as dynamic promises settle

```

**Example**

```tsx
// app/products/[id]/page.tsx
import { Suspense } from 'react';
import StaticProductDetails from './StaticProductDetails';
import DynamicUserCartBadge from './DynamicUserCartBadge';

export default function ProductPage() {
  return (
    <div>
      {/* Pre-rendered static shell */}
      <StaticProductDetails />

      {/* Dynamic streamed hole */}
      <Suspense fallback={<span>Loading cart...</span>}>
        <DynamicUserCartBadge />
      </Suspense>
    </div>
  );
}

```

**Output**
The browser receives the static product shell immediately, followed by dynamic user cart data streamed seamlessly into the open Suspense boundary.

---

## Question 72: A page shows stale data in production. How do you debug it in Next.js?
Debug stale data issues by systematically verifying caching layers in sequence:

1. **Data Cache:** Check if `fetch` uses `force-cache` without invalidation rules; add `revalidateTag` or `revalidatePath` after data mutations.
2. **Full Route Cache:** Confirm whether the page segment was compiled as static or needs explicit dynamic route configuration (`export const dynamic = 'force-dynamic'`).
3. **Router Cache:** Call `router.refresh()` in Client Components to clear the browser's in-memory client navigation cache.

**Example**

```tsx
// Fixing Data Cache stale issues via explicit tag revalidation
// app/api/update/route.ts
import { revalidateTag } from 'next/cache';

export async function POST() {
  await updateDatabase();
  // Clear persistent Data Cache entry
  revalidateTag('user-data');
  return Response.json({ updated: true });
}

```

**Output**
Clears cached entries across layers, restoring fresh data fetching behavior.

---

## Question 73: How would you migrate a large app from the Pages Router to the App Router?
Migrate incrementally, as both the `pages/` and `app/` directories can run concurrently within the same project. Move routes one at a time: recreate routes inside `app/`, convert `getServerSideProps` and `getStaticProps` into inline async Server Component fetches, replace `next/router` imports with `next/navigation`, and isolate interactive UI into Client Components using `'use client'`.

**Example**

*Legacy Pages Router (`pages/todos.tsx`):*

```tsx
export async function getServerSideProps() {
  const res = await fetch('https://api.example.com/todos');
  const todos = await res.json();
  return { props: { todos } };
}

export default function Todos({ todos }: { todos: any[] }) {
  return <div>Todos Count: {todos.length}</div>;
}

```

*Modern App Router (`app/todos/page.tsx`):*

```tsx
export default async function TodosPage() {
  const res = await fetch('https://api.example.com/todos');
  const todos = await res.json();
  return <div>Todos Count: {todos.length}</div>;
}

```

**Output**
Replaces lifecycle props wrapper methods with cleaner async components while keeping unmigrated legacy pages running.

---

## Question 74: How do you implement authentication in a Next.js App Router app?
Store session tokens in HTTP-only cookies to protect them from client-side script access. Enforce access control across application layers: Edge Proxy (`proxy.ts`) handles early route gating and redirects, while actual authorization checks sit in the Data Access Layer (DAL) alongside database queries. Edge Proxy acts as a routing filter, not the final security boundary.

**Workflow/Architecture**

```text
Incoming Request ──► [ proxy.ts ] (Early Route Filter / Redirects)
                          │
                          ▼
                     [ Data Access Layer ] (Final Authorization Check)
                          │
                          ▼
                     [ Database Query ]

```

**Example**

```tsx
// lib/data-access.ts
import "server-only";
import { cookies } from "next/headers";

export async function getSecureUserData() {
  const cookieStore = await cookies();
  const token = cookieStore.get("session")?.value;

  if (!token) throw new Error("Unauthorized");

  // Perform authorization check in data access layer
  return db.query("SELECT * FROM users WHERE token = ?", [token]);
}

```

**Output**
Centralizes security validation directly inside data-fetching logic.

---

## Question 75: How do you find and fix performance problems in a Next.js app?
Measure performance metrics first using `next build` analysis output, Lighthouse, Web Vitals (LCP, CLS, INP), and bundle analyzers. Apply fixes in order of impact:

* Shift non-interactive logic to Server Components to reduce JavaScript bundle weight.
* Code-split heavy interactive libraries using `next/dynamic`.
* Optimize media with `next/image` and typography with `next/font`.
* Choose static rendering or ISR over per-request SSR where possible.

**Example**

```tsx
// app/dashboard/page.tsx
import dynamic from 'next/dynamic';

// Code-split heavy client-side chart library
const HeavyChart = dynamic(() => import('./HeavyChart'), {
  loading: () => <p>Loading chart module...</p>,
  ssr: false,
});

export default function Dashboard() {
  return <HeavyChart />;
}

```

**Output**
Reduces initial page load JavaScript bundle sizes by deferring component loading until required.

---

## Question 76: What are the security considerations with Server Actions?
A Server Action is a publicly reachable HTTP endpoint, even though it reads syntactically like an internal local function call. Developers must validate all input parameters, authenticate user identity, and verify explicit authorization inside every Server Action. Next.js provides action ID obfuscation and origin checks, but these do not replace server-side authorization checks.

**Example**

```tsx
// app/actions.ts
'use server';

import { cookies } from 'next/headers';
import { z } from 'zod';

const schema = z.object({ todoId: z.string() });

export async function deleteTodo(formData: FormData) {
  // 1. Authenticate user
  const cookieStore = await cookies();
  const token = cookieStore.get('session')?.value;
  if (!token) throw new Error("Unauthenticated");

  // 2. Validate input
  const { todoId } = schema.parse({ todoId: formData.get('todoId') });

  // 3. Perform authorized deletion
  await db.todo.delete({ where: { id: todoId, ownerToken: token } });
}

```

**Output**
Prevents unauthorized manipulation or injection attacks against exposure endpoints.

---

## Question 77: How do you structure data fetching in a large Next.js codebase?
Centralize data queries inside a dedicated Data Access Layer (DAL) containing query functions, authorization checks, and cache tags. Colocate data fetches directly where data is rendered, leveraging Request Memoization to deduplicate identical calls within a render tree, and execute independent queries in parallel with `Promise.all`.

**Example**

```tsx
// lib/dal.ts
import "server-only";

export async function getUser(id: string) {
  const res = await fetch(`https://api.example.com/users/${id}`, {
    next: { tags: ["user"] },
  });
  return res.json();
}

// app/user/[id]/page.tsx
import { getUser } from '@/lib/dal';

export default async function UserPage({ params }: { params: Promise<{ id: string }> }) {
  const { id } = await params;
  const user = await getUser(id);

  return <h1>Profile: {user.name}</h1>;
}

```

**Output**
Maintains reusable, authorized, and automatically deduplicated data queries across the application.

---

## Question 78: What are request waterfalls and how do you avoid them?
A request waterfall occurs when independent data fetches execute sequentially (awaiting one after another) rather than concurrently, causing cumulative latency delays. Avoid waterfalls by initiating independent fetches simultaneously using `Promise.all()` or streaming dynamic segments using `Suspense`.

**Example**

*Waterfall Execution (Slow - Sequential):*

```tsx
// Total latency = getUser time + getPosts time
const user = await getUser();
const posts = await getPosts();

```

*Parallel Execution (Fast - Concurrent):*

```tsx
// Total latency = Max(getUser, getPosts)
const [user, posts] = await Promise.all([getUser(), getPosts()]);

```

**Output**
Reduces total data-fetching wait time to the duration of the single slowest query.

---

## Question 79: How do you deploy a Next.js app outside of Vercel?
Next.js can run on any platform supporting Node.js. Set `output: 'standalone'` in `next.config.ts` to produce a minimal, self-contained server bundle that can be containerized using Docker. For static hosting environments, `output: 'export'` generates pure HTML/CSS/JS assets. Non-Vercel deployments require manual setup for ISR caching backends and image optimization services.

**Example**

*Configuration (`next.config.ts`):*

```typescript
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  output: 'standalone',
};

export default nextConfig;

```

*Dockerfile Snippet:*

```dockerfile
FROM node:20-alpine AS runner
WORKDIR /app
COPY .next/standalone ./
COPY .next/static ./.next/static
EXPOSE 3000
CMD ["node", "server.js"]

```

**Output**
Generates an optimized Docker image ready to deploy on AWS ECS, Kubernetes, or custom Linux servers.

---

## Question 80: How do you test a Next.js application?
Testing Next.js applications requires a layered approach:

* **Unit/Component Testing:** Use Jest or Vitest alongside React Testing Library to test individual utility functions and Client Component rendering.
* **End-to-End (E2E) Testing:** Use Playwright or Cypress to test complete user flows, routing transitions, and pre-rendered Server Component pages.
* **Server Actions & APIs:** Test Server Actions and Route Handlers directly as asynchronous functions or network boundaries.

**Example**

```typescript
// e2e/navigation.spec.ts (Playwright E2E)
import { test, expect } from '@playwright/test';

test('should navigate to about page', async ({ page }) => {
  await page.goto('http://localhost:3000/');
  await page.click('text=About');
  await expect(page).toHaveURL('http://localhost:3000/about');
  await expect(page.locator('h1')).toContainText('About Us');
});

```

**Output**
Validates complete application routing, rendering, and hydration flows.

---

## Question 81: How do you organize a Next.js app in a monorepo?
Monorepos use tools like Turborepo alongside npm or pnpm workspaces to manage multiple applications and shared packages (e.g., design systems, configuration, utilities). Turborepo caches build tasks and executes scripts in parallel. Shared packages export UI components and utilities, while individual Next.js applications manage their own routes and environment variables.

**Workflow/Architecture**

```text
my-monorepo/
  ├── apps/
  │   ├── web/            --> Next.js Application 1
  │   └── docs/           --> Next.js Application 2
  └── packages/
      ├── ui/             --> Shared Component Library
      └── config/         --> Shared ESLint / TS Configs

```

**Example**

```json
// turbo.json
{
  "$schema": "https://turbo.build/schema.json",
  "pipeline": {
    "build": {
      "outputs": [".next/**", "!-.next/cache/**"],
      "dependsOn": ["^build"]
    }
  }
}

```

**Output**
Enables incremental builds and package sharing across applications.

---

## Question 82: What are the trade-offs of streaming and where does it hurt?
Streaming improves perceived performance by rendering page shells immediately while dynamic slots load in parallel. However, once HTTP response headers begin streaming to the client, the status code and headers are locked. Unhandled server errors during mid-stream rendering cannot be converted into standard HTTP 500 pages or redirects. All redirect and authentication checks must resolve prior to entering streaming boundaries.

**Example**

```tsx
// app/dashboard/page.tsx
import { redirect } from 'next/navigation';
import { Suspense } from 'react';

export default async function DashboardPage() {
  const auth = await checkAuth();
  
  // Auth check MUST happen before rendering Suspense boundaries
  if (!auth) {
    redirect('/login');
  }

  return (
    <div>
      <h1>Dashboard</h1>
      <Suspense fallback={<div>Loading data...</div>}>
        <AsyncDataStream />
      </Suspense>
    </div>
  );
}

```

**Output**
Ensures redirects execute cleanly before the HTTP connection switches to streaming mode.

---

## Question 83: What causes hydration errors and how do you fix them?
Hydration errors occur when server-rendered markup differs from the client's initial render output. Common root causes include non-deterministic values (`Math.random()`, `Date.now()`), accessing browser-only globals (`window`, `localStorage`) during render, or invalid HTML element nesting corrected by the browser DOM parser.

Fix by deferring browser reads to `useEffect` or gating client UI behind a mounted state check.

**Example**

```tsx
'use client';

import { useState, useEffect } from 'react';

export default function ThemeDisplay() {
  const [mounted, setMounted] = useState(false);

  useEffect(() => {
    setMounted(true);
  }, []);

  if (!mounted) return <div>Theme: Loading...</div>;

  return <div>Theme: {window.localStorage.getItem('theme')}</div>;
}

```

**Output**
Server and initial client hydration match identically, eliminating console hydration warnings.

---

## Question 84: How do you load third-party scripts efficiently?
Use `next/script` with a specified `strategy` prop instead of standard HTML `<script>` tags:

* **`beforeInteractive`:** Loads critical scripts before page hydration.
* **`afterInteractive`:** (Default) Loads scripts immediately after page hydration completes.
* **`lazyOnload`:** Defers script execution until browser idle time (ideal for chat widgets and analytics).

**Example**

```tsx
import Script from 'next/script';

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en">
      <body>
        {children}
        {/* Low-priority analytics script */}
        <Script
          src="https://example.com/analytics.js"
          strategy="lazyOnload"
        />
      </body>
    </html>
  );
}

```

**Output**
Loads third-party JavaScript asynchronously without blocking page hydration or delaying initial rendering metrics.

---

## Question 85: What is Turbopack and how does it relate to webpack?
Turbopack is a Rust-based incremental bundler developed by Vercel as the successor to webpack for Next.js applications. It optimizes development fast-refresh cycles and production build performance by shifting compilation logic to Rust and applying granular caching.

**Example**

```bash
# Execute dev server using Turbopack
next dev --turbo

```

**Output**
Provides faster cold startup times and near-instantaneous code updates during development.

---

## Question 86: What do the server-only and client-only packages do?
Importing `'server-only'` into a module throws a build-time compile error if that module is imported into a Client Component, preventing database drivers or secret keys from leaking to the browser. `'client-only'` provides the opposite guardrail, failing builds if browser-only modules are imported by server code.

**Example**

```typescript
// lib/db.ts
import "server-only";

export async function queryDatabase() {
  return db.select().from('users');
}

```

**Output**
If `queryDatabase` is accidentally imported into a file with `'use client'`, `next build` fails immediately with an explicit build error.

---

## Question 87: How do you implement draft or preview mode for a CMS?
Draft Mode bypasses the Data Cache and Full Route Cache to render unpublished content dynamically. A Route Handler invokes `(await draftMode()).enable()`, which sets an authentication cookie that forces Next.js to render routes dynamically and fetch draft data from the CMS. Calling `(await draftMode()).disable()` restores normal caching.

**Example**

```typescript
// app/api/draft/route.ts
import { draftMode } from "next/headers";

export async function GET(request: Request) {
  const draft = await draftMode();
  draft.enable();
  return new Response("Draft mode enabled");
}

```

**Output**
Sets a draft preview cookie enabling editors to inspect unpublished CMS revisions directly on live routes.

---

## Question 88: How do you decide the rendering strategy for a new feature?
Determine rendering models based on data freshness and user personalization requirements:

* **Static (SSG):** Universal content that changes rarely.
* **ISR:** Universal content with periodic updates.
* **Dynamic (SSR):** Personalized or request-specific content.
* **Partial Prerendering (PPR):** Static layout shells containing dynamic streamed content slots.

**Example**

```tsx
// Explicit route segment configurations
export const dynamic = 'force-dynamic'; // SSR Mode
export const revalidate = 600;           // ISR Mode (10 mins)

```

**Output**
Optimizes application speed, CDN caching efficiency, and server load based on content requirements.

---

## Senior Interview Quick Points & Summary Cheatsheet

* **Caching Architecture:** Next.js manages 4 caches: Request Memoization (per-render deduplication), Data Cache (persistent fetch store), Full Route Cache (server HTML/RSC), and Router Cache (in-memory browser navigation).
* **Route Groups & Parallel Routes:** Use `(group)` to organize files without altering URL structures. Use `@slot` to render multiple dynamic slots in parallel within a shared layout.
* **RSC Execution Model:** Server Components output an RSC Payload stream rather than raw HTML strings. Server dependencies remain strictly off client bundles.
* **Partial Prerendering (PPR):** Combines static CDN shells with dynamic streamed Suspense content in a single route.
* **Data Invalidation:** Use `revalidatePath()` for path-scoped cache purging and `revalidateTag()` for cross-route data-tagged cache purging.
* **Security Boundaries:** Middleware and Edge Proxy gate routes, but actual authorization logic belongs inside the Data Access Layer alongside data queries. Server Actions are public HTTP endpoints and require explicit input validation and auth checks.
* **Build Guardrails:** Use `import 'server-only'` in database modules to throw build-time compilation errors if backend code is pulled into Client Components.
* **Performance Optimization:** Eliminate request waterfalls with `Promise.all()`, defer browser reads to `useEffect()` to avoid hydration errors, and load third-party scripts using `next/script` with `lazyOnload`.