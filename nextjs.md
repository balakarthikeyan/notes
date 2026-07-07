# Next.Js

Next.JS is a React framework developed by Vercel that enables several extra features, including server-side rendering and generating static SEO friendly websites. We would be using 11.1.0 as our main framework.
Vercel is a platform for static sites and frontend frameworks, built to integrate with your headless content, commerce, or database.
Contentful is the platform where you can update the content of your website, a mobile app or any other platform that displays content.

## How to setup Next.Js

`npx create-next-app@latest nextjs-blog`

```bash
nextjs-blog/
├── public/              # Static assets (images, etc.)
├── pages/               # All your routes live here
│   ├── index.js         # Homepage
│   ├── about.js         # Sample About Page
│   └── blog/            # Dynamic blog routes
├── components/          # Reusable components
├── styles/              # Tailwind config, global styles
├── utils/               # Helper functions, hooks (e.g., SWR)
├── data/                # Sample static blog data
├── tailwind.config.js   # Tailwind configuration
└── next.config.js       # Next.js configuration
```

Key features of Next.js include server-side rendering (SSR), static site generation (SSG), client-side routing, automatic code splitting, and an intuitive file-based routing system. These features enable developers to create fast, SEO-friendly web applications with minimal configuration. 

2. Key Features of Next.js 

<ol type="a"> 
<li>Server-Side Rendering (SSR)

One of Next.js standout features is SSR, which allows rendering React components on the server instead of the client. This results in faster initial page loads and better SEO since search engines can crawl the fully rendered HTML. </li>

<li>Static Site Generation (SSG)

Next.js offers SSG, which pre-renders pages at build time. This approach generates static HTML files for each page, resulting in incredibly fast page loads and enabling easy deployment to content delivery networks (CDNs). </li>

<li>Client-Side Routing

Next.js provides a built-in client-side routing system. You can create dynamic, single-page applications (SPAs) without the need for complex routing configurations. </li>

<li>Automatic Code Splitting

Next.js automatically splits your JavaScript code into smaller chunks, ensuring that users only download the code necessary for the current page. This optimizes load times and performance. </li>

<li>File-Based Routing

Next.js simplifies routing by allowing developers to create pages using a file-based system. Simply create a JavaScript file in the "pages" directory, and Next.js handles the routing for you.
</li>

</ol>

3. Explore the Project Structure 

Next.js projects have a predefined project structure. Key directories include: 

- `pages:` Contains your application's pages. File-based routing is based on this directory's structure. 

- `public:` Stores static assets like images, stylesheets, and fonts. 

- `styles:` Holds global CSS styles for your application. 

4. Pages and Routing 

In Next.js, creating pages is straightforward. Each JavaScript file in the pages directory corresponds to a route. For example: 

- `pages/index.js` corresponds to the root route `/`. 

- `pages/about.js` corresponds to the `/about` route. 

To create dynamic routes, you can use brackets in the filename, like [slug].js, to handle route parameters. 

5. Data Fetching 

Next.js offers multiple ways to fetch data for your pages, including: 

- `getStaticProps:` Fetches data at build time (SSG). 

- `getServerSideProps:` Fetches data on each request (SSR). 

- `getInitialProps (deprecated):` Used for data fetching in older Next.js projects. 

Choose the appropriate method based on your application's requirements. 

6. Styling in Next.js 

You can style your Next.js application using various methods, including CSS modules, styled-components, or traditional CSS files. Next.js allows you to choose the approach that best fits your project's needs. 

7. Optimizing Performance 

Next.js is designed with performance in mind, but there are additional steps you can take to optimize your application: 

Use the `next/image` component for responsive image loading. 

Implement client-side data fetching with libraries like SWR or React Query. 

Leverage automatic code splitting to reduce bundle sizes. 

8. Deployment with Vercel 

Vercel, the company behind Next.js, offers seamless deployment for Next.js applications. To deploy your Next.js app on Vercel: 

- Sign up for a Vercel account. 
- Link your project's GitHub repository. 
- Configure your deployment settings. 

Deploy your application with a single click. Vercel provides a global CDN, SSL support, and easy scaling, making it an excellent choice for hosting Next.js application.

## Commands

```bash
npm install @contentful/rich-text-react-renderer @contentful/rich-text-types
```

## gatsby

```bash
npm config get prefix -g
npm install -g gatsby-cli
gatsby -v
gatsby new hello-world https://www.github.com/gatsby/gatsby-starter-hello-world
```