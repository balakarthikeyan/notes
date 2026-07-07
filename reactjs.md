# React Setup Guide

## Install Dependencies
1. **Core libraries**
   ```bash
   npm install --save react react-dom
   ```
2. **Development tools**
   ```bash
   npm install --save-dev @babel/core @babel/preset-env @babel/preset-react \
   babel-loader css-loader html-webpack-plugin style-loader webpack webpack-cli webpack-dev-server
   ```

## Project Structure
Create an `app` folder with:
- `index.html`
- `index.js`
- `index.css`

---

## Example Entry File (`index.js`)
```javascript
const React = require('react');
const ReactDOM = require('react-dom');
```

Alternatively, create a new project using:
```bash
npx create-react-app my-react-19
```

---

## DOM vs Virtual DOM

- **DOM (Document Object Model):**  
  A tree-like representation of HTML in the browser. Updating it directly can be expensive because the browser must recalculate layout and repaint.

  When you change the DOM in JavaScript — say, by updating a `<div>` textContent or changing a style — the browser:

  - Locates the element in its internal structure
  - Recalculates layout and paint (expensive)
  - Triggers reflows

- **Virtual DOM:**  
  An in-memory lightweight copy of the DOM. React compares the new Virtual DOM tree with the previous one (diffing) and updates only the parts of the real DOM that changed.

  - React builds an entire shadow tree just to compare it to the last one.
  - The Virtual DOM is a cheap structure in memory (no layout, styles, or rendering)
  - Comparing two virtual trees is much faster than modifying the DOM unnecessarily
  - The real DOM is touched only when needed, and only the parts that changed

**Why React is fast:**
- Avoids unnecessary direct DOM manipulation.  
- Uses Virtual DOM for efficient updates.  
- Applies changes only where needed.  
- Treats components as pure functions of state.

---

## Higher-Order Components (HOCs)

- **Definition:** A Higher-Order Component is a function that takes a component and returns a new one with added functionality.  
- **Purpose:** Reuse logic across multiple components without duplicating code.  
- **Note:** HOCs are not part of React’s API but a design pattern.

**Example:**
```javascript
function withAuth(WrappedComponent) {
  return function AuthComponent(props) {
    const isLoggedIn = Boolean(localStorage.getItem("token"));
    if (!isLoggedIn) {
      return <div>Please log in</div>;
    }
    return <WrappedComponent {...props} />;
  };
}

function Dashboard(props) {
  return <h1>Welcome {props.user}</h1>;
}

const ProtectedDashboard = withAuth(Dashboard);
```

---

## Hooks

- **Definition:** Hooks are functions provided by React to add features like state, effects, and refs to function components.  
- **Why Hooks:** They simplify logic reuse and replace older patterns like HOCs and render props.

**Example (useEffect):**
```javascript
useEffect(() => {
  // Your code here
}, []);
```

---

## Lifecycle of Function Components

Function components don’t persist like class instances; React re-invokes them with new inputs. Their lifecycle can be divided into:

1. **Mount** — Component appears on screen.  
   - `useEffect` runs (with empty dependency array).  
2. **Update** — Component re-renders due to state/prop changes.  
   - `useEffect` runs when dependencies change.  
3. **Unmount** — Component is removed.  
   - Cleanup function runs.

**Visual Flow:**
```
[Mount]
   ↓
useEffect runs (empty deps)
   ↓
[Update]
   ↓
useEffect runs (on dependency change)
   ↓
[Unmount]
   ↓
Cleanup function runs
```

---

**To upgrade your React project from version 16 to the latest React 19, you’ll need to update dependencies step by step (first to React 18.3, then to React 19). The biggest changes include new Server Components, the Actions API for form handling, new hooks like `useActionState`, `useFormStatus`, and `useOptimistic`, plus deprecations of older lifecycle methods.**  

## 🔧 Upgrade Steps

1. **Update React and React DOM**
   ```bash
   npm install react@18.3 react-dom@18.3
   ```
   - This transitional version helps identify deprecated APIs before moving to React 19.
   - Fix warnings and replace deprecated lifecycle methods (`componentWillMount`, `componentWillReceiveProps`, etc.).

2. **Upgrade to React 19**
   ```bash
   npm install react@19 react-dom@19
   ```
   - Ensure `react-scripts` is updated (React 19 requires newer tooling).
   ```bash
   npm install react-scripts@latest
   ```

3. **Update Material-UI**
   - Your current version (`@material-ui/core@4.x`) is outdated. Upgrade to **MUI v5**:
   ```bash
   npm install @mui/material @emotion/react @emotion/styled
   ```

## 🚀 Major Changes: React 16 → React 19

| Version | Key Features | Impact |
|---------|--------------|--------|
| **React 16 (2017–2020)** | Fiber architecture, Error Boundaries, Fragments, Context API, Hooks (introduced in 16.8) | Foundation for modern React apps |
| **React 17 (2020)** | No new features, focus on gradual upgrades | Easier migration |
| **React 18 (2022)** | Concurrent Rendering and createRoot API (replacing ReactDOM.render), Automatic Batching, `useTransition`, `useDeferredValue`, Suspense improvements | Performance boost |
| **React 19 (2024)** | **Server Components**, **Actions API**, new hooks (`useActionState`, `useFormStatus`, `useOptimistic`), `use` API for async data, stricter deprecations and improved hooks | Simplifies server/client boundaries, better form handling, modernized patterns |

---

1. **ReactDOM API Update**
   ```js
   import { createRoot } from 'react-dom/client';
   import App from './App';

   const root = createRoot(document.getElementById('root'));
   root.render(<App />);
   ```

2. **Material-UI → MUI v5**
   - Replace imports:
     ```js
     Old
     import { Button } from '@material-ui/core';

     New
     import { Button } from '@mui/material';
     ```
   - Icons:
     ```js
     import { Home } from '@mui/icons-material';
     ```

3. **React 19 Features**
   - **Actions API** for forms:
     ```js
     function MyForm() {
       async function action(formData) {
         handle submission
       }
       return <form action={action}>...</form>;
     }
     ```
   - New hooks:
     - `useActionState`
     - `useFormStatus`
     - `useOptimistic`
   - `use` API for async data inside components.

4. **Deprecations**
   - Remove legacy lifecycle methods (`componentWillMount`, `componentWillReceiveProps`, `componentWillUpdate`) .
   - Replace `ReactDOM.render` with `createRoot`.
   - String refs → use `React.createRef()` or callback refs.
---

.eslintrc.json

```json
{
  "root": true,
  "env": {
    "browser": true,
    "es2021": true,
    "node": true
  },
  "extends": [
    "react-app",
    "react-app/jest",
    "eslint:recommended",
    "plugin:react/recommended",
    "plugin:react-hooks/recommended",
    "plugin:jsx-a11y/recommended",
    "plugin:prettier/recommended"
  ],
  "plugins": ["react", "react-hooks", "jsx-a11y"],
  "rules": {
    "react/prop-types": "off",          Not needed if using TypeScript or modern patterns
    "react/react-in-jsx-scope": "off",  React 17+ doesn’t require explicit import
    "no-unused-vars": ["warn", { "argsIgnorePattern": "^_" }],
    "prettier/prettier": ["error", { "endOfLine": "auto" }]
  },
  "settings": {
    "react": {
      "version": "detect"
    }
  }
}
```
npm install --save-dev eslint prettier eslint-plugin-react eslint-plugin-react-hooks eslint-plugin-jsx-a11y eslint-plugin-prettier eslint-config-prettier
npm install --save-dev prettier

npm run lint

.prettierrc

```json
{
  "semi": true,
  "singleQuote": true,
  "trailingComma": "es5",
  "printWidth": 100,
  "tabWidth": 2,
  "useTabs": false,
  "bracketSpacing": true,
  "arrowParens": "always",
  "endOfLine": "auto"
}
```

- semi: true → always add semicolons.
- singleQuote: true → use 'single quotes' instead of "double quotes".
- trailingComma: "es5" → adds trailing commas where valid in ES5 (objects, arrays).
- printWidth: 100 → wraps lines at 100 characters for readability.
- tabWidth: 2 → two spaces per indentation level.
- arrowParens: "always" → (x) => x instead of x => x.
- endOfLine: "auto" → avoids cross‑platform line ending issues.

- Optionally add a script to package.json:
```json
"scripts": {
  "format": "prettier --write \"src/**/*.{js,jsx,ts,tsx,json,css,md}\""
}
```

• 	React Router v5.1.2 is outdated: It was released in late 2019 and primarily supports React 16/17.
• 	React 19 introduces breaking changes: New APIs (Actions, Suspense improvements, static DOM APIs) require libraries to adapt. React Router v5 does not support these features.

Current stable React Router versions:
• 	v6.x (widely used, stable, compatible with React 18 and 19).
• 	v7.x (latest, designed to bridge React 18 → 19 smoothly, with type safety and streaming support).

- Switch → Routes
In v6+, Switch was replaced with Routes.
- component / render props → element
Instead of passing a component via component={Home}, you now pass JSX via element={<Home />} eg. Replace component={...} → element={<... />}.
- Catch-all route
Instead of path="*" render={...}, you use path="*" with element={<NotFound />}.

- Replace props.history.push → useNavigate().
In v6+, props.history.push no longer works, route navigation is done with the useNavigate hook
- Wrap nested routes in <Routes> instead of using <Route> directly.
In v6+, you cannot use <Route> directly like that. Routes must be wrapped inside a <Routes> component.

- In v5: props.match.params.id was passed automatically to route components.
- In v6/v7: Route components don’t get props.match. Instead, you call useParams() inside the component to access route parameters.

- <Outlet/>: Acts as a placeholder where child routes render.

- withRouter is gone in v6. You can’t use `this.props.history.push`. Instead, you use the `useNavigate` hook. For class components, you need a wrapper HOC that injects navigate.

- Route props (render, component, exact) are gone. In v6, you must use element={<Component />}.
- PrivateRoute pattern changes. In v6, you don’t wrap Route with a custom component; instead, you create a wrapper that either returns <Outlet /> or <Navigate />.

- Used PrivateRoute with <Outlet /> for nested protected routes.
- Used navigate instead of history.push.

1. Declarative Syntax: React uses a declarative approach to define UI components, making it easier to understand and maintain code.
2. Component Reusability: React encourages the creation of reusable components, allowing developers to build complex UIs from smaller, modular pieces.
3. Virtual DOM: React uses a virtual DOM (Document Object Model) to optimize rendering performance, resulting in faster updates and smoother user experiences.
4. Strong Ecosystem: React has a vast ecosystem of libraries, tools, and community support, making it easy to integrate with other technologies and solve common development challenges.
5. JSX: React uses JSX (JavaScript XML) syntax, which allows developers to write HTML-like code directly within JavaScript, simplifying the process of building UI components.

npx create-react-app my-react-app

1. Components
Components can be functional or class-based. For beginners, functional components are easier to understand and use.

Example:
function Welcome() {
  return <h1>Hello, React!</h1>;
}

2. JSX
JSX (JavaScript XML) is a syntax extension for JavaScript that allows you to write HTML-like code inside your JavaScript.

Example:
const element = <h1>Welcome to React!</h1>;

3. Props and State
Props: Short for properties, props are used to pass data to components.
State: State is used to manage data within a component.

Example of Props:

function Greeting(props) {
  return <h1>Hello, {props.name}!</h1>;
}

<Greeting name="Alice" />

Example of State:

import { useState } from 'react';

function Counter() {
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>
        Click me
      </button>
    </div>
  );
}

Essential React Concepts to Learn
Component Lifecycle: Learn how components are mounted, updated, and unmounted.
Hooks: Understand hooks like useState, useEffect, and useContext to manage state and side effects.
Routing: Use React Router for navigation between pages.
Styling: Experiment with CSS, CSS-in-JS, or libraries like Tailwind CSS.


1. Automatic Batching for All Updates
React 19 improves performance by batching state updates automatically, even in event handlers, async functions, or promises.

function App() {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  const handleClick = () => {
    setCount(count + 1);
    setText('Updated');
  };

  return (
    <div>
      <p>{count}</p>
      <p>{text}</p>
      <button onClick={handleClick}>Update</button>
    </div>
  );
}

2. Streaming SSR with Suspense
React 19 introduces streaming server-side rendering (SSR), enabling faster load times and better SEO.

3. New Concurrent Features
Concurrent rendering enhances app responsiveness by prioritizing tasks dynamically.

4. Adding Forms in React 19
React 19 not use state for form data you can direact use formData.entries for form data

5. Client-Side Transitions
React 19 supports seamless client-side transitions for better user experience.

6. Improved Suspense API
Suspense now supports more use cases, making it easier to handle asynchronous data fetching.

7. Custom Cache Implementation
The new Cache API lets developers create custom caching strategies for their applications.

8. Enhanced Error Boundaries
React 19 improves error handling with better error boundaries and fallback rendering.

9. useTransition Hook
This hook helps in building smoother UI transitions.

const [isPending, startTransition] = useTransition();

startTransition(() => {
  setState(newState);
});
10. Reduced Bundle Size
React 19 optimizes the bundle size, leading to faster load times.

11. Strict Mode Enhancements
Strict Mode now highlights more potential issues in your app.

12. Support for Web Workers
React 19 adds native support for Web Workers, improving performance for compute-intensive tasks.

13. Improved Lazy Loading
The React.lazy API now supports better error handling and fallback mechanisms.

14. Event Delegation Optimizations
Event handling is more efficient in React 19, improving app responsiveness.

15. React Server Components
React Server Components allow rendering parts of a component tree on the server, reducing the client-side JavaScript needed.

16. Concurrent Suspense
Suspense now works seamlessly with concurrent rendering, making async operations smoother.

17. Improved Context API
The Context API gets better performance and usability updates in React 19.

18. New JSX Transform
The JSX Transform eliminates the need to import React at the top of every file.

function App() {
  return <h1>Hello, React 19!</h1>;
}
19. Improved Tree-Shaking
React 19 enhances tree-shaking, allowing unused code to be eliminated more effectively.

SERVER COMPONENTS : server component enables specific aspects of your react app to be rendered on the server thereby reducing the workload on the client and allows for pages to load faster. Prior to react 19 server components weren’t supported natively and developer had to adapt frameworks like Next.js. With this update, the process of rendering part of your application on the server is simplified.

Implementation on React 18:

import { getServerSideProps } from 'next';

export default function Page({ data }) {
  return <div>{data}</div>;
}

Update using React 19

"use server";

export async function getData() {
  const res = await fetch('https://jsonplaceholder.typicode.com/posts');
  return res.json();
}

export default function Page() {
  const posts = getData();
  return <div>{posts}</div>;
}

REACT COMPILER : react 19 compiler optimizes re-renders by handing memorization, eliminating the need for manual optimization technique by using “ useMemo” and “ useCallback ”. This update ensures a more cleaner and maintainable codebase.
Implementation on React 18:

const product = useMemo(() => multiply(num1, num2), [num1, num2]);

Update using React 19

const product = multiply(num1, num2);
From this example we can see that the “useMemo ” hook was removed and we directly called the “multiply” function. The optimization is handled by the react complier and ensures that the multiply function is not re-run needlessly.

REF AS A PROP : handling ref is now simplified with react 19 as ref can now be used as a regular prop, instead of using forwardRef for passing refs to the child components.
Implementation on React 18:

import React, { forwardRef } from 'react';

const Input = forwardRef((props, ref) => (
  <input ref={ref} {...props} />
));


Update using React 19

function Input({ ref, ...props }) {
  return <input ref={ref} {...props} />;
}
DOCUMENT METADETA MANAGEMENT : unlike in react 18 where developer use libraries like “react-helmet” for managing metadata, this update comes with a built-in<DocumentHead>` component to manage document metadata directly within the react components without any additional dependencies and setup.
Implementation on React 18:
import { Helmet } from 'react-helmet';

function HomePage() {
  return (
    <>
      <Helmet>
        <title>Home Page</title>
        <meta name="description" content="This is the home page" />
      </Helmet>
      
    </>
  );
}

Update using React 19
import { DocumentHead } from 'react';

function ContactUs() {
  return (
    <>
      <DocumentHead>
        <title>Contact us page</title>
        <meta name="description" content="This is the contact page" />
      </DocumentHead>
     
    </>
  );
}
ENHANCED ASSET LOADING : this update result in a more seamless user experience by loading assests like images or script in the background while user still views the current page. This process make it faster for user to navigate to new pages and reduces the need for writing additional code.
Implementation on React 18:
import { useEffect } from 'react';

function HomePage() {
  useEffect(() => {
    const img = new Image();
    img.src = '/path/to/image.jpg';
  }, []);

  return <div>Loading...</div>;
}

Update using React 19
function HomePage() {
  return (
    <div>
      <img src="/path/to/image.jpg" loading="lazy" />
    </div>
  );
}
IMPROVED REACT HOOKS : react 19 provides new hooks for managing state and asynchronous operations like `use() `, ` useFormStatus` and ` useOptimistic`. The new use hook can be used in place of the useEffect and useContext to resolve promises or context.
Implementation on React 18:

useEffect(() => {
    async function fetchData() {
      const res = await fetch('/api/data');
      const result = await res.json();
      setData(result);
    }
    fetchData();
  }, []);

Update using React 19

function DataFetchingComponent() {
  const data = use(async () => {
    const res = await fetch('/api/data');
    return res.json();
  });

Feature-Sliced Design Architecture in React with TypeScript

Feature-Sliced Design (FSD) is a modern architectural methodology that addresses these challenges by providing a standardized approach to organizing frontend code.

Standardization: A unified structure that any developer can understand
Controlled reusability: Clear rules about what can depend on what
Separation of concerns: Business logic separated from UI and technical details
Scalability: Structure that grows naturally with your application

The Core Principles
1. Layered Architecture
FSD organizes code into layers with strict dependency rules. Each layer has a specific purpose and can only depend on layers below it.

2. Slices Within Layers
Within most layers, code is further organized into slices — isolated modules that contain all the logic for a specific feature or entity.

3. Segments Within Slices
Each slice is divided into segments — standardized folders that group code by technical purpose (UI, API, model, lib, config).