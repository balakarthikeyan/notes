# **Overview & Introduction**

## **What is React and what is its historical background?**
React is an efficient, flexible, and open-source JavaScript framework library that allows developers to create simple, fast, and scalable web applications. Jordan Walke, a software engineer at Facebook, created React. It was first deployed on Facebook's news feed in 2011 and on Instagram in 2012. Developers with a JavaScript background can easily develop web applications using React.

## **What are React Hooks and when were they introduced?**
React Hooks allow you to use state and other React features without writing a class component. React Hooks are functions that connect React state with lifecycle features from function components. React Hooks were introduced in React version 16.8.

## **What is the scope and advantage of choosing React?**
Selection of the right technology for application or web development is becoming more challenging, and React is considered the fastest-growing JavaScript framework/library. JavaScript tools are firming their roots in the marketplace and React certification demand is exponentially increasing. React is a clear win for front-end developers as it offers a quick learning curve, clean abstraction, and reusable components.

---

# **React Interview Questions for Freshers**

## **What is React?**
React is a front-end and open-source JavaScript library useful in developing user interfaces specifically for single-page applications. It helps build complex and reusable user interface (UI) components for mobile and web applications using a component-based approach.

Important features of React:

* It supports server-side rendering.
* It uses Virtual DOM rather than real DOM (Document Object Model), as Real DOM manipulations are expensive.
* It follows unidirectional data binding or data flow.
* It uses reusable or composable UI components for developing views.

## **What are the advantages of using React?**
MVC is generally abbreviated as Model View Controller. Key advantages include:

* **Use of Virtual DOM to improve efficiency:** React uses Virtual DOM to render views. Each time data changes, a new Virtual DOM is created. Creating Virtual DOM is much faster than rendering UI inside the browser, improving app efficiency.
* **Gentle learning curve:** React has a gentle learning curve compared to frameworks like Angular. Basic JavaScript knowledge is enough to start building web applications.
* **SEO friendly:** React allows developers to build engaging UIs navigable by search engines and supports server-side rendering to boost SEO.
* **Reusable components:** Component-based architecture allows code reuse across applications with similar functionality, increasing development speed.
* **Huge ecosystem of libraries:** React provides freedom to choose tools, libraries, and architecture based on project requirements.

## **What are the limitations of React?**
Key limitations of React include:

* React is a library, not a full-blown framework.
* React has numerous components that take time to fully grasp.
* Beginner programmers may find it difficult to understand initially.
* Coding can become complex due to inline templating and JSX.

## **What is useState() in React?**
`useState()` is a built-in React Hook that allows functional components to have state variables. It is used when the DOM needs dynamic manipulation or control.

Example:
```javascript
const [count, setCounter] = useState(0);
const [otherStuffs, setOtherStuffs] = useState(...);

const setCount = () => {
   setCounter(count + 1);
   setOtherStuffs(...);
};
```

In this example, `useState(0)` returns a tuple where `count` represents the state and `setCounter` is the method used to update it. Using hooks keeps code functional and avoids unnecessary class components.

## **What are keys in React?**
A key is a special string attribute that needs to be included when rendering lists of elements.

Example:
```jsx
const ids = [1, 2, 3, 4, 5];
const listElements = ids.map((id) => {
  return (
    <li key={id.toString()}>
      {id}
    </li>
  );
});
```

Importance of keys:

* Keys help React identify which elements were added, changed, or removed.
* Keys provide a unique identity to each array element.
* Without keys, React cannot determine element order or uniqueness.
* Keys are typically used when displaying API list data.
* *Note:* Keys must be unique among siblings, but do not need to be globally unique.

## **What is JSX and how does it work?**
JSX stands for JavaScript XML. It allows writing HTML inside JavaScript and inserting elements into the DOM without using `appendChild()` or `createElement()`. It provides syntactic sugar for `React.createElement()`.

Without JSX:

```javascript
const text = React.createElement('p', {}, 'This is a text');
const container = React.createElement('div', {}, text);
ReactDOM.render(container, rootElement);
```

With JSX:

```jsx
const container = (
  <div>
    <p>This is a text</p>
  </div>
);
ReactDOM.render(container, rootElement);
```

## **What are the differences between functional and class components?**
Prior to Hooks, functional components were stateless. With Hooks, functional components are equivalent in features to class components.

1. **Declaration:**
* Functional components use JavaScript function syntax or arrow functions.
```jsx
function Card(props) {
  return (
    <div className="main-container">
      <h2>Title of the card</h2>
    </div>
  );
}
```

* Class components use ES6 class syntax by extend `React.Component`:
```jsx
class Card extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <div className="main-container">
        <h2>Title of the card</h2>
      </div>
    );
  }
}
```

2. **Handling Props (`<StudentInfo name="Vivek" rollNumber="23"/>`):**
* Functional components accept `props` as an argument directly.
```jsx
function StudentInfo(props) {
  return (
    <div className="main">
      <h2>{props.name}</h2>
      <h4>{props.rollNumber}</h4>
    </div>
  );
}
```

* Class components access props via `this.props`:
```jsx
class StudentInfo extends React.Component {
  constructor(props) {
    super(props);
  }
  render() {
    return (
      <div className="main">
        <h2>{this.props.name}</h2>
        <h4>{this.props.rollNumber}</h4> 
      </div>
    );
  }
}
```

3. **Handling State:**
* Functional components manage state via the `useState` hook.
```jsx
function ClassRoom(props) {
  const [studentsCount, setStudentsCount] = useState(0);
  const addStudent = () => {
    setStudentsCount(studentsCount + 1);
  };
  return (
    <div>
      <p>Number of students in class room: {studentsCount}</p>
      <button onClick={addStudent}>Add Student</button>
    </div>
  );
}
```

* Class components initialize state in `this.state` within the constructor and update it using `this.setState()`.
```jsx
class ClassRoom extends React.Component {
  constructor(props) {
    super(props);
    this.state = { studentsCount: 0 };
    this.addStudent = this.addStudent.bind(this);
  }
  addStudent() {
    this.setState((prevState) => {
      return { studentsCount: prevState.studentsCount + 1 };
    });
  }
  render() {
    return (
      <div>
        <p>Number of students in class room: {this.state.studentsCount}</p>
        <button onClick={this.addStudent}>Add Student</button>
      </div>
    );
  }
}
```

## **What is the Virtual DOM? How does React use the Virtual DOM to render the UI?**
Virtual DOM is a concept where a virtual representation of the real DOM is kept in memory and synced with the real DOM using ReactDOM.

* **Why was Virtual DOM introduced?** DOM manipulations are slow compared to JavaScript operations. Updating the entire DOM for small changes causes inefficiency.
* **How it works:** For every DOM object, there is a corresponding Virtual DOM object. When state/props update, React creates a new Virtual DOM tree, diffs it against the previous tree, and updates only the changed elements in the real DOM.

## **What are the differences between controlled and uncontrolled components?**
Controlled and uncontrolled components are different approaches to handling form inputs in React.

| Feature | Uncontrolled Component | Controlled Component |
| --- | --- | --- |
| **One-time value retrieval (e.g. on submit)** | ✔️ | ✔️ |
| **Validating on submit** | ✔️ | ✔️ |
| **Field-level Validation** | ❌ | ✔️ |
| **Conditionally disabling submit button** | ❌ | ✔️ |
| **Enforcing input format** | ❌ | ✔️ |
| **Several inputs for one piece of data** | ❌ | ✔️ |
| **Dynamic inputs** | ❌ | ✔️ |

* **Controlled Component Example:**
Form input values are handled directly by React state via `value` attributes and `onChange` event handlers. Supports field-level validation, conditional disabling, and dynamic formatting.
```jsx
function FormValidation(props) {
  const [inputValue, setInputValue] = useState("");
  const [inputValue, setInputValue] = useState("");
  const updateInput = (e) => {
    setInputValue(e.target.value);
  };
  return (
    <div>
      <form>
        <input type="text" value={inputValue} onChange={updateInput} />
        {/* <input type="text" value={inputValue} onChange={(e) => setInputValue(e.target.value)} /> */} // Inline
      </form>
    </div>
  );
}
```

* **Uncontrolled Component Example:**
Form input data is handled by the browser DOM itself. Values are queried on demand using `useRef` or `createRef`.
```jsx
function FormValidation(props) {
  const inputValue = React.createRef();
  const handleSubmit = (e) => {
    alert(`Input value: ${inputValue.current.value}`);
    e.preventDefault();
  };
  return (
    <div>
      <form onSubmit={handleSubmit}>
        <input type="text" ref={inputValue} />
        <button type="submit">Submit</button>
      </form>
    </div>
  );
}
```

## **What are props in React?**
Props are inputs passed to React components from parent components. They are immutable (read-only) and passed using standard HTML attribute (e.g., `<Element reactProp="1"/>`). They allow passing custom data, accessing values via `this.props.reactProp` or function parameters, and triggering state changes in child components.

## **Explain React state and props.**
| Props | State |
| --- | --- |
| Immutable | Owned by its component |
| Has better performance | Locally scoped |
| Can be passed to child components | Writable/Mutable via `setState()` | 
| Passed down hierarchy | Changes can be asynchronous |

* **React State:** Built-in object containing property values belonging to a component. Changes in state lead to re-rendering of the component.

* **Declaring & updating state in Class Components:**
```jsx
class Car extends React.Component {
  constructor(props) {
    super(props);
    // *Declaration:*
    this.state = { brand: "BMW", color: "Black" };
  }

  // *Updating:*
  changeColor() {
    this.setState({ color: "Red" });
  }
  render() {
    return (
      <div>
        <button onClick={() => this.changeColor()}>Change Color</button>
        <p>{this.state.color}</p>
      </div>
    );
  }
}
```

* **React Props:** Inputs passed down from parent components via HTML attributes.
  * Class component access: `this.props.brand`.
  * Functional component access: `props.brand`.
  * Props are read-only and cannot be mutated inside the receiving component.

In Functional component:
```js
function Car(props) {
let [brand, setBrand] = useState(props.brand);
}
```

## **Explain types of side effects in React components.**
* **Effects without Cleanup:** Executed in `useEffect` without blocking screen updates (e.g., API calls, logging, manual DOM mutations, logging).
* **Effects with Cleanup:** Requires cleanup after DOM execution to prevent memory leaks (e.g., unsubscribing from external data sources).

## **What is prop drilling in React?**
Prop drilling occurs when data needs to be passed from a deeply nested child through intermediate components that do not require the data themselves.

## **What are error boundaries?**
Error boundaries are React components that catch JavaScript errors anywhere in their child component tree during rendering, lifecycle methods, and constructors. They implement `static getDerivedStateFromError()` to display fallback UI and `componentDidCatch()` to log errors.

Example:
```jsx
class ErrorBoundary extends React.Component {
  constructor(props) {
    super(props);
    this.state = { hasError: false };
  }
  static getDerivedStateFromError(error) {     
    return { hasError: true }; 
  }
  componentDidCatch(error, errorInfo) {       
    logErrorToMyService(error, errorInfo); 
  }
  render() {
    if (this.state.hasError) {     
      return <h4>Something went wrong</h4>;     
    }
    return this.props.children;
  }
}
```

Usage:
```javascript
<ErrorBoundary>
  <CounterComponent />
</ErrorBoundary>
```

## **What are React Hooks?**
React Hooks are built-in functions introduced in React 16.8 that let functional components use state and lifecycle features.

## **Explain React Hooks and why they were introduced.**
Hooks allow managing state and side effects in functional components without converting them into class components.

```javascript
function Person(props) {
  let [name, setName] = useState('');
}
```

## **What are the rules that must be followed while using React Hooks?**
1. Call Hooks only at the top level (never inside loops, conditions, or nested functions).
2. Call Hooks only from React function components or custom Hooks.

## **What is the use of the useEffect React Hook?**
`useEffect` performs side effects in functional components. The callback executes on initial mount and whenever items in the dependency array change.

Syntax: `useEffect(callback, [dependencies]);`.

Example:
```jsx
import { useEffect } from 'react';

function WelcomeGreetings({ name }) {
 const msg = `Hi, ${name}!`;     // Calculates output
  useEffect(() => {
   document.title = `Welcome to you ${name}`;    // Side-effect!
  }, [name]);
 return <div>{msg}</div>;         // Calculates output
}
```
Providing `[name]` ensures the effect executes only when the `name` prop changes.

## **Why do React Hooks make use of refs?**
`useRef` provides access to DOM elements in functional components to manage focus, media playback, text selection, or trigger imperative animations.

## **What are Custom Hooks?**
Custom Hooks are JavaScript functions starting with `use` that invoke other Hooks, enabling reusable stateful logic across components without changing component hierarchy or using Higher-Order Components. Custom Hooks cannot be used in class components.

---

**React Interview Questions for Experienced**

## **How to create a switching component for displaying different pages?**
Map prop values to specific components inside a lookup object:

```jsx
import HomePage from './HomePage';
import AboutPage from './AboutPage';
import FacilitiesPage from './FacilitiesPage';
import ContactPage from './ContactPage';
import HelpPage from './HelpPage';

const PAGES = {
  home: HomePage,
  about: AboutPage,
  facilities: FacilitiesPage,
  contact: ContactPage,
  help: HelpPage
};

const Page = (props) => {
  const Handler = PAGES[props.page] || HelpPage;
  return <Handler {...props} />;
};
// The PAGES object keys can be used in the prop types for catching errors during dev-time.
Page.propTypes = {
  page: PropTypes.oneOf(Object.keys(PAGES)).isRequired
};
```

## **How to re-render the view when the browser is resized?**
Register a listener for `resize` events in `componentDidMount()` and remove it in `componentWillUnmount()`:

```jsx
class WindowSizeDimensions extends React.Component {
  constructor(props) {
    super(props);
    this.updateDimension = this.updateDimension.bind(this);
  }
  componentWillMount() {
    this.updateDimension();
  }
  componentDidMount() {
    window.addEventListener('resize', this.updateDimension);
  }
  componentWillUnmount() {
    window.removeEventListener('resize', this.updateDimension);
  }
  updateDimension() {
    this.setState({ width: window.innerWidth, height: window.innerHeight });
  }
  render() {
    return <span>{this.state.width} x {this.state.height}</span>;
  }
}
```

## **How to pass data between sibling components using React Router?**
Navigation and state passing can be handled programmatically using React Router's `history.push('/route/' + data)` or route parameter matching via `props.match.params.aboutId`.

Use route params or history state:

```jsx
// AppDemo.js
import React, { Component } from 'react';
import { BrowserRouter as Router, Route, NavLink } from 'react-router-dom';

class AppDemo extends Component {
  render() {
    return (
      <Router>
        <div className="AppDemo">
          <ul>
            <li><NavLink to="/" activeStyle={{ color:'blue' }}>Home</NavLink></li>
            <li><NavLink to="/about" activeStyle={{ color:'blue' }}>About</NavLink></li>
          </ul>
          <Route path="/about/:aboutId" component={AboutPage} />
          <Route exact path="/about" component={AboutPage} />
          <Route exact path="/" component={HomePage} />
        </div>
      </Router>
    );
  }
}
export default AppDemo;

// Sender sibling HomePage.js
export default  function HomePage(props) {
  const handleClick = (data) => {
    props.history.push('/about/' + data);
  };
  return (
    <div>
      <button onClick={() => handleClick('DemoButton')}>To About</button>
    </div>
  );
}

// Receiver sibling AboutPage.js
export default  function AboutPage(props) {
  if (!props.match.params.aboutId) return <div>No Data Yet</div>;
  return <div>{`Data obtained: ${props.match.params.aboutId}`}</div>;
}
```

## **How to perform automatic redirect after login?**
Render `<Redirect>` from `react-router` based on authentication state:

```jsx
import React, { Component } from 'react';
import { Redirect } from 'react-router';

export default class LoginDemoComponent extends Component {
  render() {
    if (this.state.isLoggedIn === true) {
      return <Redirect to="/your/redirect/page" />;
    }
    return <div>Please complete login</div>;
  }
}
```

## **Does React Hook work with static typing?**
Yes, React Hooks are standard functions designed to work seamlessly with TypeScript or Flow for strong static typing.

## **Explain Strict Mode in React.**
`<React.StrictMode>` checks for potential problems in development (e.g., unsafe lifecycles, legacy string refs, deprecated `findDOMNode`).

```jsx
import React from "react";
import ReactDOM from "react-dom";
import App from "./App";

ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

Issues StrictMode checks for:
* Components using unsafe lifecycle methods.
* Usage of legacy string refs.
* Usage of deprecated `findDOMNode()` API.
* Usage of legacy context API.

## **How to prevent re-renders in React?**
* **Class Components:** Implement `shouldComponentUpdate()` returning `false`, or extend `React.PureComponent`.
* **Functional Components:** Wrap components in `React.memo()`, and stabilize callbacks/values using `useCallback()` and `useMemo()`.

```jsx
class Message extends React.Component {
  constructor(props) {
    super(props);
    this.state = { message: "Hello, this is Bala" };
  }
  shouldComponentUpdate() {
    console.log("Does not get rendered");
    return false; // Prevents re-rendering
  }
  render() {
    console.log("Message is getting rendered");
    return (
      <div>
        <p>{this.state.message}</p>
      </div>
    );
  }
}
```

## **What are the different ways to style a React component?**
1. Inline styling (`style={{ color: 'yellow' }}`)
```js
class RandomComponent extends React.Component {
 render() {
   return (
     <div>
       <h3 style={{ color: "Yellow" }}>This is a heading</h3>
       <p style={{ fontSize: "32px" }}>This is a paragraph</p>
     </div>
   );
 }
}
```
2. JavaScript Style Objects `style={this.headingStyles}`.
```js
class RandomComponent extends React.Component {
 paragraphStyles = {
   color: "Red",
   fontSize: "32px"
 };

 headingStyles = {
   color: "blue",
   fontSize: "48px"
 };

 render() {
   return (
     <div>
       <h3 style={this.headingStyles}>This is a heading</h3>
       <p style={this.paragraphStyles}>This is a paragraph</p>
     </div>
   );
 }
}
```
3. CSS Stylesheets (`import './styles.css'`)
```jsx
import './RandomComponent.css';

class RandomComponent extends React.Component {
 render() {
   return (
     <div>
       <h3 className="heading">This is a heading</h3>
       <p className="paragraph">This is a paragraph</p>
     </div>
   );
 }
}
```
4. CSS Modules (`import styles from './styles.module.css'`)  and applying classes via `className={styles.paragraph}`.
```js
import styles from  './styles.module.css';

class RandomComponent extends React.Component {
 render() {
   return (
     <div>
       <h3 className="heading">This is a heading</h3>
       <p className={styles.paragraph} >This is a paragraph</p>
     </div>
   );
 }
}
```

## **Name a few techniques to optimize React app performance.**
* Use `useMemo()` for caching expensive calculations.
* Use `React.PureComponent` or `React.memo`.
* Maintain state colocation.
* Use lazy loading with `React.lazy` and `Suspense`.
* Virtualize large list DOM nodes using tools like `react-window`.

## **How to pass data between React components?**
* **Parent to Child:** Via `props`.
* **Child to Parent:** Pass a callback function as a prop from Parent to Child:

```jsx
function ParentComponent() {
  const [counter, setCounter] = useState(0);
  const callback = (data) => setCounter(data);
  return <ChildComponent callbackFunc={callback} counterValue={counter} />;
}

function ChildComponent(props) {
  return (
    <button onClick={() => props.callbackFunc(props.counterValue + 1)}>
      Increment
    </button>
  );
}
```

## **What are Higher Order Components (HOC)?**
An HOC is a function that takes a component as an argument and returns a new enhanced component to share common logic.

```jsx
function HOC(WrappedComponent, selectData) {
  return class extends React.Component {
    constructor(props) {
      super(props);
      this.state = { data: selectData(GlobalDataSource, props) };
    }
    componentDidMount() {
     // Listens to the changes added
      GlobalDataSource.addChangeListener(this.handleChange);
    }
    componentWillUnmount() {
     // Listens to the changes removed
      GlobalDataSource.removeChangeListener(this.handleChange);
    }
    handleChange = () => {
      this.setState({ data: selectData(GlobalDataSource, this.props) });
    };
    render() {
     // Rendering the wrapped component with the latest data data
      return <WrappedComponent data={this.state.data} {...this.props} />;
    }
  };
}
```

Usage:
```javascript
const ArticlesListWithHOC = HOC(ArticlesList, (GlobalDataSource) => GlobalDataSource.getArticles());
const UsersListWithHOC = HOC(UsersList, (GlobalDataSource) => GlobalDataSource.getUsers());
```

## **What are the different phases of the component lifecycle?**
1. **Initialization:** Setting initial state/props.
2. **Mounting:** Inserting elements into DOM (`componentWillMount`, `componentDidMount`).
3. **Updating:** Updating state/props (`componentWillUpdate`, `shouldComponentUpdate`, `componentDidUpdate`, `render`).
4. **Unmounting:** Removing elements from DOM (`componentWillUnmount`).

## **What are the lifecycle methods of React?**
* `constructor()`: Initializes state and binds methods.
* `static getDerivedStateFromProps()`: Updates state based on initial or changing props.
* `render()`: Outputs JSX markup.
* `componentDidMount()`: Executes side effects immediately after mounting.
* `shouldComponentUpdate()`: Determines if re-rendering should occur.
* `getSnapshotBeforeUpdate()`: Captures DOM info before updates.
* `componentDidUpdate()`: Executes after DOM updates occur.
* `componentWillUnmount()`: Performs cleanup operations before removal.

## **Explain how to create a simple React Hooks example program.**
1. Initialize project: `npx create-react-app react-items-with-hooks`

2. Create `src/SearchItem.js`:

```jsx
import React from 'react';

export function SearchItem() {
  return (
    <div>
      <input type="text" placeholder="SearchItem"/>
      <h1>Search Results</h1>
      <table>
        <thead>
          <tr>
            <th>Item Name</th><th>Price</th><th>Quantity</th>
          </tr>
        </thead>
        <tbody></tbody>
      </table>
    </div>
  );
}
```

3. Import into `App.js`:

```jsx
import { SearchItem } from './SearchItem';

function App() {
  return (
    <div className="App">
      <SearchItem />
    </div>
  );
}
export default App;
```

4. Start app: `npm start` to execute app at `http://localhost:3000`.

## **Explain the types of Hooks in React.**
1. **Built-in Hooks:**
  * *Basic Hooks:* `useState`, `useEffect`, `useContext`
  * *Additional Hooks:* `useReducer`, `useMemo`, `useCallback`, `useImperativeHandle`, `useDebugValue`, `useRef`, `useLayoutEffect`

- `useState():` This functional component is used to set and retrieve the state.
- `useEffect():` It enables for performing the side effects in the functional components.
- `useContext():` It is used for creating common data that is to be accessed by the components hierarchy without having to pass the props down to each level.
- Additional Hooks:
- `useReducer() :` It is used when there is a complex state logic that is having several sub-values or when the upcoming state is dependent on the previous state. It will also enable you to optimization of component performance that will trigger deeper updates as it is permitted to pass the dispatch down instead of callbacks.
- `useMemo() :` This will be used for recomputing the memoized value when there is a change in one of the dependencies. This optimization will help for avoiding expensive calculations on each render.
- `useCallback() :` This is useful while passing callbacks into the optimized child components and depends on the equality of reference for the prevention of unneeded renders.
- `useImperativeHandle():`  It will enable modifying the instance that will be passed with the ref object.
- `useDebugValue():` It is used for displaying a label for custom hooks in React DevTools.
- `useRef() :` It will permit creating a reference to the DOM element directly within the functional component.
- `useLayoutEffect():` It is used for the reading layout from the DOM and re-rendering synchronously.

2. **Custom Hooks:** User-defined functions that encapsulate custom stateful logic.

## **Differentiate React Hooks vs Classes.**
| React Hooks | Classes |
| --- | --- |
| Used in functional components | Used in class components |
| No constructor needed | Requires `constructor()`<br> | 
| No `this` binding needed | Requires `this` keyword |
| Simplified state handling | Verbose setup |

## **How does performance of Hooks compare with classes?**
Hooks avoid overhead like instance creation and event binding while flattening the component tree by avoiding wrapper components.

## **Do Hooks cover all functionalities provided by classes?**
Most functionalities are covered, but there are no direct hook equivalents for `getSnapshotBeforeUpdate`, `getDerivedStateFromError`, and `componentDidCatch`.

## **What is React Router?**
Standard routing library for React single-page applications enabling UI synchronization with the URL. Main components: `BrowserRouter`, `Routes`, `Route`, `Link`.
* `BrowserRouter`: Uses HTML5 history API (`pushState`, `popstate`, `replaceState`) to synchronize UI with URL.
* `Routes`: Wrapper container for route definitions.
* `Route`: Renders specific component UIs when path matches current URL.
* `Link`: Enables client-side navigation without page reloads.

## **Can React Hooks replace Redux?**
`useReducer` and `useContext` can manage local/medium application state, but Redux remains optimal for complex, large-scale global application trees with middleware needs.

## **Explain conditional rendering in React.**
Displaying UI elements dynamically using standard JavaScript logic (`if-else`, ternary operators `condition ? A : B`, or logical `&&` operators).

---

**React Architecture & Advanced Concepts**

## **What is reconciliation? How does React's diffing algorithm work?**
Reconciliation is the internal process React uses to compare the previous Virtual DOM tree with the new Virtual DOM tree after state changes.

*Reconciliation summary:*
Reconciliation is the process by which React compares the previous and new Virtual DOM trees to compute the minimal DOM updates. It uses a heuristic $O(n)$ diffing algorithm based on two assumptions: elements of different types produce different trees, and keys provide a stable identity for list elements.

*Diffing Rules:*
1. **Different Root Elements:** Replaces the entire subtree.
2. **Same Element Type:** Updates only changed attributes.
3. **List Keys:** Uses stable `key` attributes to map children efficiently.

## **What is `forwardRef` and when should it be used?**
`React.forwardRef` passes a `ref` through a component to a nested internal DOM node. It is necessary because standard function components do not accept `ref` as a default prop, allowing parents to invoke imperative actions like `.focus()` or `.scrollIntoView()`.

Pattern:
```javascript
const Input = React.forwardRef((props, ref) => {
  return <input ref={ref} {...props} />;
});
```

Usage:
```javascript
const inputRef = useRef();
<Input ref={inputRef} />
inputRef.current.focus();
```

## **What is Server-Side Rendering (SSR)? How does it differ from Client-Side Rendering (CSR)?**
* **SSR:** HTML is generated on the server for each request, sent to the client, and hydrated by React to become interactive.
* **CSR:** Initial HTML is minimal; browser renders DOM via JavaScript execution.

| Server-Side Rendering (SSR) | Client-Side Rendering (CSR) |
| --- | --- |
| Server outputs full HTML | Browser outputs DOM via JS |
| Faster initial content view | Initial blank screen during load |
| Search engine friendly (SEO) | Harder for search crawlers |
| Requires hydration step | No hydration required |
| Higher server load | Workload handled by client browser |

*Hydration:* The process where React attaches event listeners to server-rendered HTML to make it interactive.

## **How do you manage global state? Compare Context, Redux, and modern alternatives.**
* **Context API:** Best for simple global state (e.g., theme, auth, language).
* **Redux:** Centralized store with strict update patterns, best for complex interdependent state.
* **Modern Alternatives (Zustand/Jotai):** Minimal boilerplate with fine-grained performance.
* **Server State:** Handled best by React Query / TanStack Query.

## **What are common React performance optimization techniques?**
1. Prevent unnecessary re-renders using `React.memo`.
2. Stabilize prop references using `useCallback` and `useMemo`.
3. Code splitting using `React.lazy` and `Suspense`.
4. State colocation (keeping state local to usage).
5. Windowing/Virtualization using libraries like `react-window`.

---

**React Modern Patterns & Performance**

## **What is Context API? When should you use it instead of prop drilling?**
Context API allows sharing data across the component tree without manually passing props at every level via `React.createContext()`, `Provider`, and `useContext()`. Use it for global data like themes, auth user data, or locale settings.

## **What is React.memo? How is it different from useMemo and useCallback?**
* **`React.memo`:** Higher-Order Component that skips re-rendering a component if its props haven't changed.
* **`useMemo`:** Hook that memoizes the **result** of an expensive computation inside a component.
* **`useCallback`:** Hook that memoizes a **function instance** between renders.

For example: `<Child user={{ name: "Kamala" }} />`
Here, a new object is created every time, so React thinks props have changed, and then a re-render happens.

a. `useMemo` stores/memorizes a value, so it doesn't get recreated on every render.

For example: `const user = useMemo(() => ({ name: "Kamala" }), []);`
Now the same object is reused, and React.memo can work properly.

b. `useCallback` is similar, but for functions.

For example:
```js
const handleClick = useCallback(() => {
  console.log("clicked");
}, []);
```
Without this, a new function is created every render, which can also break React.memo.

## **What is useReducer, and when would you use it over useState?**
`useReducer` manages complex state logic via action dispatches and reducer functions. Use it when next state logic depends on previous state or involves multiple sub-values.

```jsx
const [state, dispatch] = useReducer(reducer, { count: 0 });

function reducer(state, action) {
  if (action.type === "increment") {
    return { count: state.count + 1 };
  }
  return state;
}
```
Use `useReducer` over `useState` when handling complex state logic involving multiple sub-values or dependent state transitions (e.g., multi-field forms).

## **What are React Suspense and React.lazy? How do they enable code splitting?**
`React.lazy()` dynamically imports components, and `Suspense` displays a fallback UI while the chunk downloads.

```jsx
const Profile = React.lazy(() => import("./Profile"));

function App() {
  return (
    <Suspense fallback={<p>Loading...</p>}>
      <Profile />
    </Suspense>
  );
}
```

## **What are React Portals, and when would you use them?**
Portals render DOM elements into a DOM node outside the parent component's DOM hierarchy via `ReactDOM.createPortal(child, container)`. Used to break out of `overflow: hidden` or `z-index` stacking contexts (modals, tooltips, notifications).

```jsx
ReactDOM.createPortal(<Modal />, document.getElementById("modal-root"));
```

## **What is React.Fragment and why is it useful?**
`React.Fragment` (or `<> ... </>`) allows grouping multiple sibling components without adding extra node wrappers (`<div>`) to the browser DOM layout.

```jsx
return (
  <React.Fragment>
    <h1>Hello</h1>
    <p>World</p>
  </React.Fragment>
);
```

## **What is the difference between useEffect and useLayoutEffect?**
* **`useEffect`:** Runs asynchronously **after** DOM updates are painted on screen (non-blocking).
* **`useLayoutEffect`:** Runs synchronously **before** the browser paints DOM updates (blocking, prevents screen flickering during DOM measurements).

1. `useEffect` runs after the browser has painted the update.
```jsx
useEffect(() => {
  console.log("runs after paint");
});
```
2. `useLayoutEffect` runs before the browser paints.
```jsx
useLayoutEffect(() => {
  console.log("runs before paint");
});
```