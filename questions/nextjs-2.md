## Question 35: How do you implement redirects and 404 handling?
Redirects in Next.js can be configured at the server configuration level (`next.config.ts`), inside Edge Proxy (`proxy.ts`), or programmatically using the `redirect()` function inside Server Components, Route Handlers, and Server Actions. Custom 404 handling in the App Router uses the `not-found.tsx` file convention and is triggered automatically on invalid paths or explicitly by calling the `notFound()` function.

**Workflow/Architecture**

```text
Incoming Request
  │
  ├─► Trigger redirect() / proxy.ts  ──► Send HTTP 307/308 Redirect
  │
  └─► Resource missing / notFound()  ──► Render app/not-found.tsx (HTTP 404)

```

**Example**

*Modern App Router Redirect & 404 (`app/products/[id]/page.tsx`):*

```tsx
import { redirect, notFound } from 'next/navigation';

export default async function ProductPage({
  params,
}: {
  params: Promise<{ id: string }>;
}) {
  const { id } = await params;

  if (id === 'deprecated') {
    redirect('/products'); // Programmatic Redirect
  }

  const res = await fetch(`https://api.example.com/products/${id}`);
  if (!res.ok) {
    notFound(); // Triggers app/not-found.tsx
  }

  const product = await res.json();
  return <h1>Product: {product.name}</h1>;
}

```

*Legacy Pages Router 404 (`pages/404.js`):*

```javascript
export default function Custom404() {
  return <h1>404 - Page Not Found (Legacy)</h1>;
}

```

**Output**

* Visiting `/products/deprecated` immediately redirects to `/products` with an HTTP 307 header.
* Visiting `/products/invalid-id` returns an HTTP 404 status and renders the `not-found.tsx` fallback UI.

---

## Question 36: What do loading, error, and not-found files do?
These special file conventions automatically manage asynchronous states and exception boundaries for route segments without requiring manual conditional rendering logic inside components:

* **`loading.tsx`:** Creates an instant loading UI state powered by React Suspense while route data loads.
* **`error.tsx`:** Wraps a route segment in a React Client Error Boundary to isolate runtime errors and display fallback UI.
* **`not-found.tsx`:** Serves as a dedicated fallback UI when a requested resource is missing.

**Example**

```text
app/
  └── dashboard/
      ├── loading.tsx     --> Instant Suspense spinner
      ├── error.tsx       --> Client Error Boundary ("use client")
      ├── not-found.tsx   --> Missing segment 404 UI
      └── page.tsx        --> Main dashboard route page

```

```tsx
// app/dashboard/error.tsx
'use client';

export default function ErrorBoundary({
  error,
  reset,
}: {
  error: Error;
  reset: () => void;
}) {
  return (
    <div>
      <p>Dashboard failure: {error.message}</p>
      <button onClick={() => reset()}>Retry</button>
    </div>
  );
}

```

**Output**

* During server data fetching, `loading.tsx` renders instantly.
* If rendering succeeds, `page.tsx` swaps in dynamically.
* If a runtime error occurs, `error.tsx` isolates the crash to the dashboard segment, keeping the rest of the application responsive.

---

## Question 37: What’s the difference between layout and template?
While both files wrap route segments, `layout.tsx` preserves its state and DOM subtree across sibling route navigations, whereas `template.tsx` creates a brand-new component instance on every navigation. Use layouts for persistent shared shells (sidebars, headers) and templates when enter/exit animations or state resets are required on route changes.

**Example**

*Layout (`app/dashboard/layout.tsx`):*

```tsx
'use client';
import { useState } from 'react';

export default function DashboardLayout({ children }: { children: React.ReactNode }) {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Layout Count: {count}</button>
      {children}
    </div>
  );
}

```

*Template (`app/dashboard/template.tsx`):*

```tsx
'use client';
import { useState } from 'react';

export default function DashboardTemplate({ children }: { children: React.ReactNode }) {
  const [count, setCount] = useState(0);

  return (
    <div>
      <button onClick={() => setCount(count + 1)}>Template Count: {count}</button>
      {children}
    </div>
  );
}

```

**Output**
Navigating between `/dashboard/settings` and `/dashboard/profile`:

* `Layout Count` preserves its value.
* `Template Count` resets back to `0` on every navigation.

---

## Question 38: What are layouts and why are they useful?
Layouts are wrapper components that allow developers to share common UI elements (such as headers, navigation menus, and footers) across multiple pages. They eliminate redundant layout code, preserve internal component state during route transitions, and prevent unnecessary full-page re-renders.

**Example**

```tsx
// app/dashboard/layout.tsx
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <div className="dashboard-shell">
      <aside><nav>Sidebar Navigation</nav></aside>
      <main>{children}</main>
    </div>
  );
}

```

**Output**
All routes within `/dashboard/*` inherit the sidebar navigation shell automatically without re-rendering the sidebar DOM during sub-route navigation.

---

## Question 39: How do dynamic routes work in Next.js?
Dynamic routes use folder naming conventions with square brackets (`[param]`, `[...slug]`, `[[...slug]]`) to map parameter values directly from incoming URL paths. A single dynamic route file can handle thousands of dynamic URLs, such as product detail pages or blog articles. In modern Next.js, parameter objects are passed as Promises and must be awaited.

**Example**

```tsx
// app/blog/[slug]/page.tsx
export default async function BlogPost({
  params,
}: {
  params: Promise<{ slug: string }>;
}) {
  const { slug } = await params;
  return <h1>Reading Article: {slug}</h1>;
}

```

**Output**
Visiting `/blog/react-server-components` resolves `params` and outputs:

```html
<h1>Reading Article: react-server-components</h1>

```

---

## Question 40: How does routing work in the app directory?
Routing in the App Router is driven strictly by folder structure hierarchy. Every folder represents a URL segment, and nested folders construct nested URL paths. Route segments become publicly accessible only when they contain a `page.tsx` file, making routing predictable and scalable.

**Workflow/Architecture**

```text
Project Folder Directory             Public Route URL
────────────────────────             ────────────────
app/page.tsx                  ──►    /
app/about/page.tsx            ──►    /about
app/shop/categories/page.tsx  ──►    /shop/categories

```

**Example**

```tsx
// app/shop/categories/page.tsx
export default function CategoriesPage() {
  return <h1>Product Categories</h1>;
}

```

**Output**
Navigating to `[https://example.com/shop/categories](https://example.com/shop/categories)` renders the UI from `app/shop/categories/page.tsx`.

---

## Question 41: How do you fetch data in the App Router?
Data is fetched directly inside async Server Components using the web-standard `fetch()` API. Performing data fetching directly on the server prevents exposing sensitive credentials or API tokens to the client and reduces client-side JavaScript bundles.

**Example**

```tsx
// app/users/page.tsx (Server Component)
export default async function UsersPage() {
  const res = await fetch('https://jsonplaceholder.typicode.com/users');
  const users = await res.json();

  return (
    <ul>
      {users.map((user: { id: number; name: string }) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}

```

**Output**
The server fetches user data during rendering and delivers pure pre-rendered HTML to the client browser.

---

## Question 42: How do you balance stale data and performance?
Balancing stale data against performance requires tuning revalidation intervals based on business domain requirements. Serving static, cached data minimizes response latency, while Incremental Static Regeneration (ISR) or tagged cache invalidation ensures information stays accurate without re-rendering every request from scratch.

**Example**

*Modern Next.js Fetch Revalidation:*

```tsx
export default async function ProductCatalog() {
  // Revalidate cached output in the background every 60 seconds
  const res = await fetch('https://api.example.com/products', {
    next: { revalidate: 60 },
  });
  const products = await res.json();

  return <div>Catalog Total: {products.length}</div>;
}

```

**Output**
Requests are served instantly from the cache. Once 60 seconds pass, the next incoming request triggers an asynchronous background regeneration to update the cached response.

---

## Question 43: How do you handle authenticated data fetching?
Authenticated data fetching keeps authentication tokens, API secrets, and private keys strictly on the server. Requests inspect session cookies or authorization headers inside Server Components, Route Handlers, or Edge Proxy using secure server-side environment variables.

**Example**

```tsx
// app/profile/page.tsx
import { cookies } from 'next/headers';

export default async function ProfilePage() {
  const cookieStore = await cookies();
  const token = cookieStore.get('session_token');

  const res = await fetch('https://api.example.com/user/me', {
    headers: {
      Authorization: `Bearer ${token?.value}`,
      'X-API-Secret': process.env.INTERNAL_API_SECRET!, // Server secret
    },
  });

  const profile = await res.json();
  return <h1>Welcome, {profile.name}</h1>;
}

```

**Output**
The server validates the session and fetches profile details securely without exposing `INTERNAL_API_SECRET` or raw tokens to the client bundle.

---

## Question 44: When would you use static generation vs server rendering?
Static Generation (SSG/ISR) is best suited for public, content-driven pages (such as blogs, documentation, and product catalogs) that can be cached and served globally via CDNs. Server-Side Rendering (SSR) is used when pages contain highly dynamic, frequently changing, or personalized data that depends on request cookies, headers, or parameters.

**Example**

*Static Generation (`app/blog/page.tsx`):*

```tsx
export const dynamic = 'force-static';

export default async function BlogIndex() {
  return <div>Static Blog Index</div>;
}

```

*Server-Side Rendering (`app/feed/page.tsx`):*

```tsx
export const dynamic = 'force-dynamic';

export default async function LiveFeed() {
  return <div>Live Request Time: {new Date().toISOString()}</div>;
}

```

**Output**

* Static Blog Index HTML is built once and served instantly from Edge CDNs.
* Live Feed HTML is calculated on the server dynamically for every incoming HTTP request.

---

## Question 45: How does caching work in Next.js?
Next.js provides built-in multi-tier caching that covers request memoization, data fetch caching, full route HTML output caching, and client-side router caching. Developers can control cache duration and invalidation rules using time-based revalidation or on-demand cache tags (`revalidateTag`).

**Workflow/Architecture**

```text
HTTP Fetch Request
       │
       ▼
[ Data Cache ] ─── (Hit) ───► Return Cached JSON
       │
    (Miss)
       ▼
Execute API Call ──► Save to Data Cache ──► Return Fresh JSON

```

**Example**

```tsx
// app/actions.ts
'use server';
import { revalidateTag } from 'next/cache';

export async function updateProduct() {
  // Purge data cache tagged with 'products'
  revalidateTag('products');
}

```

**Output**
Components configured with `fetch(url, { next: { tags: ['products'] } })` purge their cached data instantly when `revalidateTag('products')` is called.

---

## Question 46: What changes when deploying SSR vs static apps?
Deploying SSR applications requires active Node.js server resources or serverless runtimes, introducing compute costs and requiring server health monitoring and scaling. Static applications are pre-compiled into static HTML, CSS, and JS assets that can be hosted cheaply and served globally via CDNs, but they lack request-time dynamic rendering capabilities.

**Example**

*Static Export Configuration (`next.config.ts`):*

```typescript
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  output: 'export', // Produces static /out folder
};

export default nextConfig;

```

*SSR Standalone Node Configuration (`next.config.ts`):*

```typescript
import type { NextConfig } from 'next';

const nextConfig: NextConfig = {
  output: 'standalone', // Produces lightweight Node server app
};

export default nextConfig;

```

**Output**

* `output: 'export'` generates plain static files in `/out` ready for any web server or S3 bucket.
* `output: 'standalone'` builds a minimal `server.js` Node process ready for Docker container deployment.

---

## Question 47: How do you manage secrets safely?
Secrets (such as database passwords and private API keys) are stored in server-side environment variables and must never be accessed in Client Components. They are evaluated exclusively within Server Components, Route Handlers, or Server Actions, managed via `.env.local` locally and secure secret managers in production.

**Example**

```tsx
// app/api/db-check/route.ts
export async function GET() {
  // Safe: process.env.DATABASE_URL is only executed on the server
  const dbUrl = process.env.DATABASE_URL;
  return Response.json({ status: 'Connected', host: dbUrl?.split('@')[1] });
}

```

**Output**
The secret database credentials remain secure on the server and are omitted from all browser JavaScript bundles.

---

## Question 48: What are common production issues?
Common production issues in Next.js applications include caching anomalies, environment variable mismatches between staging and production, API request timeouts under traffic spikes, misconfigured revalidation settings leading to stale data, rate limits, and memory leaks. Resolving them relies on structured logging and monitoring tools.

**Example**

```tsx
// Graceful API Timeout & Fallback handling
export default async function MetricsWidget() {
  try {
    const controller = new AbortController();
    const timeoutId = setTimeout(() => controller.abort(), 3000); // 3s Timeout

    const res = await fetch('https://api.example.com/metrics', {
      signal: controller.signal,
      cache: 'no-store',
    });
    clearTimeout(timeoutId);

    if (!res.ok) throw new Error('Upstream API error');
    const data = await res.json();
    return <div>Active Users: {data.users}</div>;
  } catch (err) {
    return <div>Metrics temporarily unavailable</div>;
  }
}

```

**Output**
If the external API stalls or times out, the component renders a fallback message instead of hanging the HTTP request or crashing the page.

---

## Question 49: What steps do you follow to deploy a Next.js app?
Deploying a Next.js application follows a structured workflow:

1. Compiling code via `next build` to optimize assets and generate production output.
2. Configuring production environment variables securely.
3. Connecting version control to a cloud hosting platform (e.g., Vercel, AWS, Docker) with CI/CD automation.
4. Running automated linting, unit testing, and build verification checks.
5. Monitoring application logs and performance post-deployment.

**Workflow/Architecture**

```text
Git Commit ──► CI/CD Pipeline ──► Lint & Test ──► next build ──► Deploy Output ──► Live App & Logs

```

**Example**

*GitHub Actions Deployment Workflow Snippet (`.github/workflows/deploy.yml`):*

```yaml
name: Next.js CI/CD
on:
  push:
    branches: [main]
jobs:
  build-and-test:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - run: npm ci
      - run: npm run lint
      - run: npm run build

```

**Output**
Automates testing and build validation before deploying output safely to production hosting environments.

---

## Question 50: How do you configure environment variables?
Environment variables are defined in `.env.local` for local development and configured via the cloud platform dashboard in production. In Next.js, variables prefixed with `NEXT_PUBLIC_` are exposed to client-side browser JavaScript, while unprefixed variables remain server-only.

**Example**

*.env.local File:*

```env
# Server-only (Secret)
DATABASE_PASSWORD=secret_db_pass_123

# Client-exposed
NEXT_PUBLIC_SITE_URL=https://example.com

```

*Component Context:*

```tsx
export default function EnvCheck() {
  return (
    <div>
      <p>Public URL: {process.env.NEXT_PUBLIC_SITE_URL}</p>
      {/* process.env.DATABASE_PASSWORD is undefined in Client Components */}
    </div>
  );
}

```

**Output**
Client UI renders `Public URL: [https://example.com](https://example.com)` while server secrets remain protected.

---

## Question 51: What is Next.js and what problems does it solve compared to plain React?
Next.js is a full-stack React framework that provides server-side rendering, static site generation, file-based routing, image optimization, and built-in API support out of the box. Standard React is primarily a client-side UI library, requiring developers to manually configure routing, build setups, SEO handling, and SSR infrastructure.

**Example**

*Plain React (Requires Client JavaScript Execution for Initial UI):*

```tsx
// React SPA Root HTML delivered from server:
// <div id="root"></div>

```

*Next.js (Delivers Pre-Rendered HTML Directly):*

```tsx
// app/page.tsx
export default function Page() {
  return <h1>SEO Friendly Pre-Rendered Page</h1>;
}

```

**Output**
Search engine crawlers and browsers receive completed HTML content immediately on initial request, improving SEO scores and performance metrics.

---

## Question 52: What does “server-first” development mean in modern Next.js?
"Server-first" development means components execute on the server by default in the App Router. This architecture keeps component code and heavy dependencies on the server, ships less client-side JavaScript, improves initial page load speed, and isolates database access and secrets.

**Example**

```tsx
// app/stats/page.tsx (Runs on Server by Default)
import db from '@/lib/db';

export default async function StatsPage() {
  // Direct SQL query executed on server without sending DB driver to client
  const totalUsers = await db.query('SELECT COUNT(*) FROM users');

  return <div>Total Registered Users: {totalUsers}</div>;
}

```

**Output**
The browser receives pre-rendered static HTML containing user count, adding 0 KB of component dependencies to the client JS bundle.

---

## Question 53: How do you decide between CSR, SSR, SSG, and ISR for a page?
Select rendering strategies based on requirements for interactivity, data freshness, and performance:

* **CSR:** Interactive dashboards or authenticated app pages where SEO is unnecessary.
* **SSR:** Highly dynamic or personalized pages where data must be up-to-date on every request.
* **SSG:** Public marketing pages or documentation that rarely change.
* **ISR:** High-traffic pages with semi-dynamic data that need fast load times alongside periodic background updates.

**Example**

```tsx
// SSG Mode
export const dynamic = 'force-static';

// SSR Mode
export const dynamic = 'force-dynamic';

// ISR Mode
export const revalidate = 300; // Revalidate every 5 minutes

```

**Output**
Configures route compilation output to optimize performance against data freshness trade-offs.

---

## Question 54: What’s the difference between the App Router and Pages Router?
The legacy Pages Router uses the `pages/` directory and relies on lifecycle functions like `getStaticProps` and `getServerSideProps` for data fetching. The modern App Router uses the `app/` directory and is built on React Server Components, nested layouts, streaming, and direct async data fetching inside components.

**Example**

*Legacy Pages Router (`pages/products.tsx`):*

```tsx
export async function getStaticProps() {
  const res = await fetch('https://api.example.com/products');
  const products = await res.json();
  return { props: { products } };
}

export default function Products({ products }: { products: any[] }) {
  return <div>Products Count: {products.length}</div>;
}

```

*Modern App Router (`app/products/page.tsx`):*

```tsx
export default async function Products() {
  const res = await fetch('https://api.example.com/products');
  const products = await res.json();
  return <div>Products Count: {products.length}</div>;
}

```

**Output**
Both approaches yield pre-rendered static HTML, but the App Router eliminates boilerplate props wrappers and enables streaming directly out of the box.

---

## Question 55: What are the key features of Next.js?
Core features of Next.js include file-based routing, flexible rendering strategies (SSR, SSG, ISR, CSR), automatic image and font optimization, built-in Route Handlers and Server Actions, and native SEO metadata support. These capabilities reduce manual infrastructure configuration and accelerate development.

**Example**

```tsx
// Combining metadata, image optimization, and Server Components
import Image from 'next/image';
import type { Metadata } from 'next';

export const metadata: Metadata = { title: 'Product Overview' };

export default function Overview() {
  return (
    <div>
      <Image src="/hero.png" width={400} height={200} alt="Hero" priority />
      <h1>Next.js Feature Suite</h1>
    </div>
  );
}

```

**Output**
Renders optimized HTML output with managed `<title>` tags, preloaded images, and fast load performance.

---

## Question 56: How would you migrate from Pages Router to App Router?
Migration is performed incrementally by running both the `pages/` and `app/` directories in parallel. Start by migrating low-risk, non-critical routes first, updating data fetching mechanisms from `getStaticProps` or `getServerSideProps` to async Server Components and nested layouts.

**Example**

*Step 1: Legacy page active in `pages/about.tsx*`

```tsx
export default function About() {
  return <h1>About Us (Pages Router)</h1>;
}

```

*Step 2: Move route to `app/about/page.tsx` and delete `pages/about.tsx*`

```tsx
export default async function AboutPage() {
  return <h1>About Us (App Router)</h1>;
}

```

**Output**
Next.js routes incoming requests to `/about` using the App Router while existing legacy routes in `pages/` continue functioning uninterrupted.

---

## Question 57: How do you handle pagination and filtering?
Pagination and filtering are managed on the server using URL query parameters (`searchParams`). Reading search parameters directly inside Server Components triggers targeted server-side data fetching, preserving SEO readability and eliminating client-side data parsing overhead.

**Example**

```tsx
// app/items/page.tsx
export default async function ItemsPage({
  searchParams,
}: {
  searchParams: Promise<{ page?: string; category?: string }>;
}) {
  const { page = '1', category = 'all' } = await searchParams;

  const res = await fetch(
    `https://api.example.com/items?page=${page}&category=${category}`
  );
  const items = await res.json();

  return (
    <div>
      <h2>Category: {category} (Page {page})</h2>
      <ul>{items.map((item: any) => <li key={item.id}>{item.name}</li>)}</ul>
    </div>
  );
}

```

**Output**
Navigating to `/items?page=2&category=electronics` fetches page 2 of electronics items on the server and renders the matching HTML.

---

## Question 58: How do you implement RBAC (Role-Based Access Control)?
RBAC is enforced on the server using Edge Proxy (`proxy.ts`) and server-side role validation. Request tokens or session claims are validated before rendering protected pages or serving Route Handlers. Client-side role checks are used solely to update the UI (e.g., hiding buttons) and are not relied upon for security.

**Workflow/Architecture**

```text
Incoming Request
       │
       ▼
[ proxy.ts Interceptor ] ──► Extract Auth Token & Role
       │
       ├── Role Authorized ──► Render Protected Route
       └── Role Denied     ──► Redirect to 403 / Forbidden Page

```

**Example**

```typescript
// proxy.ts
import { NextResponse } from 'next/server';
import type { NextRequest } from 'next/server';

export function proxy(request: NextRequest) {
  const userRole = request.cookies.get('user_role')?.value;

  if (request.nextUrl.pathname.startsWith('/admin') && userRole !== 'ADMIN') {
    return NextResponse.redirect(new URL('/unauthorized', request.url));
  }

  return NextResponse.next();
}

```

**Output**
Non-admin users accessing `/admin` are intercepted at the network edge and redirected instantly to `/unauthorized`.

---

## Question 59: How do you design scalable routing?
Scalable routing is designed using nested layouts, route groups `(groupName)` to structure logical modules without affecting URL paths, and dynamic parameters. Reusing shared sub-layouts across related pages eliminates code duplication and maintains structural consistency as applications grow.

**Workflow/Architecture**

```text
app/
  ├── (marketing)/         --> Route Group (No URL segment impact)
  │   ├── layout.tsx       --> Marketing shell layout
  │   └── page.tsx         --> Route: /
  └── (dashboard)/         --> Route Group
      ├── layout.tsx       --> Dashboard sidebar layout
      └── analytics/
          └── page.tsx     --> Route: /analytics

```

**Example**

```tsx
// app/(marketing)/about/page.tsx
export default function AboutPage() {
  return <h1>Public About Page</h1>;
}

```

**Output**
Maps clean public URLs (`/about`, `/analytics`) while separating marketing and dashboard route logic into distinct folder structures.

---

## Question 60: How would you build a dashboard with auth and server rendering?
Build the dashboard core using async Server Components to fetch data securely on the server. Enforce authentication and session validation using Edge Proxy (`proxy.ts`) to block unauthorized access prior to rendering. Restrict Client Components strictly to interactive components such as dynamic charts or controls.

**Workflow/Architecture**

```text
User Request /dashboard
       │
       ▼
[ proxy.ts Checks Auth Session ]
       │ (Valid)
       ▼
[ Server Component Fetches DB Data ] ──► [ Client Chart Component Hydrates ]

```

**Example**

```tsx
// app/dashboard/page.tsx
import { cookies } from 'next/headers';
import AnalyticsChart from './AnalyticsChart'; // Client Component

export default async function Dashboard() {
  const cookieStore = await cookies();
  const token = cookieStore.get('token');

  const res = await fetch('https://api.example.com/analytics', {
    headers: { Authorization: `Bearer ${token?.value}` },
  });
  const data = await res.json();

  return (
    <div>
      <h1>Dashboard Metrics</h1>
      <AnalyticsChart chartData={data} />
    </div>
  );
}

```

**Output**
Data fetching happens securely on the server, delivering pre-rendered dashboard markup to the browser, where `AnalyticsChart` hydrates for interactivity.

---

## Question 61: What are common mistakes that cause large client bundles?
Common bundle bloat mistakes include adding the `'use client'` directive unnecessarily at high levels of the component tree, importing server-only modules or heavy Node libraries into Client Components, and importing un-treeshaken library utilities.

**Example**

*Mistake (Large Client Bundle):*

```tsx
'use client'; // Marked client at page root!

import { heavyUtility } from 'large-library'; // Entire library bundled to browser!

export default function Page() {
  return <div>{heavyUtility()}</div>;
}

```

*Solution (Isolated Client Boundary):*

```tsx
// app/page.tsx (Server Component by default)
import HeavyWidget from './HeavyWidget';

export default function Page() {
  return <HeavyWidget />;
}

```

**Output**
Isolating Client Components reduces client JavaScript bundle sizes dramatically.

---

## Question 62: How do you pass data from Server Components to Client Components?
Data is passed from Server Components to Client Components as React props. All props passed across the server-client boundary must be JSON-serializable (strings, numbers, booleans, plain objects, arrays). Functions or non-serializable objects cannot be passed.

**Example**

*Server Component (`app/page.tsx`):*

```tsx
import ClientUserCard from './ClientUserCard';

export default async function Page() {
  const userData = { id: 101, name: 'Alex', role: 'Developer' };

  return <ClientUserCard user={userData} />;
}

```

*Client Component (`app/ClientUserCard.tsx`):*

```tsx
'use client';

export default function ClientUserCard({ user }: { user: { name: string; role: string } }) {
  return (
    <div onClick={() => alert(`Selected ${user.name}`)}>
      <h3>{user.name}</h3>
      <p>{user.role}</p>
    </div>
  );
}

```

**Output**
The server serializes `userData` into props, allowing `ClientUserCard` to hydrate and handle click interactions in the browser.

---

## Question 63: What triggers the need for a Client Component?
A Client Component is required when a component depends on interactive browser runtime features:

* React hooks (`useState`, `useEffect`, `useReducer`, `useContext`).
* DOM event handlers (`onClick`, `onChange`, `onSubmit`).
* Browser-only APIs (`window`, `document`, `localStorage`, `sessionStorage`).
* Custom client-side animation or browser hooks.

**Example**

```tsx
'use client';

import { useEffect, useState } from 'react';

export default function LocalStorageViewer() {
  const [value, setValue] = useState<string | null>(null);

  useEffect(() => {
    // Requires browser window API
    setValue(localStorage.getItem('theme'));
  }, []);

  return <div>Current Theme: {value ?? 'Default'}</div>;
}

```

**Output**
Executes in the browser environment safely after hydration without throwing server rendering errors.

---

## Question 64: What is a Server Component and when should you prefer it?
A Server Component is a React component that executes exclusively on the server and ships zero JavaScript code to the client bundle. Prefer Server Components as the default choice for data fetching, reading files or databases, accessing server secrets, and rendering static layout markup.

**Example**

```tsx
// app/posts/page.tsx (Server Component)
import fs from 'fs/promises';

export default async function PostsPage() {
  const fileData = await fs.readFile('./posts.json', 'utf-8');
  const posts = JSON.parse(fileData);

  return (
    <div>
      {posts.map((post: { id: number; title: string }) => (
        <article key={post.id}><h2>{post.title}</h2></article>
      ))}
    </div>
  );
}

```

**Output**
Executes filesystem reads on the server and delivers pre-rendered HTML without sending component or Node.js logic to the browser.

---

## Quick Points & Summary

* **App Router File Conventions:** `page.tsx` defines route UI; `layout.tsx` preserves DOM state; `template.tsx` remounts on navigation; `loading.tsx` manages Suspense streaming; `error.tsx` acts as a Client Error Boundary; `not-found.tsx` handles missing paths.
* **Server-First Paradigm:** Components render on the server by default. Server Components reduce client JS bundle sizes and allow direct access to databases and backend secrets.
* **Client Boundary Guidance:** Use `'use client'` strictly when needing state (`useState`), effects (`useEffect`), event handlers (`onClick`), or browser APIs (`localStorage`). Keep Client Components pushed to the edges of your component tree.
* **Dynamic Parameters:** Dynamic route parameters (`params`) and query strings (`searchParams`) are asynchronous Promises in modern Next.js and must be awaited.
* **Edge Routing & Proxy:** Next.js 16 uses `proxy.ts` at the project root for edge request interception, auth redirects, and header rewrites.
* **Data Fetching & Caching Strategy:** Use async Server Components for data fetching. Balance data freshness against load latency using revalidation intervals or on-demand cache tags (`revalidateTag`).
* **Environment Variable Safety:** Variables prefixed with `NEXT_PUBLIC_` are bundled into client browser JavaScript. Unprefixed variables remain server-only to keep database keys and secrets secure.
* **Production Readiness:** Use `output: 'standalone'` for Docker container deployments and `output: 'export'` for static hosting environments. Enforce server-side authorization rather than relying on client UI checks.