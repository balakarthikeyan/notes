## Question 1: What is Next.js and Express.js?

**Next.js**

Next.js is a React framework developed by Vercel that enables advanced production features, including server-side rendering (SSR) and static site generation (SSG), alongside automatic optimization for creating SEO-friendly websites. It has evolved rapidly since its release in 2016, with the latest stable version being 16.0 (December 2025), offering advanced performance, Rust-based tooling, and improved developer experience.

**Key features of Next.js include:**

* **Server-Side Rendering (SSR):** Automatically renders pages on the server, which can help with SEO and load performance.
* **Static Site Generation (SSG):** Generates static HTML pages during build time for faster delivery to users.
* **File-based Routing:** Pages are created by adding files to the routes directory (`app/` or `pages/`), where each file corresponds to a route.
* **API Routes / Route Handlers:** Offers a simple way to build APIs directly in the same application using `app/api/` or `pages/api`.

**Express.js**

Express.js is a minimal and flexible Node.js web application framework designed for building APIs and web servers. It is widely used for creating RESTful APIs and handling server-side logic by providing a set of basic tools for routing, middleware, handling HTTP requests, and serving static files.

**Key features of Express.js include:**

* **Routing:** Easily defines routes for handling HTTP requests (`GET`, `POST`, `PUT`, `DELETE`, etc.).
* **Middleware:** Builds custom middleware functions to handle requests, responses, and errors.
* **Template Engines:** Supports various templating engines (e.g., EJS, Pug) for rendering dynamic HTML.
* **Flexibility:** Unlike full-stack frameworks, Express is minimalistic and gives developers full control over application architecture. Express is typically used to build backend APIs and can be combined with frontend technologies like React, Vue, or Angular to create full-stack applications.

**Key differences between Next.js and Express.js:**

* **Purpose and Focus:** Next.js is primarily a full-stack framework focused on React web applications, frontend rendering, routing, SSG, and SSR. Express.js is a minimal backend framework for creating APIs, serving static files, and executing server-side logic.
* **Rendering:** Next.js supports SSR, SSG, and CSR for SEO-friendly, high-performance web apps. Express.js does not focus on frontend rendering by default, though it can serve static assets or use templating engines like EJS.
* **API Handling:** Next.js uses API routes/Route Handlers aimed mainly at frontend integration or light server operations. Express.js is a powerhouse built for complex backend APIs, custom middleware, and low-level HTTP flow control.
* **Use Case:** Next.js is best suited for full-stack, content-heavy, or SEO-focused web apps. Express.js is ideal for backend microservices, REST APIs, and custom server architectures.
* **Complexity:** Next.js offers an opinionated structure with built-in routing, SSR, and SSG. Express.js provides unopinionated flexibility requiring manual architectural decisions.

**Workflow/Architecture**

```text
[ Client / Browser ]
        │
        ├─────────────────────────┐
        ▼                         ▼
[ Next.js Server ]       [ Express.js Server ]
  ├─ App Router / React    ├─ Custom Middleware
  ├─ SSR / SSG HTML        ├─ REST Routes / JSON
  └─ Route Handlers        └─ DB / Business Logic

```

**Example**

*Node.js project:*

```bash
npm init -y
npm install express
```

*Modern Express.js (ES Modules / Express 5+):*

```javascript
import express from 'express';

const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World from Express.js!');
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});

```

*Legacy Express.js (CommonJS):*

```javascript
const express = require('express');
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World from Express.js!');
});

app.listen(port, () => {
  console.log(`Server is running at http://localhost:${port}`);
});

```

**Output:**

```text
// Server Console Output:
Server is running at http://localhost:3000

// HTTP GET / Response (Browser/Terminal):
Hello World from Express.js!

```

---

## Question 2: How is Next.js different from React?
React is a client-side JavaScript library dedicated to building component-based user interfaces. Next.js is a full-stack framework built on top of React that introduces standardized application structure, production builds, and rendering architectures.

In a React application built with Vite or Create React App, developers must manually select and configure routing, data fetching tools, rendering models, and deployment configurations. In Next.js, file-system routing, server rendering, code splitting, metadata management, and production optimizations are provided natively out of the box.

**Example**

*React (Client-Side SPA Routing with React Router):*

```tsx
// App.tsx in Vite/React
import { BrowserRouter, Routes, Route } from 'react-router-dom';

export default function App() {
  return (
    <BrowserRouter>
      <Routes>
        <Route path="/" element={<h1>Home (Client Rendered)</h1>} />
      </Routes>
    </BrowserRouter>
  );
}

```

*Next.js (Framework Routing with App Router):*

```tsx
// app/page.tsx - Automatically rendered on the server
export default function HomePage() {
  return <h1>Home (Server Rendered Framework)</h1>;
}

```

**Output:**

* **React SPA:** Returns an empty `<div id="root"></div>` HTML skeleton that populates UI only after JavaScript executes in the browser.
* **Next.js:** Returns fully pre-rendered HTML `<h1>Home (Server Rendered Framework)</h1>` directly from the server on initial HTTP request.

---

## Question 3: What is the App Router?
The App Router is the modern Next.js router introduced in Next.js 13 and established as the default mental model for all production Next.js development. Built around the `app/` directory, it leverages React Server Components, nested routing, layout sharing, dynamic loading UIs, error boundaries, Route Handlers, and streaming.

While the legacy `pages/` directory relied on `getStaticProps` and `getServerSideProps`, the App Router natively uses async Server Components, `page.tsx`, `layout.tsx`, Route Handlers, and modern cache/revalidation primitives.

**Workflow/Architecture**

```text
app/
  ├── layout.tsx         ──► (Root Layout / Outer Shell)
  ├── page.tsx           ──► Route: /
  └── products/
      ├── page.tsx       ──► Route: /products
      └── [id]/
          └── page.tsx   ──► Route: /products/:id

```

**Example**

```tsx
// app/products/page.tsx -> Maps to /products
export default async function ProductsPage() {
  return <h1>Products Catalog</h1>;
}

// app/products/[id]/page.tsx -> Maps to dynamic route /products/:id
export default async function ProductDetailPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;
  return <h1>Product Details for ID: {id}</h1>;
}

```

**Output:**

* Request to `/products` renders `<h1>Products Catalog</h1>`.
* Request to `/products/42` renders `<h1>Product Details for ID: 42</h1>`.

---

## Question 4: What is file-based routing?
File-based routing is a pattern where the physical folder and file structure inside the project directory explicitly defines the public URL routes of the application. In the App Router, a directory path becomes publicly accessible as a route segment only when it contains a `page.tsx` (or `page.js`) file.

**Example**

```text
app/
  ├── page.tsx              --> Mapped Route: /
  ├── about/
  │   └── page.tsx          --> Mapped Route: /about
  └── blog/
      └── [slug]/
          └── page.tsx      --> Mapped Route: /blog/:slug

```

**Output:**

* Visiting `[https://example.com/](https://example.com/)` loads `app/page.tsx`.
* Visiting `[https://example.com/about](https://example.com/about)` loads `app/about/page.tsx`.
* Visiting `[https://example.com/blog/hello-world](https://example.com/blog/hello-world)` loads `app/blog/[slug]/page.tsx` with `slug = "hello-world"`.

---

## Question 5: What is the difference between page.tsx, layout.tsx, and template.tsx?
These three file conventions control component hierarchy and state persistence across route transitions:

* **`page.tsx`:** Renders the unique UI specific to a given route segment.
* **`layout.tsx`:** Wraps a route segment and its child segments. Layouts remain mounted, preserve component state, and do not re-render or remount during client-side navigation between sibling routes.
* **`template.tsx`:** Similar to a layout as it wraps child segments, but it creates a completely new instance on every navigation. State inside a template is completely reset when moving across routes.

Use layouts for persistent shells (e.g., sidebars, main navigation) and templates when enter/exit animations or state reset triggers are required on route changes.

**Example**

*Layout (`app/dashboard/layout.tsx`):*

```tsx
export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  return (
    <div className="dashboard-shell">
      <nav>Sidebar Navigation (Preserved)</nav>
      <main>{children}</main>
    </div>
  );
}

```

*Template (`app/dashboard/template.tsx`):*

```tsx
'use client';
import { useEffect } from 'react';

export default function DashboardTemplate({ children }: { children: React.ReactNode }) {
  useEffect(() => {
    console.log('Navigated to route segment - Template mounted');
  }, []);

  return <div className="animate-fade-in">{children}</div>;
}

```

**Output:**
Navigating between `/dashboard/analytics` and `/dashboard/settings` keeps the `DashboardLayout` DOM mounted without state loss, while `DashboardTemplate` remounts and logs the mounting message on every navigation.

---

## Question 6: What are dynamic routes in Next.js?
Dynamic routes handle incoming URL paths where specific segment values are dynamic and unknown ahead of time.

* **Basic Dynamic Route (`[slug]`):** Matches a single path segment.
* **Catch-all Route (`[...slug]`):** Matches all subsequent nested path segments.
* **Optional Catch-all Route (`[[...slug]]`):** Matches root segment as well as any sub-paths.

In Next.js 15 and 16, route parameters (`params`) are passed as an asynchronous `Promise` that must be awaited inside components.

**Example**

```tsx
// app/blog/[slug]/page.tsx
export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  return <article>Blog Post: {slug}</article>;
}

```

**Output:**
Requesting `/blog/nextjs-guide` outputs:

```html
<article>Blog Post: nextjs-guide</article>

```

---

## Question 7: What is generateStaticParams()?
`generateStaticParams()` is a server-side function used in dynamic route segments to define the list of route parameter combinations that Next.js should pre-render statically at build time. It replaces the legacy `getStaticPaths` function used in the Pages Router.

**Example**

```tsx
// app/blog/[slug]/page.tsx
export async function generateStaticParams() {
  const posts = [{ slug: 'first-post' }, { slug: 'second-post' }];

  return posts.map((post) => ({
    slug: post.slug,
  }));
}

export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  return <article>Static Post: {slug}</article>;
}

```

**Output:**
During `next build`, Next.js statically generates static HTML files for `/blog/first-post` and `/blog/second-post`.

---

## Question 8: What are Server Components?
Server Components are React components that execute exclusively on the server and never ship their component JavaScript bundle to the browser client. They are the default component model in the Next.js App Router.

They are ideal for directly reading files, querying databases, using secret API keys, and pre-rendering heavy static content. Server Components cannot access browser APIs (`window`, `localStorage`), handle DOM events (`onClick`), or use React state and effect hooks (`useState`, `useEffect`).

**Workflow/Architecture**

```text
[ Browser ] ◄── RSC Payload (JSON/HTML Stream) ── [ Next.js Server ]
 (No JS for Component)                             (DB Queries / Secrets)

```

**Example**

```tsx
// app/users/page.tsx (Default Server Component)
import fs from 'fs/promises';

export default async function UsersPage() {
  // Direct file system or database access on server
  const data = await fs.readFile('./data/users.json', 'utf8');
  const users = JSON.parse(data);

  return (
    <ul>
      {users.map((u: { id: number; name: string }) => (
        <li key={u.id}>{u.name}</li>
      ))}
    </ul>
  );
}

```

**Output:**
Browser receives rendered HTML without any component JavaScript execution overhead on the client side.

---

## Question 9: What are Client Components?
Client Components are React components that allow client-side interactivity and run in both the browser and server during initial HTML generation. To designate a component as a Client Component, add the `'use client'` directive at the very top of the file before any imports.

They are used for managing state, attaching event listeners, executing lifecycle effects, and interacting with browser-only APIs.

**Example**

```tsx
'use client';

import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  return (
    <button onClick={() => setCount(count + 1)}>
      Clicks: {count}
    </button>
  );
}

```

**Output:**
Renders a button that increments its displayed value dynamically when clicked by the user.

---

## Question 10: When should you use "use client"?
The `'use client'` directive should only be added when explicit browser runtime access is required:

* React hooks (`useState`, `useEffect`, `useReducer`, `useContext`).
* DOM Event Listeners (`onClick`, `onChange`, `onSubmit`).
* Browser-only APIs (`window`, `document`, `localStorage`, `navigator`).
* Custom browser animation or focus management libraries.

Do not place `'use client'` at the root of every file. Keeping static, heavy, or data-fetching logic in Server Components keeps the client JavaScript bundle minimal.

**Example**

```tsx
// app/search/page.tsx (Server Component - fetches static shell)
import SearchInput from './SearchInput';

export default function SearchPage() {
  return (
    <div>
      <h1>Search Portal</h1>
      {/* Isolated Client Boundary */}
      <SearchInput />
    </div>
  );
}

// app/search/SearchInput.tsx (Client Component)
'use client';
import { useState } from 'react';

export default function SearchInput() {
  const [query, setQuery] = useState('');
  return <input value={query} onChange={(e) => setQuery(e.target.value)} />;
}

```

**Output:**
Page HTML renders on the server while client JS bundle is downloaded exclusively for `SearchInput`.

---

## Question 11: Can a Server Component import a Client Component?
Yes, a Server Component can directly import and render a Client Component, passing serializable props to it.

However, a Client Component **cannot** directly import a Server Component because client JavaScript bundle code executes in the browser environment. To render a Server Component inside a Client Component, pass the Server Component as `children` or as a prop from a parent Server Component.

**Example**

*Allowed (Server Component imports Client Component):*

```tsx
// app/page.tsx (Server Component)
import ClientButton from './ClientButton';

export default function HomePage() {
  return <ClientButton label="Click Me" />;
}

```

*Passing Server Component as Children to Client Component:*

```tsx
// ClientWrapper.tsx
'use client';
export default function ClientWrapper({ children }: { children: React.ReactNode }) {
  return <div className="client-box">{children}</div>;
}

// app/page.tsx (Server Component Parent)
import ClientWrapper from './ClientWrapper';
import ServerData from './ServerData';

export default function Page() {
  return (
    <ClientWrapper>
      <ServerData />
    </ClientWrapper>
  );
}

```

**Output:**
The server resolves `ServerData` on the server and safely slots the generated output into `ClientWrapper`.

---

## Question 12: How do you fetch data in the App Router?
In Server Components, data is fetched asynchronously using direct `async/await` syntax with native `fetch()` or database client calls directly inside the component body.

In Client Components, data is fetched using traditional React hooks (`useEffect`), React Query/SWR, or custom hooks when dependent on user action or dynamic client updates. Server-side `fetch()` in Next.js extends web standard `fetch` with custom framework caching and revalidation controls.

**Example**

```tsx
// app/products/page.tsx (Server Component Data Fetching)
export default async function ProductPage() {
  const res = await fetch('https://api.example.com/products', {
    next: { revalidate: 3600 }, // Cache for 1 hour
  });
  const products = await res.json();

  return (
    <ul>
      {products.map((p: { id: number; title: string }) => (
        <li key={p.id}>{p.title}</li>
      ))}
    </ul>
  );
}

```

**Output:**
Data is fetched on the server, cached for 3600 seconds, and rendered as static HTML to the user.

---

## Question 13: What is the difference between static rendering and dynamic rendering?

* **Static Rendering:** HTML is pre-built at compile time (or updated in the background via revalidation) and served instantly from a CDN or server cache. It is ideal for public, non-personalized content (blogs, marketing pages).
* **Dynamic Rendering:** HTML is computed and rendered on the server at request time for every incoming request. It is required when content relies on request-specific context like user authentication cookies, HTTP headers, URL search parameters, or uncached data.

**Workflow/Architecture**

```text
Static:  Build/Revalidate Time ──► HTML Saved ──► Served Instantly
Dynamic: HTTP Request Received ──► Compute/Fetch ──► HTML Returned

```

**Example**

```tsx
// Static Rendering (Default if no dynamic functions used)
export default async function StaticPage() {
  return <div>Static Build Time: {new Date().toISOString()}</div>;
}

// Dynamic Rendering (Triggered by dynamic API usage like cookies)
import { cookies } from 'next/headers';

export default async function DynamicPage() {
  const cookieStore = await cookies();
  const token = cookieStore.get('token');
  return <div>User Session Token: {token?.value}</div>;
}

```

**Output:**
Static page returns identical fixed time across requests; Dynamic page evaluates fresh cookies on every request.

---

## Question 14: What is ISR (Incremental Static Regeneration)?
Incremental Static Regeneration (ISR) enables static pages to be updated in the background without needing to rebuild the entire application.

In legacy Pages Router code, ISR was defined via the `revalidate` property inside `getStaticProps`. In modern App Router architectures, ISR is achieved via `fetch()` revalidation times (`next: { revalidate: seconds }`), path/tag revalidation (`revalidatePath()`, `revalidateTag()`), or Cache Component directives (`"use cache"`).

**Example**

*Modern App Router ISR (Fetch Revalidate & Tag Revalidation):*

```tsx
// Data fetch with ISR tag
export default async function NewsFeed() {
  const res = await fetch('https://api.example.com/news', {
    next: { revalidate: 60, tags: ['news'] },
  });
  const articles = await res.json();
  return <section>{/* Articles list */}</section>;
}

// Server Action to instantly invalidate ISR cache on demand
import { revalidateTag } from 'next/cache';

export async function refreshNews() {
  'use server';
  revalidateTag('news');
}

```

*Legacy Pages Router ISR:*

```tsx
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/news');
  const articles = await res.json();

  return {
    props: { articles },
    revalidate: 60, // Regenerate background page after 60s
  };
}

```

**Output:**
Static HTML is served immediately to users. After 60 seconds (or upon invoking `revalidateTag`), a background build updates the cache seamlessly for future requests.

---

## Question 15: How does caching work in Next.js at a high level?
Next.js features a multi-layered caching architecture designed to optimize performance:

* **Request Memoization:** Deduplicates identical `fetch` calls within a single React component render tree.
* **Data Cache:** Persists fetched data across server requests globally unless invalidated (`force-cache`, `no-store`, `revalidateTag`).
* **Full Route Cache:** Stores HTML and RSC payload on the server for static routes.
* **Router Cache:** Client-side in-memory cache that stores RSC payloads during user navigation.

Modern Next.js 16 projects also support explicit Cache APIs via `'use cache'`, `cacheLife()`, `cacheTag()`, and `updateTag()`.

**Workflow/Architecture**

```text
Incoming Request
  │
  ├─► [ Client Router Cache ] (In-memory browser navigation)
  ├─► [ Full Route Cache ]   (Pre-rendered server HTML/RSC)
  ├─► [ Data Cache ]         (Server-persisted fetch data)
  └─► [ Request Memoization](In-flight duplicate request suppression)

```

**Example**

```tsx
// Force no cache (Bypass Data Cache)
fetch('https://api.example.com/realtime', { cache: 'no-store' });

// Cache indefinitely until tag invalidation
fetch('https://api.example.com/products', { next: { tags: ['products'] } });

```

**Output:**
`no-store` fetches live data on every invocation, whereas tagged fetches serve cached data instantly until invalidated.

---

## Question 16: What is loading.tsx?
`loading.tsx` is an automated route convention that creates instant loading states powered by React Suspense. While page server component data is being prepared, Next.js instantly serves `loading.tsx` static output, enabling progressive layout streaming.

**Example**

```tsx
// app/dashboard/loading.tsx
export default function Loading() {
  return <div className="spinner">Loading dashboard data...</div>;
}

```

**Output:**
Users see `"Loading dashboard data..."` immediately upon navigating to `/dashboard`, replaced seamlessly by `page.tsx` once server async tasks settle.

---

## Question 17: What is error.tsx?
`error.tsx` creates an isolated React Error Boundary wrapper around a route segment and its children. It intercepts uncaught server or client runtime exceptions and renders fallback diagnostic UI. `error.tsx` **must** be a Client Component.

**Example**

```tsx
// app/dashboard/error.tsx
'use client';

export default function ErrorBoundary({
  error,
  reset,
}: {
  error: Error & { digest?: string };
  reset: () => void;
}) {
  return (
    <div>
      <h2>Something went wrong: {error.message}</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  );
}

```

**Output:**
If `page.tsx` throws an unhandled error, the route displays the error message and a retry button without crashing the entire app shell.

---

## Question 18: What is not-found.tsx?
`not-found.tsx` defines a custom 404 UI boundary for a specific route segment. It is triggered automatically when an invalid URL is accessed or explicitly when calling the `notFound()` function inside server routes or pages.

**Example**

```tsx
// app/blog/[slug]/page.tsx
import { notFound } from 'next/navigation';

export default async function BlogPost({ params }: { params: Promise<{ slug: string }> }) {
  const { slug } = await params;
  const post = await fetchPost(slug);

  if (!post) {
    notFound(); // Triggers app/blog/[slug]/not-found.tsx or app/not-found.tsx
  }

  return <article>{post.title}</article>;
}

```

**Output:**
Renders custom 404 UI and sends HTTP status `404` to the user browser.

---

## Question 19: What are Route Handlers?
Route Handlers allow developers to create custom HTTP API endpoints within the App Router using `route.ts` or `route.js` files. They support standard HTTP methods (`GET`, `POST`, `PUT`, `DELETE`, `PATCH`, `HEAD`, `OPTIONS`) and execute strictly on the server.

Route Handlers replace legacy `pages/api` routes and are ideal for handling webhooks, third-party authentication callbacks, and microservice integration.

**Example**

```typescript
// app/api/health/route.ts
import { NextResponse } from 'next/server';

export async function GET() {
  return NextResponse.json({ status: 'ok', timestamp: new Date().toISOString() });
}

export async function POST(request: Request) {
  const body = await request.json();
  return NextResponse.json({ received: body }, { status: 201 });
}

```

**Output:**
Sending `GET /api/health` yields:

```json
{ "status": "ok", "timestamp": "2026-08-23T00:00:00.000Z" }

```

---

## Question 20: What are Server Functions and Server Actions?
Server Functions are asynchronous server-side functions marked with the `'use server'` directive. When invoked inside React forms or user interaction event handlers, they are referred to as **Server Actions**.

Server Actions execute securely on the server without requiring developers to manually write custom API endpoint boilerplate or fetch requests.

**Example**

```tsx
// app/actions.ts
'use server';

export async function updateUserProfile(formData: FormData) {
  const name = formData.get('name');
  // Mutate database directly
  await db.user.update({ where: { id: 1 }, data: { name: String(name) } });
}

// app/profile/page.tsx
import { updateUserProfile } from '../actions';

export default function ProfilePage() {
  return (
    <form action={updateUserProfile}>
      <input name="name" type="text" />
      <button type="submit">Update Profile</button>
    </form>
  );
}

```

**Output:**
Form submits data directly to the server action, executes database mutations securely on the server, and automatically updates UI cache.

---

## Question 21: What is the difference between a Route Handler and a Server Action?

* **Route Handler (`route.ts`):** Exposes an explicit public HTTP REST endpoint accessible by external software, webhooks, mobile apps, or public clients.
* **Server Action (`'use server'`):** Internal framework function coupled directly with React UI component interactions, form submissions, and state mutations.

**Example**

*Route Handler (`app/api/webhooks/stripe/route.ts`):*

```typescript
export async function POST(req: Request) {
  const event = await req.json();
  // External payment webhook callback
  return Response.json({ received: true });
}

```

*Server Action (`app/actions.ts`):*

```typescript
'use server';
import { revalidatePath } from 'next/cache';

export async function addItemToCart(id: string) {
  // App-internal UI action
  await cartDb.add(id);
  revalidatePath('/cart');
}

```

**Output:**
Route Handler returns JSON to external HTTP caller; Server Action mutates backend state and updates client UI.

---

## Question 22: What is the difference between SSR, SSG, CSR, and ISR?

* **SSR (Server-Side Rendering):** Renders dynamic HTML on every request.
* **SSG (Static Site Generation):** Generates static HTML at build time.
* **CSR (Client-Side Rendering):** Sends empty JS bundle; DOM is built entirely in browser.
* **ISR (Incremental Static Regeneration):** Serves static build pages while background-revalidating output periodically or on demand.

**Workflow/Architecture**

```text
SSR: Request ──► Render Server HTML ──► Return Fresh HTML
SSG: Build ──► Generate Static Files ──► Serve Instantly from CDN
CSR: Request ──► Return Empty HTML Shell ──► Browser Builds UI
ISR: Request ──► Serve Cached Static HTML ──► Regenerate Background

```

**Example**

```tsx
// Modern App Router Unified Configuration

// SSG: Static default
export const dynamic = 'force-static';

// SSR: Render on every request
export const dynamic = 'force-dynamic';

// ISR: Static with revalidation time
export const revalidate = 60; 

```

**Output:**
Controls framework compilation output for performance vs data freshness tradeoffs.

---

## Question 23: What is hydration?
Hydration is the process in which React executes in the browser to attach event listeners (`onClick`), internal component state (`useState`), and interactive logic to static HTML pre-rendered by the server.

**Workflow/Architecture**

```text
1. Server renders static HTML string ──► Sent to Browser (Visible UI)
2. Browser downloads Client JS Bundle
3. React performs Hydration ──► Attaches listeners (Interactive UI)

```

**Example**

```tsx
// HTML received from server before hydration:
// <button>Count: 0</button>

'use client';
import { useState } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);
  return <button onClick={() => setCount(count + 1)}>Count: {count}</button>;
}

```

**Output:**
The page displays button UI immediately upon HTTP load, and becomes clickable/interactive once React hydration completes.

---

## Question 24: What causes hydration errors?
Hydration errors occur when the server-generated HTML markup does not match the initial HTML markup tree generated by React on the client.

Common causes include:

* Direct rendering of dynamic values like `Date.now()` or `Math.random()`.
* Accessing browser-only APIs (`window`, `localStorage`) during render.
* Invalid HTML element nesting (e.g., nesting `<p>` inside `<p>` or `<div>` inside `<p>`).
* Divergent conditional logic dependent on client state during initial render.

**Example**

*Faulty Code (Triggers Hydration Error):*

```tsx
export default function Clock() {
  // Error: Server timestamp will differ from Client browser render timestamp!
  return <div>Current Time: {new Date().toLocaleTimeString()}</div>;
}

```

*Corrected Code (Client Effect or suppressHydrationWarning):*

```tsx
'use client';
import { useState, useEffect } from 'react';

export default function Clock() {
  const [time, setTime] = useState<string | null>(null);

  useEffect(() => {
    setTime(new Date().toLocaleTimeString());
  }, []);

  return <div>Current Time: {time ?? 'Loading...'}</div>;
}

```

**Output:**
Eliminates React hydration warning mismatch messages in browser console.

---

## Question 25: What is next/link used for?
`next/link` is the primary React component used for client-side navigation between Next.js routes. It prevents full browser page reloads and automatically prefetches route assets in the background when links enter the user viewport.

Use `<Link>` for internal app routing and standard `<a>` tags for external web links.

**Example**

```tsx
import Link from 'next/link';

export default function Navigation() {
  return (
    <nav>
      <Link href="/dashboard">Dashboard</Link>
      <a href="https://example.com" target="_blank" rel="noreferrer">External Site</a>
    </nav>
  );
}

```

**Output:**
Clicking `/dashboard` executes instantaneous SPA client navigation without triggering full page refresh.

---

## Question 26: What is useRouter() used for?
`useRouter()` is a hook imported from `next/navigation` used inside Client Components for programmatic navigation actions (e.g., redirecting after button clicks or form submissions).

In Server Components or server logic, use the server-side `redirect()` function instead.

**Example**

```tsx
'use client';

import { useRouter } from 'next/navigation';

export default function LoginButton() {
  const router = useRouter();

  const handleLogin = async () => {
    // Perform authentication logic
    router.push('/dashboard');
  };

  return <button onClick={handleLogin}>Log In</button>;
}

```

**Output:**
Programmatically navigates the user to `/dashboard` upon function execution.

---

## Question 27: How do you add metadata for SEO in Next.js?
In the App Router, metadata for search engine optimization (SEO) is managed by exporting a static `metadata` object or an async `generateMetadata()` function from a `page.tsx` or `layout.tsx` file.

**Example**

*Static Metadata:*

```tsx
import type { Metadata } from 'next';

export const metadata: Metadata = {
  title: 'Home - Tech Portal',
  description: 'Welcome to the technology platform',
};

export default function Page() {
  return <h1>Home Page</h1>;
}

```

*Dynamic Metadata:*

```tsx
import type { Metadata } from 'next';

export async function generateMetadata({
  params,
}: {
  params: Promise<{ id: string }>;
}): Promise<Metadata> {
  const { id } = await params;
  return {
    title: `Product ${id} Details`,
  };
}

```

**Output:**
Next.js injects HTML `<head>` tags: `<title>Product 42 Details</title>`.

---

## Question 28: What is next/image?
`next/image` is an optimized React image component that replaces standard HTML `<img>` elements. It automatically serves images in modern formats (WebP/AVIF), resizes images per device screen size, prevents Cumulative Layout Shift (CLS), and implements smart lazy loading.

**Example**

```tsx
import Image from 'next/image';

export default function Profile() {
  return (
    <Image
      src="/avatar.jpg"
      alt="User Avatar"
      width={150}
      height={150}
      priority
    />
  );
}

```

**Output:**
Generates responsive `<picture>` markup with responsive `srcset` and lazy loading optimizations.

---

## Question 29: What is next/font?
`next/font` automatically optimizes, self-hosts, and loads typography styles during application build time. It removes external network requests to third-party font providers (like Google Fonts) to improve visual performance and eliminate zero layout shifts.

**Example**

```tsx
// app/layout.tsx
import { Inter } from 'next/font/google';

const inter = Inter({ subsets: ['latin'] });

export default function RootLayout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  );
}

```

**Output:**
Self-hosts zero-CLS CSS font files generated statically into application assets.

---

## Question 30: What is Proxy in Next.js?
In Next.js 16, **Proxy** (`proxy.ts`) is the official file convention that replaces legacy `middleware.ts`. A single `proxy.ts` file located at the project root intercepts incoming HTTP requests before routing, rendering, or static asset execution completes.

It runs in lightweight edge environments and is designed for request-level routing decisions: redirects, rewrites, custom headers, and cookie authorization checks. It must not be used for heavy database queries or application-level business logic.

**Workflow/Architecture**

```text
HTTP Request ──► [ proxy.ts Interceptor ]
                       │
         ┌─────────────┴─────────────┐
         ▼                           ▼
[ Redirect / Rewrite ]     [ Continue to Route Rendering ]

```

**Example**

*Modern Next.js 16 Proxy (`proxy.ts`):*

```typescript
// proxy.ts (at project root)
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function proxy(request: NextRequest) {
  const token = request.cookies.get('auth_token');

  if (!token && request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.redirect(new URL('/login', request.url));
  }

  return NextResponse.next();
}

export const config = {
  matcher: ['/dashboard/:path*'],
};

```

*Legacy Next.js Middleware (`middleware.ts`):*

```typescript
// middleware.ts (Legacy)
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function middleware(request: NextRequest) {
  return NextResponse.next();
}

```

**Output:**
Unauthenticated requests accessing `/dashboard` are intercepted at network edge and redirected instantly to `/login`.

---

## Question 31: How do environment variables work in Next.js?
Environment variables without prefixes are kept strictly on the server side and are used for API secrets, tokens, and database connections.

Variables prefixed with `NEXT_PUBLIC_` are embedded directly into client browser JavaScript bundles during build compile step. Never expose private keys or secrets with the `NEXT_PUBLIC_` prefix.

**Example**

```env
# .env.local
DATABASE_SECRET=super_secret_db_password
NEXT_PUBLIC_ANALYTICS_ID=pub_analytics_12345

```

```tsx
// app/page.tsx
export default async function Page() {
  console.log(process.env.DATABASE_SECRET); // Available on Server
  console.log(process.env.NEXT_PUBLIC_ANALYTICS_ID); // Available on Client & Server
  return <div>Analytics: {process.env.NEXT_PUBLIC_ANALYTICS_ID}</div>;
}

```

**Output:**
`DATABASE_SECRET` remains securely on server; `NEXT_PUBLIC_ANALYTICS_ID` renders safely in browser markup.

---

## Question 32: How do you handle authentication in a Next.js app?
Authentication in Next.js involves managing session tokens/cookies with server-side security checks:

* **Edge Interception:** `proxy.ts` checks auth tokens early and redirects unauthenticated requests.
* **Server Components & Route Handlers:** Read session cookies (`cookies()`) to enforce authorization before rendering pages or returning data.
* **Server Actions:** Perform secure authentication mutations (login, logout, token refresh).

Always perform authorization checks on the server side—never rely exclusively on client-side state.

**Workflow/Architecture**

```text
[ Client Login ] ──► [ Server Action ] ──► Sets HTTP-Only Cookie
[ Navigate Route ] ──► [ proxy.ts Check ] ──► [ Server Component Auth Verify ]

```

**Example**

```tsx
// app/dashboard/page.tsx
import { cookies } from 'next/headers';
import { redirect } from 'next/navigation';

export default async function DashboardPage() {
  const cookieStore = await cookies();
  const token = cookieStore.get('auth_token');

  if (!token) {
    redirect('/login');
  }

  return <h1>Protected Dashboard</h1>;
}

```

**Output:**
Validates session cookie on server; renders page HTML if valid, or redirects to `/login` if absent.

---

## Question 33: What is the difference between redirect() and client-side navigation?

* **`redirect()` (Server-Side):** Called within Server Components, Route Handlers, or Server Actions to issue HTTP 307/308 redirect headers and stop rendering immediately.
* **`router.push()` (Client-Side):** Executed inside Client Components via `useRouter()` in response to client interactions.

**Example**

*Server-Side (`redirect`):*

```tsx
import { redirect } from 'next/navigation';

export default async function ProtectedPage() {
  const authorized = false;
  if (!authorized) {
    redirect('/unauthorized');
  }
}

```

*Client-Side (`router.push`):*

```tsx
'use client';
import { useRouter } from 'next/navigation';

export default function ClientNav() {
  const router = useRouter();
  return <button onClick={() => router.push('/about')}>About</button>;
}

```

**Output:**
Server redirect triggers HTTP header re-routing; client navigation transitions routes dynamically in browser memory.

---

## Question 34: How do you deploy a Next.js app?
Next.js applications are compiled into optimized production builds using `next build`. Deployment options include:

* **Managed Cloud (Vercel):** Native support for zero-config global Edge Proxy, Route Handlers, and automatic ISR CDN distribution.
* **Node.js Server Hosts / Docker Container:** Utilizing Next.js `output: 'standalone'` mode to create a self-contained Node.js server production bundle.

**Example**

*Configuring standalone build (`next.config.ts`):*

```typescript
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  output: 'standalone',
};

export default nextConfig;

```

*Build Commands:*

```bash
npm run build
npm run start

```

**Output:**
Generates standalone server distribution files inside `.next/standalone` ready for containerization or server hosting.

---

## Quick Points & Summary

* **App Router Paradigm:** Default model built on React Server Components, nested layouts, and dynamic streaming.
* **Server Components vs Client Components:** Server Components run exclusively on the server with zero client JS bundle. Use `'use client'` strictly for interactivity, DOM events, and React hooks.
* **Params Async Boundary (Next.js 15+):** Dynamic route `params` and `searchParams` are Promises and must be awaited inside `page.tsx` and dynamic handlers.
* **Next.js 16 Proxy (`proxy.ts`):** Replaces legacy `middleware.ts`. Located at project root to execute request-level redirects, rewrites, and headers at the network edge.
* **Data Fetching & Caching:** Direct `async/await` in Server Components. Caching controlled via `cache: 'no-store'`, `revalidateTag()`, `revalidatePath()`, and `'use cache'` directives.
* **Data Security & Env Variables:** Unprefixed `.env` variables are server-only. `NEXT_PUBLIC_` variables are bundled into browser JS. Always enforce authorization checks on the server.
* **Error & Loading Boundaries:** `loading.tsx` manages automatic React Suspense streaming; `error.tsx` acts as an error boundary and must be a Client Component.
* **Server Actions:** Secure server-side mutation functions marked `'use server'`, simplifying form actions and API orchestration without custom endpoint setup.
* **Static vs Dynamic Rendering:** Pre-built HTML (SSG/ISR) vs request-time generation (SSR). Dynamic rendering is triggered when using request-specific primitives like cookies, headers, or search params.