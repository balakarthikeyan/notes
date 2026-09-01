# 1. What is React and Why Do We Need It?

**Answer**
React is a JavaScript library for building user interfaces using reusable, composable components. It focuses primarily on the UI layer of an application. Without React or a similar UI library, developers manipulate the browser DOM imperatively, managing manual DOM updates, event handling, application state synchronization, and component relationships. React introduces a declarative model where the UI is expressed as a function of state ($UI = f(State)$). Instead of issuing step-by-step DOM commands, you describe what the UI should look like for the current state, and React handles updating the real DOM efficiently.

**Legacy Example (Imperative Vanilla JS)**

```javascript
// Direct manual DOM manipulation
const button = document.getElementById("btn");
const message = document.getElementById("message");

button.addEventListener("click", function() {
    message.textContent = "Hello from Vanilla JS!";
});

```

**Modern Example (Declarative React Function Component)**

```jsx
import { useState } from "react";

export default function App() {
    const [message, setMessage] = useState("");

    return (
        <div>
            <button onClick={() => setMessage("Hello from React!")}>
                Click Me
            </button>
            {message && <p>{message}</p>}
        </div>
    );
}

```

**Output**

* Initial Render: Displays a `[ Click Me ]` button.
* After User Click: Renders `<p>Hello from React!</p>` beneath the button.

**Workflow / Architecture**

```text
State Changes
     │
     ▼
React Engine (Calculates UI representation)
     │
     ▼
Reconciliation / Virtual DOM Diffing
     │
     ▼
Minimal Host DOM Updates

```

---

# 2. What is Imperative vs. Declarative Programming in React?

**Answer**
Imperative programming specifies the explicit step-by-step instructions required to achieve a result. Declarative programming describes the desired target state, leaving the step-by-step execution details to the underlying library or engine. React uses a declarative paradigm.

**Legacy / Imperative Example**

```javascript
// Imperative: Explicitly searching DOM nodes and mutating properties step-by-step
const container = document.getElementById("container");
const statusText = document.createElement("p");
statusText.textContent = "Status: Inactive";
container.appendChild(statusText);

function toggleStatus() {
    if (statusText.textContent === "Status: Inactive") {
        statusText.textContent = "Status: Active";
    } else {
        statusText.textContent = "Status: Inactive";
    }
}

```

**Modern / Declarative Example**

```jsx
import { useState } from "react";

export default function StatusToggle() {
    const [isActive, setIsActive] = useState(false);

    return (
        <div>
            <button onClick={() => setIsActive(prev => !prev)}>
                Toggle Status
            </button>
            <p>Status: {isActive ? "Active" : "Inactive"}</p>
        </div>
    );
}

```

**Output**

* Initial Render: Button `[ Toggle Status ]` and text `Status: Inactive`.
* After Click: Text automatically updates to `Status: Active`.

---

# 3. What is Component-Based Architecture and Composition vs. Inheritance?

**Answer**
React applications are composed of independent, isolated, and reusable component blocks arranged in a tree hierarchy. React strongly advocates for **Composition over Inheritance** to reuse UI structure and behavior. Rather than extending base component classes, components encapsulate children or pass props to structure complex UIs flexibly.

**Legacy Example (Prototypal/Class Inheritance Mental Model - Anti-pattern in React)**

```jsx
import React from "react";

// Discouraged in React: Class inheritance
class BaseCard extends React.Component {
    renderCardHeader() {
        return <div className="header">Card Header</div>;
    }
}

class ProductCard extends BaseCard {
    render() {
        return (
            <div className="card">
                {this.renderCardHeader()}
                <p>Product Details</p>
            </div>
        );
    }
}

```

**Modern Example (Composition with Children Prop)**

```jsx
function Card({ children }) {
    return <div className="card-container" style={{ border: "1px solid #ccc", padding: "16px" }}>{children}</div>;
}

export default function ProductCard() {
    return (
        <Card>
            <h2>Product Name</h2>
            <p>Price: $99</p>
        </Card>
    );
}

```

**Output**

```text
┌──────────────────────────┐
│ Product Name             │
│ Price: $99               │
└──────────────────────────┘

```

---

# 4. What is NPM and How is It Used in React?

**Answer**
NPM (Node Package Manager) is the primary registry and CLI tool used to install, update, and manage JavaScript/Node.js dependencies and project scripts in React applications.

**Example**
Command terminal install:

```bash
npm install react react-dom

```

Generated `package.json` manifest:

```json
{
  "name": "react-app",
  "version": "1.0.0",
  "dependencies": {
    "react": "^19.0.0",
    "react-dom": "^19.0.0"
  }
}

```

**Output**
Creates the `node_modules/` directory containing module binaries and locking precise sub-dependency trees in `package-lock.json`.

---

# 5. What is the Difference Between React Elements and React Components?

**Answer**
A **React Element** is a lightweight, immutable plain JavaScript object describing a DOM node or UI structure (its type and props). A **React Component** is a function or class that accepts props as input and returns a React element tree.

**Example**

```jsx
import React from "react";

// 1. React Element (Plain object creation)
const element = <h1 className="title">I am an Element</h1>;

// Conceptually translates into:
// const element = React.createElement('h1', { className: 'title' }, 'I am an Element');

// 2. React Component (Function producing elements)
function WelcomeComponent({ name }) {
    return <h1>Hello, {name}</h1>;
}

export default function App() {
    return (
        <div>
            {element}
            <WelcomeComponent name="Alice" />
        </div>
    );
}

```

**Output**

```text
I am an Element
Hello, Alice

```

---

# 6. What is JSX, JSX Expressions, and JSX Attributes?

**Answer**
JSX (JavaScript XML) is an HTML-like syntax extension for JavaScript that lets developers write UI structure directly inside JavaScript code. Transpilers (like Babel, SWC, or Vite) transform JSX into standard JavaScript function calls. Dynamic JavaScript expressions are embedded within `{}` curly braces. Attributes use camelCase naming conventions (e.g., `className`, `htmlFor`, `onClick`).

**Example**

```jsx
export default function UserCard() {
    const username = "Jane Doe";
    const avatarUrl = "https://via.placeholder.com/150";
    const isAdmin = true;

    return (
        <div className="user-card">
            <img src={avatarUrl} alt={username} />
            <h2>{username.toUpperCase()}</h2>
            <p>Role: {isAdmin ? "Administrator" : "Standard User"}</p>
        </div>
    );
}

```

**Output**
Displays an image with alt `Jane Doe`, heading `JANE DOE`, and text `Role: Administrator`.

---

# 7. What Are Props, Why Are They Read-Only, and What is One-Way Data Flow?

**Answer**
**Props** (properties) are arbitrary input parameters passed down from a parent component to a child component. React relies on a **One-Way Data Flow** (unidirectional), meaning data flows down from top to bottom. Props are strictly **read-only** (immutable) inside the receiving component to ensure predictable rendering and maintain pure rendering principles. If a child needs to inform a parent of an event, the parent passes a callback function down as a prop.

**Example**

```jsx
// Child Component
function ActionButton({ label, onClick }) {
    // DO NOT MUTATE: label = "New Label"; // Error! Props are read-only
    return <button onClick={onClick}>{label}</button>;
}

// Parent Component
export default function Parent() {
    const handleNotification = () => alert("Triggered from Child!");

    return <ActionButton label="Click Me" onClick={handleNotification} />;
}

```

**Output**
Renders a button labeled `Click Me`. Clicking it fires an alert displaying `"Triggered from Child!"`.

**Workflow**

```text
Parent (State/Handler)
       │
       │ Props (Data / Callback)
       ▼
Child Component (Renders UI / Triggers Callback)

```

---

# 8. How Do You Render Lists and Why Is the `key` Prop Required?

**Answer**
Lists of elements are rendered by mapping array items to JSX components using `Array.prototype.map()`. The `key` prop is a special string attribute that gives elements within an array a stable identity across re-renders. React uses keys during the Virtual DOM diffing process (reconciliation) to identify which items have been added, moved, updated, or removed. Keys must be unique among siblings and should rely on stable database IDs rather than array index numbers.

**Example**

```jsx
export default function UserList() {
    const users = [
        { id: "usr_101", name: "Alice" },
        { id: "usr_102", name: "Bob" },
        { id: "usr_103", name: "Charlie" }
    ];

    return (
        <ul>
            {users.map(user => (
                <li key={user.id}>{user.name}</li>
            ))}
        </ul>
    );
}

```

**Output**

* Alice
* Bob
* Charlie

---

# 9. What is React State and How Does It Cause Re-renders?

**Answer**
State is a component-managed data object that can change over time based on user interactions, API network responses, or system events. When state changes via a state setter function, React schedules a component re-render, reconciles the Virtual DOM, and updates the host browser DOM to match the new state.

**Legacy Example (Class State)**

```jsx
import React from "react";

class CounterClass extends React.Component {
    constructor(props) {
        super(props);
        this.state = { count: 0 };
    }

    render() {
        return (
            <button onClick={() => this.setState({ count: this.state.count + 1 })}>
                Class Count: {this.state.count}
            </button>
        );
    }
}
export default CounterClass;

```

**Modern Example (Function Component with `useState`)**

```jsx
import { useState } from "react";

export default function CounterFunction() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(prev => prev + 1)}>
            Hooks Count: {count}
        </button>
    );
}

```

**Output**
Displays a button starting at `Hooks Count: 0`. Clicking increments the state number sequentially.

---

# 10. How Do `this`, `call`, `apply`, `bind`, and Class Fields Work in React?

**Answer**
In legacy JavaScript class components, methods passed as event callbacks lose their `this` binding context due to standard JS dynamic scoping rules. To maintain access to instance properties (`this.state`, `this.setState`), methods had to be explicitly bound using `.bind(this)` in the constructor, or defined as class fields using ES6 arrow functions which lexically bind `this`. Modern functional React components eliminate `this` entirely.

**Legacy Class Example (`bind` vs Class Field)**

```jsx
import React from "react";

class BindingExample extends React.Component {
    constructor(props) {
        super(props);
        this.state = { text: "Hello" };
        // Manual binding requirement
        this.handleBindClick = this.handleBindClick.bind(this);
    }

    handleBindClick() {
        alert(this.state.text);
    }

    // Modern Class Field (Lexical Arrow Function)
    handleArrowClick = () => {
        alert(this.state.text);
    };

    render() {
        return (
            <div>
                <button onClick={this.handleBindClick}>Bind Click</button>
                <button onClick={this.handleArrowClick}>Arrow Click</button>
            </div>
        );
    }
}
export default BindingExample;

```

**JavaScript Call/Apply/Bind Context Example**

```javascript
const user = { name: "Sarah" };
function greet(greeting, punctuation) {
    return `${greeting} ${this.name}${punctuation}`;
}

// Call: Passes context + comma-separated arguments
console.log(greet.call(user, "Hello", "!")); // "Hello Sarah!"

// Apply: Passes context + array of arguments
console.log(greet.apply(user, ["Hi", "."])); // "Hi Sarah."

// Bind: Returns a new function with bound context
const boundGreet = greet.bind(user);
console.log(boundGreet("Hey", "?")); // "Hey Sarah?"

```

**Output**
Clicking either button in the legacy component correctly displays an alert box containing `"Hello"`.

---

# 11. What Are Pure Functions and Why Does Purity Matter in React?

**Answer**
A function is **pure** if it consistently returns the exact same output when given the exact same input arguments and produces zero side-effects (e.g., mutating global variables, performing network calls, or modifying DOM nodes during its execution). React assumes that component render functions are pure. Purity guarantees that UI rendering remains predictable, testable, and safe for concurrent multi-threaded rendering optimizations.

**Impure Example (Anti-pattern)**

```jsx
let guestCount = 0; // External mutated variable

function ImpureCup() {
    guestCount = guestCount + 1; // Side-effect during render!
    return <h2>Tea cup for guest #{guestCount}</h2>;
}

```

**Pure Example**

```jsx
function PureCup({ guestNumber }) {
    return <h2>Tea cup for guest #{guestNumber}</h2>;
}

export default function TeaParty() {
    return (
        <div>
            <PureCup guestNumber={1} />
            <PureCup guestNumber={2} />
        </div>
    );
}

```

**Output**

```text
Tea cup for guest #1
Tea cup for guest #2

```

---

# 12. What Are PropTypes and How Do They Compare to Modern TypeScript?

**Answer**
`PropTypes` is a legacy runtime validation mechanism used to check data types of component props in development. Modern React enterprise applications prefer **TypeScript** because it performs static type checking during compile-time, providing immediate IDE autocomplete errors without runtime performance overhead.

**Legacy Example (PropTypes)**

```jsx
import PropTypes from "prop-types";

function UserProfile({ name, age }) {
    return <div>{name} - {age} years old</div>;
}

UserProfile.propTypes = {
    name: PropTypes.string.isRequired,
    age: PropTypes.number.isRequired
};

```

**Modern Example (TypeScript Interface)**

```tsx
interface UserProfileProps {
    name: string;
    age: number;
}

export default function UserProfileTS({ name, age }: UserProfileProps) {
    return <div>{name} - {age} years old</div>;
}

```

**Output**

* Valid Props (`name="John"`, `age={30}`): Renders `John - 30 years old`.
* Invalid Props in PropTypes: Console warning in development browser tools.
* Invalid Props in TypeScript: Build-time compilation error in your code editor.

---

# 13. What is the React Component Lifecycle (Legacy Class vs. Modern Hooks Mapping)?

**Answer**
Components go through three primary lifecycle phases: **Mounting** (insertion into DOM), **Updating** (re-rendering due to state/prop changes), and **Unmounting** (removal from DOM). Legacy class components managed these phases via distinct lifecycle hooks. Functional components unify these phases into declarative synchronization effects using the `useEffect` hook.

**Legacy Class Lifecycle Example**

```jsx
import React from "react";

class LifecycleClass extends React.Component {
    componentDidMount() {
        console.log("Mounted to DOM");
    }

    componentDidUpdate(prevProps, prevState) {
        console.log("Component updated");
    }

    componentWillUnmount() {
        console.log("Cleanup before unmounting");
    }

    render() {
        return <div>Class Component Lifecycle</div>;
    }
}
export default LifecycleClass;

```

**Modern Hook Lifecycle Mapping**

```jsx
import { useEffect } from "react";

export default function LifecycleFunction() {
    useEffect(() => {
        console.log("Mounted / Equivalent to componentDidMount");

        return () => {
            console.log("Unmounted / Equivalent to componentWillUnmount");
        };
    }, []); // Empty dependency array = mount & unmount only

    return <div>Functional Component Lifecycle</div>;
}

```

**Output**
Console output tracking sequence:

1. `Mounted to DOM`
2. `Cleanup before unmounting` (upon removal)

**Lifecycle Mapping**

* Mounting: `componentDidMount` $\rightarrow$ `useEffect(() => {}, [])`
* Updating: `componentDidUpdate` $\rightarrow$ `useEffect(() => {}, [dependencies])`
* Unmounting: `componentWillUnmount` $\rightarrow$ `useEffect(() => { return () => cleanup() }, [])`

---

# 14. How Do You Handle API Requests Safely in React?

**Answer**
API data fetching is a side effect performed after a component mounts. Modern implementations require handling loading states, error boundaries, and cleanup mechanisms using `AbortController` to avoid race conditions and state updates on unmounted components.

**Example**

```jsx
import { useState, useEffect } from "react";

export default function DataFetcher() {
    const [data, setData] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    useEffect(() => {
        const controller = new AbortController();

        async function fetchData() {
            try {
                setLoading(true);
                const response = await fetch("https://jsonplaceholder.typicode.com/todos/1", {
                    signal: controller.signal
                });
                if (!response.ok) throw new Error("HTTP request failed");
                const result = await response.json();
                setData(result);
            } catch (err) {
                if (err.name !== "AbortError") {
                    setError(err.message);
                }
            } finally {
                setLoading(false);
            }
        }

        fetchData();

        return () => controller.abort(); // Cleanup / cancel fetch on unmount
    }, []);

    if (loading) return <p>Loading data...</p>;
    if (error) return <p>Error: {error}</p>;

    return <div>Title: {data?.title}</div>;
}

```

**Output**

* Initial: `Loading data...`
* Success: `Title: delectus aut autem`

---

# 15. How Do You Integrate React Icons?

**Answer**
Icon libraries (such as `react-icons`) expose vector icons (SVG) packaged directly as reusable React components, allowing for dynamic styling, accessibility attributes, and tree-shaking support.

**Example**

```jsx
import { FaSearch, FaUser } from "react-icons/fa";

export default function IconBar() {
    return (
        <div>
            <button aria-label="Search">
                <FaSearch style={{ color: "blue", marginRight: "8px" }} />
                Search
            </button>
            <FaUser style={{ fontSize: "24px", color: "green" }} />
        </div>
    );
}

```

**Output**
Renders a search button containing an inline blue search magnifying icon alongside a standalone green user icon.

---

# 16. What Is the Difference Between Controlled and Uncontrolled Components?

**Answer**

* **Controlled Component**: Input form state is completely driven by React local state via `value` and `onChange` handlers. React acts as the single source of truth.
* **Uncontrolled Component**: Input form state is managed natively by the browser DOM. React reads current values on demand using persistent references (`useRef` or `React.createRef`).

**Example**

```jsx
import { useState, useRef } from "react";

export default function FormsComparison() {
    // Controlled
    const [controlledVal, setControlledVal] = useState("");

    // Uncontrolled
    const inputRef = useRef(null);

    const handleUncontrolledSubmit = (e) => {
        e.preventDefault();
        alert(`Uncontrolled Value: ${inputRef.current.value}`);
    };

    return (
        <div>
            {/* Controlled Input */}
            <div>
                <h3>Controlled Input</h3>
                <input 
                    value={controlledVal} 
                    onChange={(e) => setControlledVal(e.target.value)} 
                />
                <p>Live State: {controlledVal}</p>
            </div>

            {/* Uncontrolled Input */}
            <form onSubmit={handleUncontrolledSubmit}>
                <h3>Uncontrolled Input</h3>
                <input ref={inputRef} defaultValue="Initial Text" />
                <button type="submit">Submit Ref</button>
            </form>
        </div>
    );
}

```

**Output**

* Controlled input reflects typed characters synchronously in the `<p>` paragraph below it.
* Uncontrolled input lets the browser manage input text natively until clicking "Submit Ref" triggers an alert box reading the current `inputRef.current.value`.

---

# 17. What is the `children` Prop and How Does It Support UI Composition?

**Answer**
`children` is a default prop present on every React component that contains the visual content or child components nested between its opening and closing JSX tags. It enables component composition by allowing generic wrapper layout components to accept arbitrary internal markup without needing to know its precise structure beforehand.

**Example**

```jsx
function ModalWrapper({ children, title }) {
    return (
        <div className="modal-backdrop">
            <div className="modal-box">
                <h3>{title}</h3>
                <div className="modal-content">{children}</div>
            </div>
        </div>
    );
}

export default function App() {
    return (
        <ModalWrapper title="Confirm Action">
            <p>Are you sure you want to delete this item?</p>
            <button>Delete</button>
        </ModalWrapper>
    );
}

```

**Output**
Renders a modal card with a heading "Confirm Action" wrapping the custom text paragraph and delete button passed into `{children}`.

---

# 18. How Do Default Props Work in Legacy vs. Modern React?

**Answer**
Default props specify fallback property values used whenever a parent component omits passing a specific prop. Legacy React used a static `defaultProps` property. Modern React function components use native ES6 default parameter assignments inside the function signature.

**Legacy Example (`defaultProps`)**

```jsx
function LegacyButton({ theme }) {
    return <button className={theme}>Legacy</button>;
}
LegacyButton.defaultProps = {
    theme: "primary"
};

```

**Modern Example (ES6 Default Parameters)**

```jsx
export default function ModernButton({ theme = "primary", text = "Click" }) {
    return <button className={`btn-${theme}`}>{text}</button>;
}

```

**Output**
Rendering `<ModernButton/>` yields `<button class="btn-primary">Click</button>`.

---

# 19. What is a Higher-Order Component (HOC)?

**Answer**
A Higher-Order Component (HOC) is a pure structural pattern where a function receives a component as an argument and returns an enhanced wrapper component. It allows cross-cutting logic (like authentication checks, logging, or state synchronization) to be reused across multiple components.

**Example**

```jsx
import React from "react";

// Higher-Order Component
function withLoading(WrappedComponent) {
    return function WithLoadingComponent({ isLoading, ...props }) {
        if (isLoading) return <p>Loading component...</p>;
        return <WrappedComponent {...props} />;
    };
}

// Target Component
function UserData({ name }) {
    return <h3>User: {name}</h3>;
}

// Enhanced Component
const UserDataWithLoading = withLoading(UserData);

export default function App() {
    return (
        <div>
            <UserDataWithLoading isLoading={true} name="Alice" />
            <UserDataWithLoading isLoading={false} name="Bob" />
        </div>
    );
}

```

**Output**

* First component renders: `<p>Loading component...</p>`
* Second component renders: `<h3>User: Bob</h3>`

---

# 20. What is the Render Props Pattern?

**Answer**
The Render Props pattern shares stateful behavior across components by providing a prop whose value is a function. The wrapper component executes this function, passing its internal state as arguments to determine what UI to render.

**Example**

```jsx
import { useState } from "react";

// Provider using render prop
function MouseTracker({ render }) {
    const [position, setPosition] = useState({ x: 0, y: 0 });

    const handleMouseMove = (e) => {
        setPosition({ x: e.clientX, y: e.clientY });
    };

    return (
        <div style={{ height: "100vh" }} onMouseMove={handleMouseMove}>
            {render(position)}
        </div>
    );
}

export default function App() {
    return (
        <MouseTracker 
            render={({ x, y }) => (
                <h1>Mouse position: ({x}, {y})</h1>
            )} 
        />
    );
}

```

**Output**
Displays a live heading `Mouse position: (X, Y)` updating continuously as the user moves their cursor over the screen.

---

# 21. What is Context API and How Does It Solve Prop Drilling?

**Answer**
The Context API provides a way to share stateful values globally across a component tree without manually passing props down through every intermediate component (a problem known as "prop drilling"). It consists of a Context object created via `createContext`, a `Provider` component to supply values, and a `useContext` hook to consume those values.

**Example**

```jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext("light");

function ThemeToggler() {
    const { theme, toggleTheme } = useContext(ThemeContext);
    return (
        <button onClick={toggleTheme}>
            Current Theme: {theme}
        </button>
    );
}

export default function App() {
    const [theme, setTheme] = useState("dark");
    const toggleTheme = () => setTheme(prev => prev === "light" ? "dark" : "light");

    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            <ThemeToggler />
        </ThemeContext.Provider>
    );
}

```

**Output**
Button displaying `Current Theme: dark`. Clicking toggles the theme string to `light`.

**Workflow**

```text
ThemeContext.Provider (Value: { theme, toggleTheme })
        │
        ├── Deep Child Tree (Skipping prop drilling)
        │
        ▼
ThemeToggler Component (useContext(ThemeContext))

```

---

# 22. How Does Client-Side Routing Work in React Router?

**Answer**
React Router enables dynamic client-side routing in single-page applications (SPAs). It intercept browser URL updates and conditionally swaps visible page components in the DOM without requesting full web page reloads from a web server.

**Example**

```jsx
import { BrowserRouter, Routes, Route, Link, Outlet } from "react-router";

function Layout() {
    return (
        <div>
            <nav>
                <Link to="/">Home</Link> | <Link to="/dashboard">Dashboard</Link>
            </nav>
            <hr />
            <Outlet /> {/* Renders nested route components */}
        </div>
    );
}

function Home() { return <h2>Home Page</h2>; }
function Dashboard() { return <h2>Dashboard Page</h2>; }

export default function App() {
    return (
        <BrowserRouter>
            <Routes>
                <Route path="/" element={<Layout />}>
                    <Route index element={<Home />} />
                    <Route path="dashboard" element={<Dashboard />} />
                </Route>
            </Routes>
        </BrowserRouter>
    );
}

```

**Output**
Clicking the navigation links updates the URL bar dynamically to `/dashboard` and swaps the rendered content to `Dashboard Page` without triggering a full page refresh.

---

# 23. What are Module Bundlers (Webpack / Vite) and Production Builds?

**Answer**
Module bundlers (such as Webpack or Vite) analyze an application's dependency graph (JSX, JavaScript, CSS, images) and bundle assets into static distribution files (`/dist` or `/build`) optimized for web browsers. Production builds run optimizations including code minification, dead code removal (tree-shaking), chunking, and asset hash fingerprinting.

**Example Development vs Production Output Comparison**

```bash
# Terminal Build Script Execution
npm run build

```

Development Assets:

* Multi-megabyte unminified bundle (`bundle.js`)
* Source maps embedded for debugging
* Hot Module Replacement (HMR) runtime enabled

Production Build Output (`/dist`):

* `index.html`
* `assets/index-a1b2c3d4.js` (Gzipped & Minified)
* `assets/index-e5f6g7h8.css`

---

# 24. What is Code Splitting and Lazy Loading with `Suspense`?

**Answer**
Code splitting breaks a large JavaScript application bundle into smaller, download-on-demand chunks. **Lazy loading** defers downloading non-critical route chunks until the user navigates to them. React supports this natively using `React.lazy()` dynamic imports wrapped in a `<Suspense>` fallback boundary.

**Example**

```jsx
import React, { lazy, Suspense } from "react";

// Lazy load component chunk dynamically
const HeavyAdminPanel = lazy(() => import("./AdminPanel"));

export default function App() {
    const [showAdmin, setShowAdmin] = React.useState(false);

    return (
        <div>
            <button onClick={() => setShowAdmin(true)}>Load Admin</button>
            
            {showAdmin && (
                <Suspense fallback={<p>Downloading module chunk...</p>}>
                    <HeavyAdminPanel />
                </Suspense>
            )}
        </div>
    );
}

```

**Output**
Clicking "Load Admin" briefly displays `Downloading module chunk...` while fetching the JS file, then smoothly renders `HeavyAdminPanel`.

---

# 25. What is the SPA Deep-Link Routing Deployment Problem and How Do You Fix It?

**Answer**
When deploying a Single Page Application (SPA) with client-side routing (e.g., React Router), direct navigation to deep paths like `[example.com/dashboard](https://example.com/dashboard)` causes traditional web servers (Nginx, Apache, S3) to throw a **404 Not Found** error. This happens because the server searches for a physical `/dashboard/index.html` file on disk that does not exist. The fix requires configuring the web server to rewrite all fallback requests back to the main `/index.html` file so React Router can process the URL path.

**Example Server Fix (Nginx Configuration Rewrites)**

```text
server {
    listen 80;
    server_name example.com;
    root /var/www/react-app/dist;
    index index.html;

    location / {
        # Redirect all missing physical file paths back to index.html
        try_files $uri $uri/ /index.html;
    }
}

```

**Workflow**

```text
User navigates directly to /dashboard
                  │
                  ▼
Server checks for physical path /dashboard/index.html (Not Found)
                  │
                  ▼
Nginx try_files rule triggers -> Returns root /index.html (HTTP 200)
                  │
                  ▼
Browser downloads app bundle -> React Router reads path "/dashboard"
                  │
                  ▼
Dashboard component renders successfully

```

---

# 26. What Are React Hooks and Why Were They Introduced?

**Answer**
Hooks are built-in functions introduced in React 16.8 that allow functional components to manage local state, run lifecycle side-effects, access persistent refs, and tap into Context without writing ES6 class components. They eliminate issues with class components such as confusing `this` context bindings, verbose boilerplate, and split lifecycle logic (`componentDidMount` vs `componentWillUnmount`) by enabling clean, composable stateful functions.

**Example**

```jsx
import { useState } from "react";

export default function HookExample() {
    const [count, setCount] = useState(0);

    return (
        <button onClick={() => setCount(count + 1)}>
            Count: {count}
        </button>
    );
}

```

**Output**
Displays an interactive button showing incremental numbers when clicked.

---

# 27. What is `useState` and How Do Functional Updates, Objects, and Lazy Initializers Work?

**Answer**
`useState` declares a state variable preserved across renders.

* **Functional Update**: Updating state using a callback function (`setState(prev => prev + 1)`) guarantees updates run against the absolute latest queued state, preventing stale batching bugs.
* **State Objects**: Objects in state are immutable and must be updated by spreading existing state into a new object instance (`setState(prev => ({ ...prev, key: 'val' }))`).
* **Lazy Initialization**: Passing a initializer function to `useState(() => compute())` ensures expensive initial state computations run only once during component mounting rather than executing on every render cycle.

**Example**

```jsx
import { useState } from "react";

function computeExpensiveInitialState() {
    console.log("Expensive initial computation running...");
    return 100;
}

export default function AdvancedState() {
    // 1. Lazy Initializer
    const [score, setScore] = useState(() => computeExpensiveInitialState());

    // 2. State Object
    const [user, setUser] = useState({ name: "Alice", role: "User" });

    const handleTripleIncrement = () => {
        // 3. Functional updates batch correctly
        setScore(prev => prev + 1);
        setScore(prev => prev + 1);
        setScore(prev => prev + 1);
    };

    const promoteUser = () => {
        setUser(prev => ({ ...prev, role: "Admin" }));
    };

    return (
        <div>
            <p>Score: {score}</p>
            <button onClick={handleTripleIncrement}>+3 Score</button>
            <p>User: {user.name} ({user.role})</p>
            <button onClick={promoteUser}>Promote</button>
        </div>
    );
}

```

**Output**

* Initial score: `100`. Clicking `+3 Score` correctly updates score to `103`.
* Promoting updates `User: Alice (Admin)` cleanly without losing `name`.

---

# 28. What Are Closures and Stale Closures in React Hooks?

**Answer**
A **closure** occurs when an inner function retains access to variables declared in its outer scope. Because React render cycles re-run component functions, each render creates its own closed-over snapshot of local variables (state, props). A **stale closure** happens when an asynchronous callback (such as a timer or event handler) retains a reference to an outdated closure variable from an older render, missing current state values.

**Example Bug (Stale Closure)**

```jsx
import { useState, useRef } from "react";

export default function StaleClosureDemo() {
    const [count, setCount] = useState(0);
    const countRef = useRef(count);
    countRef.current = count; // Keep ref updated with latest value

    const handleStaleAlert = () => {
        setTimeout(() => {
            // Bug: Reading closed-over state from timer creation frame
            alert(`Stale captured state: ${count}`);
        }, 3000);
    };

    const handleFixedAlert = () => {
        setTimeout(() => {
            // Fix: Reading mutable ref current value
            alert(`Fixed current state: ${countRef.current}`);
        }, 3000);
    };

    return (
        <div>
            <p>Current Count: {count}</p>
            <button onClick={() => setCount(c => c + 1)}>Increment</button>
            <button onClick={handleStaleAlert}>Trigger Stale Alert (3s)</button>
            <button onClick={handleFixedAlert}>Trigger Fixed Alert (3s)</button>
        </div>
    );
}

```

**Output**

1. Click `Trigger Stale Alert` when count is `0`.
2. Rapidly click `Increment` 3 times (count becomes `3`).
3. After 3 seconds, the stale alert reads `Stale captured state: 0`. The fixed alert reads `Fixed current state: 3`.

---

# 29. How Does `useEffect` Synchronize External Systems and Clean Up?

**Answer**
`useEffect` synchronizes functional React components with external systems (such as browser DOM APIs, subscriptions, network sockets, or timers). Its second argument is a dependency array:

* **No Array**: Runs after every committed render.
* **Empty Array `[]**`: Runs once after initial mount.
* **Dependency List `[a, b]**`: Runs after initial mount and whenever `a` or `b` values change.
* **Cleanup Function**: Returning a function inside `useEffect` cleans up resources (e.g., clearing intervals or unsubscribing) before the component re-runs the effect or unmounts.

**Example**

```jsx
import { useState, useEffect } from "react";

export default function Timer() {
    const [seconds, setSeconds] = useState(0);

    useEffect(() => {
        const intervalId = setInterval(() => {
            setSeconds(prev => prev + 1);
        }, 1000);

        // Cleanup function prevents memory leaks
        return () => clearInterval(intervalId);
    }, []); 

    return <h3>Timer: {seconds}s</h3>;
}

```

**Output**
Renders an active counter (`Timer: 1s`, `Timer: 2s`) updating every second without creating leaking background timers.

---

# 30. What Are the Rules of Hooks and Why Must They Be Followed?

**Answer**
React enforces two fundamental rules for Hooks:

1. **Call Hooks only at the top level**: Never call Hooks inside conditional statements (`if`), loops (`for`), or nested inner functions.
2. **Call Hooks only from React function components or Custom Hooks**.

**Why?**
React tracks Hook values internally using an ordered single linked-list array allocated to the component instance. React relies on the exact invocation sequence of Hooks across every render cycle to map state slots correctly. Changing Hook execution order causes state mismatch errors.

**Incorrect Code (Violates Rule 1)**

```jsx
// DO NOT DO THIS!
function InvalidComponent({ isLoggedIn }) {
    if (isLoggedIn) {
        // Error: Hook called conditionally!
        useEffect(() => { console.log("Logged in"); }, []);
    }
    const [count, setCount] = useState(0);
}

```

**Correct Code**

```jsx
import { useState, useEffect } from "react";

export default function ValidComponent({ isLoggedIn }) {
    const [count, setCount] = useState(0);

    useEffect(() => {
        if (isLoggedIn) {
            console.log("Logged in user logic");
        }
    }, [isLoggedIn]);

    return <div>Count: {count}</div>;
}

```

**Output**
Standard execution without runtime rendering crash errors.

---

# 31. What are Custom Hooks and How Do They Encapsulate Stateful Logic?

**Answer**
A Custom Hook is a JavaScript function whose name starts with `use` and which can invoke other React Hooks. Custom Hooks extract and encapsulate reusable stateful behavior across components without duplicating logic or altering UI component hierarchies. Each call to a Custom Hook maintains its own completely isolated state.

**Example**

```jsx
import { useState, useEffect } from "react";

// Custom Hook
function useOnlineStatus() {
    const [isOnline, setIsOnline] = useState(navigator.onLine);

    useEffect(() => {
        const handleOnline = () => setIsOnline(true);
        const handleOffline = () => setIsOnline(false);

        window.addEventListener("online", handleOnline);
        window.addEventListener("offline", handleOffline);

        return () => {
            window.removeEventListener("online", handleOnline);
            window.removeEventListener("offline", handleOffline);
        };
    }, []);

    return isOnline;
}

// Consumer Component
export default function StatusBadge() {
    const isOnline = useOnlineStatus();

    return (
        <span style={{ color: isOnline ? "green" : "red" }}>
            {isOnline ? "● Connected" : "○ Disconnected"}
        </span>
    );
}

```

**Output**
Displays a live status indicator badge (`● Connected`) updating automatically when network connectivity changes.

---

# 32. What is `useReducer` and When Should It Be Used Over `useState`?

**Answer**
`useReducer` is an alternative state-management Hook inspired by Redux. It receives a reducer function `(state, action) => newState` along with an initial state, and returns the current state paired with a `dispatch` method. `useReducer` is preferred over `useState` when managing complex state trees containing multiple sub-values, or when future state updates depend heavily on previous complex state structures.

**Example**

```jsx
import { useReducer } from "react";

const initialState = { count: 0, step: 1 };

function reducer(state, action) {
    switch (action.type) {
        case "increment":
            return { ...state, count: state.count + state.step };
        case "decrement":
            return { ...state, count: state.count - state.step };
        case "setStep":
            return { ...state, step: action.payload };
        default:
            return state;
    }
}

export default function ReducerCounter() {
    const [state, dispatch] = useReducer(reducer, initialState);

    return (
        <div>
            <h2>Count: {state.count}</h2>
            <button onClick={() => dispatch({ type: "increment" })}>+</button>
            <button onClick={() => dispatch({ type: "decrement" })}>-</button>
            <input 
                type="number" 
                value={state.step} 
                onChange={(e) => dispatch({ type: "setStep", payload: Number(e.target.value) })}
            />
        </div>
    );
}

```

**Output**
Clicking `+` increments count by the configured input step value. All state logic stays isolated inside the pure `reducer` function.

---

# 33. What is `useRef` and How Does It Store Mutable Values Across Renders?

**Answer**
`useRef` returns a plain JavaScript object `{ current: initialValue }` that persists across the lifecycle of a component instance. Mutating `.current` changes the value synchronously **without** causing a component re-render. `useRef` is primarily used for referencing physical DOM nodes directly and for persisting instance values (like timer IDs, instance flags, or previous prop snapshots) across render cycles.

**Example**

```jsx
import { useRef } from "react";

export default function RefDemo() {
    const inputRef = useRef(null);
    const clickCountRef = useRef(0);

    const handleFocus = () => {
        // Direct DOM access
        inputRef.current.focus();
        
        // Silent mutation without triggering UI re-render
        clickCountRef.current += 1;
        console.log(`Focus button clicked ${clickCountRef.current} times`);
    };

    return (
        <div>
            <input ref={inputRef} type="text" placeholder="Click button to focus..." />
            <button onClick={handleFocus}>Focus Input</button>
        </div>
    );
}

```

**Output**
Clicking "Focus Input" forces browser cursor focus into the text input box while silently tracking click metrics in the console without triggering UI re-renders.

---

# 34. How Do `React.memo`, `useCallback`, and `useMemo` Work Together for Performance Optimization?

**Answer**
These three APIs prevent unnecessary renders and computations:

* **`React.memo`**: Wraps a child component, memoizing its rendered UI output. React skips rendering the child if its props have not shallowly changed.
* **`useMemo`**: Memoizes the evaluated result of an expensive computation, re-evaluating it only when specified dependencies change.
* **`useCallback`**: Memoizes a function reference itself across renders, ensuring child components wrapped in `React.memo` do not re-render due to new function instances created on every parent render.

**Example**

```jsx
import React, { useState, useMemo, useCallback } from "react";

// Memoized Child Component
const ChildList = React.memo(({ items, onItemClick }) => {
    console.log("ChildList rendered!");
    return (
        <ul>
            {items.map(item => (
                <li key={item} onClick={() => onItemClick(item)}>{item}</li>
            ))}
        </ul>
    );
});

export default function ParentOptimization() {
    const [count, setCount] = useState(0);
    const [searchTerm, setSearchTerm] = useState("");

    const rawItems = useMemo(() => ["Apple", "Banana", "Cherry", "Date"], []);

    // 1. Memoized Calculation
    const filteredItems = useMemo(() => {
        return rawItems.filter(item => item.toLowerCase().includes(searchTerm.toLowerCase()));
    }, [rawItems, searchTerm]);

    // 2. Memoized Function Callback
    const handleItemClick = useCallback((item) => {
        console.log("Clicked item:", item);
    }, []);

    return (
        <div>
            <button onClick={() => setCount(c => c + 1)}>Re-render Parent ({count})</button>
            <input 
                placeholder="Filter fruit..." 
                value={searchTerm} 
                onChange={e => setSearchTerm(e.target.value)} 
            />
            <ChildList items={filteredItems} onItemClick={handleItemClick} />
        </div>
    );
}

```

**Output**
Clicking "Re-render Parent" increments parent state, but console logs confirm `ChildList rendered!` is **skipped** because `filteredItems` and `handleItemClick` maintain exact reference equality.

---

# 35. How Do You Refactor a Legacy Class Component to Functional Hooks?

**Answer**
Refactoring requires shifting from imperatively handling lifecycle state methods (`componentDidMount`, `setState`) to declaratively specifying state dependencies (`useState`, `useEffect`).

**Legacy Class Component**

```jsx
import React from "react";

class LegacyCounter extends React.Component {
    state = { count: 0 };

    componentDidMount() {
        document.title = `Count: ${this.state.count}`;
    }

    componentDidUpdate() {
        document.title = `Count: ${this.state.count}`;
    }

    render() {
        return (
            <button onClick={() => this.setState({ count: this.state.count + 1 })}>
                Count: {this.state.count}
            </button>
        );
    }
}
export default LegacyCounter;

```

**Modern Refactored Functional Hook Component**

```jsx
import { useState, useEffect } from "react";

export default function ModernCounter() {
    const [count, setCount] = useState(0);

    // Replaces both componentDidMount & componentDidUpdate cleanly
    useEffect(() => {
        document.title = `Count: ${count}`;
    }, [count]);

    return (
        <button onClick={() => setCount(c => c + 1)}>
            Count: {count}
        </button>
    );
}

```

**Output**
Identical functional behavior: renders button and keeps browser tab `document.title` updated while removing class syntax boilerplate.

---

# 36. What is the Virtual DOM and React Rendering Architecture (Reconciliation, Fiber, Commit)?

**Answer**
The **Virtual DOM (VDOM)** is an lightweight in-memory tree of JavaScript objects representing the real DOM structure.
React executes rendering through three distinct phases:

1. **Trigger Phase**: An event or state update enqueues a render request.
2. **Render / Reconciliation Phase**: React executes component render functions, builds a new Fiber VDOM tree, and diffs it against the old tree using its heuristics algorithm. This phase is purely computational and produces zero host DOM changes.
3. **Commit Phase**: React synchronously mutates the actual real DOM using calculated minimal operations.
4. **Layout Effects & Paint**: `useLayoutEffect` runs synchronously before the browser paints. The browser then paints pixels to screen, after which `useEffect` runs asynchronously.

**Workflow Architecture**

```text
State Setter (Trigger)
         │
         ▼
Render Phase (Fiber VDOM Reconciliation & Diffing)
         │
         ▼
Commit Phase (Host Browser DOM Mutated)
         │
         ▼
useLayoutEffect Execution
         │
         ▼
Browser Screen Paint
         │
         ▼
useEffect Execution (Asynchronous Side Effects)

```

---

# 37. What is Redux / Redux Toolkit (RTK) and How Does It Compare to Context API?

**Answer**
Redux is a centralized state container for JavaScript apps. Modern Redux uses **Redux Toolkit (RTK)** to remove legacy boilerplate.

* **Context API**: Native feature designed for sharing low-frequency contextual configuration values (theme, locale, auth user) across sub-trees.
* **Redux Toolkit**: External predictable global store designed for complex, high-frequency state updates, providing action/reducer architectures, DevTools time-travel debugging, middleware capabilities, and performance selectors.

**Modern Redux Toolkit (RTK) Example**

```javascript
import { configureStore, createSlice } from "@reduxjs/toolkit";
import { Provider, useSelector, useDispatch } from "react-redux";

// 1. Create Slice
const counterSlice = createSlice({
    name: "counter",
    initialState: { value: 0 },
    reducers: {
        increment: (state) => { state.value += 1; }, // Immer handles immutability
        decrement: (state) => { state.value -= 1; }
    }
});

export const { increment, decrement } = counterSlice.actions;

// 2. Configure Store
const store = configureStore({
    reducer: { counter: counterSlice.reducer }
});

// 3. UI Component
function CounterUI() {
    const count = useSelector((state) => state.counter.value);
    const dispatch = useDispatch();

    return (
        <div>
            <h2>RTK Store Count: {count}</h2>
            <button onClick={() => dispatch(increment())}>Increment</button>
        </div>
    );
}

export default function App() {
    return (
        <Provider store={store}>
            <CounterUI />
        </Provider>
    );
}

```

**Output**
Renders a button backed by a central RTK store. Clicking dispatches actions cleanly through reducers.

---

# 38. What is Next.js and How Do SSR, SSG, App Router, and Middleware Work?

**Answer**
Next.js is a production React framework providing hybrid server and client rendering features:

* **Server-Side Rendering (SSR)**: Generates HTML on the server dynamically **per incoming client request**. Ideal for dynamic data.
* **Static Site Generation (SSG)**: Pre-renders HTML at **build time**. Ideal for blogs, marketing sites, and docs.
* **App Router (`app/` directory)**: Modern file-system router built on React Server Components (RSC) where components render on the server by default unless labeled with `"use client"`.
* **Middleware**: Runs edge code intercepting incoming requests **before** they hit page rendering pipelines. Useful for auth checks and redirects.

**Example (Next.js App Router Page & Middleware)**

*Middleware (`middleware.ts`)*

```typescript
import { NextResponse } from "next/server";
import type { NextRequest } from "next/server";

export function middleware(request: NextRequest) {
    const token = request.cookies.get("authToken");
    if (!token && request.nextUrl.pathname.startsWith("/dashboard")) {
        return NextResponse.redirect(new URL("/login", request.url));
    }
    return NextResponse.next();
}

```

*Server Component Page (`app/users/page.tsx`)*

```tsx
// Server Component (RSC) - Runs exclusively on server
async function getUsers() {
    const res = await fetch("https://jsonplaceholder.typicode.com/users", { cache: "force-cache" }); // SSG behavior
    return res.json();
}

export default async function UsersPage() {
    const users = await getUsers();

    return (
        <main>
            <h1>Server Rendered User List</h1>
            <ul>
                {users.map((u: any) => (
                    <li key={u.id}>{u.name}</li>
                ))}
            </ul>
        </main>
    );
}

```

**Output**
Server generates complete HTML containing fetched users directly on the server host and streams it to the browser, yielding immediate rendering and high SEO visibility.

**Architecture Workflow**

```text
Browser Client Request
         │
         ▼
Next.js Middleware Interception (Edge Validation / Auth Check)
         │
         ▼
Next.js App Router (React Server Components / SSR / SSG Data Fetch)
         │
         ▼
Fully Rendered HTML Page Streamed to Browser
         │
         ▼
Client-Side Hydration (Interactive Client Components take over)

```

---

# 39. What Are Core Web Vitals and How Do You Optimize React Performance?

**Answer**
Core Web Vitals are Google metrics that assess user experience:

* **Largest Contentful Paint (LCP)**: Measures main content loading speed (Target: $< 2.5\text{s}$).
* **Interaction to Next Paint (INP)**: Measures page interaction responsiveness (Target: $< 200\text{ms}$).
* **Cumulative Layout Shift (CLS)**: Measures visual layout stability (Target: $< 0.1$).

**Key React / Next.js Optimizations**

* Fix LCP: Utilize Next.js `<Image>` component for automatic WebP format conversion, lazy loading, and dimension sizing.
* Fix INP: Break heavy synchronous JavaScript loops using code-splitting, Web Workers, or `useTransition` / `useDeferredValue` hooks to yield main thread responsiveness.
* Fix CLS: Set explicit width and height dimensions on dynamic images, embeds, and dynamic elements.

**Example Optimization Implementation**

```jsx
import Image from "next/image";
import { useState, useTransition } from "react";

export default function PerformanceOptimized() {
    const [isPending, startTransition] = useTransition();
    const [filter, setFilter] = useState("");

    const handleFilterChange = (e) => {
        // High priority state update (keeps input responsive -> fixes INP)
        const text = e.target.value;
        
        // Low priority background transition update
        startTransition(() => {
            setFilter(text);
        });
    };

    return (
        <div>
            {/* Fixes LCP & CLS with explicit dimensions and optimization */}
            <Image 
                src="/hero.jpg" 
                alt="Hero banner" 
                width={800} 
                height={400} 
                priority 
            />

            <input onChange={handleFilterChange} placeholder="Search list..." />
            {isPending && <p>Filtering heavy list...</p>}
        </div>
    );
}

```

**Output**
Smooth UI input response without dropped frames or unexpected visual layout shifts during heavy processing.

---

# Senior Interview Architecture & Performance Cheat Sheet

**Core Principles**

* React is a component-driven UI library that updates DOM elements through declarative state management ($UI = f(State)$).
* Prefer Composition over Inheritance using props and wrapper components.
* Keep components pure: same inputs must yield identical output JSX with zero side-effects during execution.

**Props, State, & Reconciliation**

* Props flow unidirectionally down the component tree and are strictly read-only.
* State updates trigger React's Fiber engine to execute reconciliation diffing between Virtual DOM trees before mutating host browser elements synchronously.
* Unique, stable `key` props (such as database primary keys) are mandatory in dynamic arrays to help reconciliation track sibling insertions and removals.

**Hooks Key Takeaways**

* Call Hooks strictly at the top level of React functions (never conditionally or inside loops) to maintain deterministic array slot indices across re-renders.
* `useState`: Functional updates `setState(prev => ...)` ensure updates run against current queued values.
* `useEffect`: Synchronizes React with external systems. Always return cleanup functions to prevent memory leaks.
* `useRef`: Stores mutable values that persist across renders without triggering a re-render when changed.
* `useReducer`: Preferred for managing multi-field, complex state updates.

**Performance & Optimization Patterns**

* `React.memo`: Skips child component re-renders when props pass shallow equality checks.
* `useMemo`: Caches results of expensive calculations.
* `useCallback`: Retains function reference equality across renders to optimize memoized child components.
* Utilize dynamic imports (`React.lazy` + `<Suspense>`) for code-splitting large route bundles.

**State Management Strategy**

* Local UI State: `useState` / `useReducer`.
* Shared Low-Frequency Cross-Cutting State: Context API (e.g., Theme, Auth context).
* Complex High-Frequency Global State: Redux Toolkit (RTK) with selectors and slices.

**Next.js & Rendering Modes**
* Server Components (RSC): Render on the server by default, lowering JavaScript bundle sizes sent to the client.
* Server-Side Rendering (SSR): Dynamic per-request rendering on server.
* Static Site Generation (SSG): Build-time dynamic pre-rendering for optimal speed and SEO.
* SPA Fallback Routing: Set web servers to redirect non-physical path requests to `/index.html` to fix deep-link 404 errors.