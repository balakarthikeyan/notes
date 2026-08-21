**Flux & Redux Architecture**

Flux is an architectural pattern structured around unidirectional data flow. Data enters the app and flows through components in one direction until it renders on the screen.

```text
┌──────────┐      ┌────────────┐      ┌───────────┐      ┌──────────┐
│  Action  │ ---> │ Dispatcher │ ---> │   Store   │ ---> │   View   │
└──────────┘      └────────────┘      └───────────┘      └──────────┘
^                                                        │
└────────────────── User Events ─────────────────────────┘

```

**What is Redux and how does it work with React?**

Redux is a predictable architectural state management library for JavaScript apps based on the Flux design pattern. It enforces a single centralized global data container called a `store` that governs the state of the entire system.

* **How it Works:**
1. Components trigger architectural change events called **Actions** (plain JavaScript objects specifying a `type` and an optional payload).
2. These actions are passed to pure functions called **Reducers**.
3. Reducers interpret the action type and return an updated state object without modifying the existing state directly.
4. The updated state is pushed to the store, which automatically notifies subscribed React components via the `react-redux` context connector layer (`connect` in legacy class components, or `useSelector` hooks in modern configurations).



**Architecture Elements**

* **Actions**: JavaScript objects carrying a `type` property that describe payload intent sent to the store.
* **Dispatcher**: The central hub that routes all actions and payloads to the stores.
* **Store**: Holds application state and logic, updating state in response to dispatched actions.
* **View**: React components that receive store data and render the visual interface.

---

**Redux Data Flow Cycle & Implementation**

**Data Flow Execution Order**

1. **Trigger**: A React component mounts and invokes an action creator (e.g., `this.props.getMovies()`).
2. **Dispatch**: The action creator executes and dispatches an action object (e.g., `{ type: GET_MOVIES, data }`).
3. **Redirection**: Redux routes the action through reducers (e.g., `rootReducer` to `movieReducer`).
4. **State Update**: Reducers handle action matching and return an updated state tree.
5. **Re-render**: Connected components receive state updates via `mapStateToProps` / `useSelector` and re-render the UI.

---

**Legacy Redux Implementation (Class Components & Boilerplate)**

**1. Installation**

```bash
npm install redux react-redux --save

```

**2. Actions (`actions/actions.js`)**

```javascript
export const ADD_TODO = 'ADD_TODO';
let nextTodoId = 0;

export function addTodo(text) {
   return {
      type: ADD_TODO,
      id: nextTodoId++,
      text
   };
}

```

**3. Reducers (`reducers/reducers.js`)**

```javascript
import { combineReducers } from 'redux';
import { ADD_TODO } from '../actions/actions';
 
function todo(state, action) {
   switch (action.type) {
      case ADD_TODO:
         return {
            id: action.id,
            text: action.text,
         };
      default:
         return state;
   }
}

function todos(state = [], action) {
   switch (action.type) {
      case ADD_TODO:
         return [
            ...state,
            todo(undefined, action)
         ];
      default:
         return state;
   }
}

const todoApp = combineReducers({
   todos
});

export default todoApp;

```

**4. Component Integration (`App.jsx`)**

```javascript
import React, { Component } from 'react';
import { connect } from 'react-redux';
import { addTodo } from './actions/actions';

class AddTodo extends Component {
   render() {
      return (
         <div>
            <input type='text' ref='input' />
            <button onClick={(e) => this.handleClick(e)}>Add</button>
         </div>
      );
   }
   handleClick(e) {
      const node = this.refs.input;
      const text = node.value.trim();
      this.props.onAddClick(text);
      node.value = '';
   }
}

class Todo extends Component {
   render() {
      return <li>{this.props.text}</li>;
   }
}

class TodoList extends Component {
   render() {
      return (
         <ul>
            {this.props.todos.map(todo =>
               <Todo key={todo.id} {...todo} />
            )}
         </ul>
      );
   }
}

class App extends Component {
   render() {
      const { dispatch, visibleTodos } = this.props;
      return (
         <div>
            <AddTodo onAddClick={text => dispatch(addTodo(text))} />
            <TodoList todos={visibleTodos} />
         </div>
      );
   }
}

function select(state) {
   return {
      visibleTodos: state.todos
   };
}

export default connect(select)(App);

```

**5. Entry Point (`main.js`)**

```javascript
import React from 'react';
import { render } from 'react-dom';
import { createStore } from 'redux';
import { Provider } from 'react-redux';
import App from './App.jsx';
import todoApp from './reducers/reducers';
 
let store = createStore(todoApp);
let rootElement = document.getElementById('app');
 
render(
   <Provider store={store}>
      <App />
   </Provider>,
   rootElement
);

```

---

**Modern Redux Toolkit (RTK) Implementation (Functional Components & Hooks)**

**1. Installation**

```bash
npm install @reduxjs/toolkit react-redux

```

**2. Slice Definition (`features/todoSlice.js`)**

```javascript
import { createSlice } from '@reduxjs/toolkit';

let nextTodoId = 0;

const todoSlice = createSlice({
  name: 'todos',
  initialState: [],
  reducers: {
    addTodo: (state, action) => {
      // Immer allows direct mutations safely under the hood
      state.push({
        id: nextTodoId++,
        text: action.payload
      });
    }
  }
});

export const { addTodo } = todoSlice.actions;
export default todoSlice.reducer;

```

**3. Store Configuration (`store.js`)**

```javascript
import { configureStore } from '@reduxjs/toolkit';
import todoReducer from './features/todoSlice';

export const store = configureStore({
  reducer: {
    todos: todoReducer
  }
});

```

**4. Component Integration (`App.jsx`)**

```jsx
import React, { useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { addTodo } from './features/todoSlice';

function AddTodo() {
  const [text, setText] = useState('');
  const dispatch = useDispatch();

  const handleAdd = (e) => {
    e.preventDefault();
    if (text.trim()) {
      dispatch(addTodo(text.trim()));
      setText('');
    }
  };

  return (
    <form onSubmit={handleAdd}>
      <input 
        type="text" 
        value={text} 
        onChange={(e) => setText(e.target.value)} 
      />
      <button type="submit">Add</button>
    </form>
  );
}

function TodoList() {
  const todos = useSelector((state) => state.todos);

  return (
    <ul>
      {todos.map((todo) => (
        <li key={todo.id}>{todo.text}</li>
      ))}
    </ul>
  );
}

export default function App() {
  return (
    <div>
      <AddTodo />
      <TodoList />
    </div>
  );
}

```

**5. Entry Point (`index.jsx`)**

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';
import { Provider } from 'react-redux';
import { store } from './store';
import App from './App';

const root = ReactDOM.createRoot(document.getElementById('root'));
root.render(
  <Provider store={store}>
    <App />
  </Provider>
);

```

---

**Global State Management Comparison**

| Strategy | Ideal Use Case | Pros | Cons |
| --- | --- | --- | --- |
| **Context API** | Low-frequency data (themes, auth, locales) | Built-in native API; no extra bundle size | Frequent updates trigger widespread re-renders |
| **Redux / Toolkit** | Large enterprise apps with complex inter-dependent data | Centralized store, predictable actions, robust dev tools | Boilerplate setup; excessive for smaller applications |
| **Modern Alternatives (Zustand / Jotai)** | Medium-to-large apps seeking lightweight state | Simple APIs, granular state subscriptions, minimal setup | External library dependency |

---

**Performance Optimization Checklist**

1. **Prevent Unnecessary Component Re-renders:** Wrap static functional child components with `React.memo` and class components with `React.PureComponent` or `shouldComponentUpdate()`.
2. **Stabilize Reference Prop Signatures:** Wrap inline object properties or array calculations in `useMemo` and functions in `useCallback` to preserve reference equality between renders.
3. **State Colocation:** Keep state definitions as close as possible to the components that consume them, avoiding unnecessary parent re-renders.
4. **Code-Splitting via Bundler Chunking:** Split heavy application routes using `React.lazy()` dynamic imports combined with `<Suspense>` boundaries.
5. **List Virtualization:** Use windowing libraries like `react-window` or `react-virtualized` to render only currently visible DOM nodes in long data arrays.

---

**Core Redux & Modern RTK Interview Questions**

**What is Redux Toolkit (RTK) and why is it recommended over Legacy Redux?**

Redux Toolkit (RTK) is the official, opinionated toolset for efficient Redux development created to eliminate legacy boilerplate code.

* **Simplified Configuration:** `configureStore` sets up store creation with default middleware (including Redux Thunk and immutability checks) and Redux DevTools automatically.
* **Reduced Boilerplate:** `createSlice` replaces separate action type constants, action creators, and reducer `switch` statements into a single unified object.
* **Mutating Syntax:** Integrates the Immer library, allowing developers to write intuitive "mutating" state logic that safely produces immutable updates under the hood.

**How does `createSlice` work in RTK?**

`createSlice` is a higher-level function that accepts an initial state, an object of reducer functions, and a "slice name". It automatically generates corresponding action creators and action type strings for each defined reducer.

```javascript
const userSlice = createSlice({
  name: 'user',
  initialState: { name: '', isAuthenticated: false },
  reducers: {
    login: (state, action) => {
      state.name = action.payload;
      state.isAuthenticated = true;
    },
    logout: (state) => {
      state.name = '';
      state.isAuthenticated = false;
    }
  }
});

```

**How does Immer enable direct state mutation in RTK reducers?**

Under the hood, RTK wraps reducers inside Immer's `produce` function. Immer creates a temporary "Draft State" based on the current state. When state properties are directly mutated (e.g., `state.todos.push(newItem)`), Immer tracks those modifications and produces an entirely new, immutable state copy for Redux.

**How do you handle asynchronous operations in Modern Redux using `createAsyncThunk`?**

`createAsyncThunk` accepts an action type string and a payload creator function returning a Promise. It automatically generates action creators for `pending`, `fulfilled`, and `rejected` states, which are handled in a slice's `extraReducers`.

```javascript
import { createSlice, createAsyncThunk } from '@reduxjs/toolkit';

export const fetchUsers = createAsyncThunk('users/fetch', async () => {
  const response = await fetch('/api/users');
  return await response.json();
});

const userSlice = createSlice({
  name: 'users',
  initialState: { data: [], loading: false, error: null },
  reducers: {},
  extraReducers: (builder) => {
    builder
      .addCase(fetchUsers.pending, (state) => {
        state.loading = true;
      })
      .addCase(fetchUsers.fulfilled, (state, action) => {
        state.loading = false;
        state.data = action.payload;
      })
      .addCase(fetchUsers.rejected, (state, action) => {
        state.loading = false;
        state.error = action.error.message;
      });
  }
});

```

**What is RTK Query and how does it differ from traditional Redux Thunks?**

RTK Query is an advanced data-fetching and caching tool built into `@reduxjs/toolkit`.

* **Traditional Thunks:** Require manual tracking of loading states, error handling, caching logic, and lifecycle dispatchers inside reducers.
* **RTK Query:** Eliminates manual data-fetching code by auto-generating React hooks (`useGetUsersQuery`) that automatically manage request caching, background re-validation, polling, loading states, and deduplication out of the box.

**What are Redux Selectors and how does `createSelector` optimize performance?**

Selectors are functions that extract specific pieces of state from the Redux store. `createSelector` (from Reselect/RTK) creates memoized selectors that compute derived data:

* It recalculates output only when its specific input arguments change.
* It prevents costly re-computations and stops React components from re-rendering when unrelated store state changes.

```javascript
import { createSelector } from '@reduxjs/toolkit';

const selectTodos = (state) => state.todos;

export const selectCompletedTodos = createSelector(
  [selectTodos],
  (todos) => todos.filter((todo) => todo.completed)
);

```

**How do custom hooks like `useAppDispatch` and `useAppSelector` improve TypeScript integration in Redux Toolkit?**

Rather than importing generic `useDispatch` and `useSelector` hooks in every component, RTK recommends creating typed hook aliases using store types (`RootState` and `AppDispatch`). This provides instant autocompletion and strict type verification across component state selections and action dispatches without extra casting.

---

**Technical Questions**

**How do you design an optimal decoupled Headless WordPress architecture utilizing a React 19 single-page layout front-end backed by a PHP WPGraphQL system?**

An optimal architecture separates front-end layout concerns from content modeling layers through a decoupled framework model:

1. **Data Extraction Layer:** Replace traditional WordPress PHP page loops by querying a headless server via **WPGraphQL** or the WordPress REST API. This configuration enables optimized bulk requests, nested relational processing, and precise data delivery.
2. **Authentication Protocols:** Secure authenticated interactions (like posting content or managing user metrics) using **JWT (JSON Web Tokens)** passed securely inside HTTPOnly server cookies to shield client keys from cross-site scripting (XSS) risks.
3. **Static Optimizations:** Use frameworks like Next.js or Remix to implement **Incremental Static Regeneration (ISR)**. This architecture triggers a background webhook fetch on the PHP server whenever content changes inside the WordPress admin dashboard, re-compiling static pages on the fly without requiring full front-end rebuilds.

*Implementation Example:*

Executing GraphQL queries inside React functional components via GraphQL client instances:

```jsx
import React, { useEffect, useState } from 'react';

const POSTS_QUERY = `
  query GetPosts {
    posts {
      nodes {
        id
        title
        slug
        excerpt
      }
    }
  }
`;

export function WordPressBlogList() {
  const [posts, setPosts] = useState([]);
  const [loading, setLoading] = useState(true);

  useEffect(() => {
    async function fetchWPPosts() {
      try {
        const response = await fetch('https://cms.yourdomain.com/graphql', {
          method: 'POST',
          headers: { 'Content-Type': 'application/json' },
          body: JSON.stringify({ query: POSTS_QUERY }),
        });
        const { data } = await response.json();
        setPosts(data?.posts?.nodes || []);
      } catch (error) {
        console.error('WPGraphQL Query Failed:', error);
      } finally {
        setLoading(false);
      }
    }
    fetchWPPosts();
  }, []);

  if (loading) return <div>Fetching Headless GraphQL Content...</div>;

  return (
    <main>
      <h2>WordPress Decoupled Posts</h2>
      {posts.map((post) => (
        <article key={post.id}>
          <h3>{post.title}</h3>
          <div dangerouslySetInnerHTML={{ __html: post.excerpt }} />
        </article>
      ))}
    </main>
  );
}
```

**As a Solution Architect, how do you handle cross-origin authorization, state consistency, and mitigation of CSRF risks when linking React applications with backend framework systems running PHP (Laravel/Symfony)?**

Securing communications across standalone React client build setups and centralized PHP servers requires a multi-layered security and transport design:

* **CORS Management Configuration:** Define strict explicit parameters within PHP frameworks (e.g., Laravel's `cors.php` file). Explicitly map targeted domain permissions via `allowed_origins` to block external browser execution origins, and enforce `supports_credentials: true` to support cross-domain cookie transfers.
* **CSRF Attack Defenses:** Use token verification middleware like **Laravel Sanctum**. The React application makes an initial handshake call to a specialized cookie endpoint (`/sanctum/csrf-cookie`). The PHP server delivers an encrypted anti-forgery value inside an `XSRF-TOKEN` cookie wrapper. The front-end client reads this token value and attaches it to the headers of subsequent mutable API calls as an `X-XSRF-TOKEN` parameter, verifying request authenticity.
* **Session Management Synchronization:** Maintain state consistency by persisting transient authentication data inside a type-safe client store (e.g., Zustand/Redux Toolkit). Use a root interceptor wrapper (like an Axios or Fetch interceptor instance) to automatically capture `401 Unauthorized` response signals from the PHP API, gracefully resetting application state and re-routing expired sessions to login layouts.

*Implementation Example:*

Setting up secure cross-origin Axios API instances handling CSRF headers and unauthorized interceptors:

```jsx
import axios from 'axios';

const api = axios.create({
  baseURL: 'https://api.yourdomain.com',
  withCredentials: true, // Enables sending HTTPOnly auth cookies cross-origin
  headers: {
    'X-Requested-With': 'XMLHttpRequest',
    'Accept': 'application/json',
  },
});

// Step 1: Handshake call to fetch CSRF Cookie from Laravel Sanctum
export async function initializeCsrfHandshake() {
  await api.get('/sanctum/csrf-cookie');
}

// Step 2: Global interceptor handling 401 unauthenticated session expirations
api.interceptors.response.use(
  (response) => response,
  (error) => {
    if (error.response && error.response.status === 401) {
      console.warn('Session expired. Purging client auth state...');
      window.location.href = '/login';
    }
    return Promise.reject(error);
  }
);

export default api;
```

**How should a Technical Lead implement micro-frontend patterns inside an enterprise React ecosystem, and what mechanisms prevent styling conflicts across federated code slices?**

A Technical Lead can decouple large enterprise applications into independent deployment lifecycles using **Webpack/Vite Module Federation**:

1. **Orchestration Design:** Configure a master shell workspace host that references independently deployed remote build locations using dynamic entry files (`remoteEntry.js`). This layout uses lazy loading wrappers to download federated modules on demand over the network.
2. **State Management & Communications:** Share global parameters across micro-frontends via native **CustomEvents** or isolated global state instances attached to the browser's root `window` namespace, preventing coupling between separate repository structures.
3. **Isolation of CSS Styles:** Prevent styling bleeding and global namespace contamination by using strict build compilation isolation paradigms:
* **CSS Modules:** Automatically appends unique hash identifiers to compilation selectors.
* **Shadow DOM Wrappers:** Encapsulates sub-application blocks inside strict web-component boundaries, completely shielding components from external style sheets.
* **Tailwind CSS Custom Prefixing:** Configures specific identifier tags (`prefix: 'mfe-one-'`) inside micro-frontend build files to differentiate compiled style classes.


**Explain how you can optimize a traditional legacy monolithic WooCommerce checkout interface by injecting an optimized embedded React single-page mini-application.**

Migrating high-conversion transaction points from traditional PHP server renders to an asynchronous React app requires a step-by-step decoupling process:

1. **DOM Node Injection:** Hook into traditional theme templates using standard WordPress filters (like `woocommerce_checkout_before_customer_details`). Inject a dedicated target mounting container (`<div id="react-checkout-gateway"></div>`), while using `wp_dequeue_script` to disable core monolithic checkout scripts.
2. **Asset Dependencies Enqueuing:** Compile the React application into optimized, versioned production code chunks. Enqueue these build chunks within the host WordPress application layout using `wp_enqueue_script` and `wp_enqueue_style`.
3. **Local Context Binding & State Hydration:** Inject session states and server runtime constants (such as system currency rules, shipping matrix states, or active localization rules) into the window context using `wp_localize_script`:
```php
wp_localize_script('react-checkout-bundle', 'wcReactSettings', [
    'ajaxUrl'   => admin_url('admin-ajax.php'),
    'restNonce' => wp_create_nonce('wp_rest'),
    'cartTotal' => WC()->cart->get_total()
]);
```
4. **API Transaction Routing:** Direct user modifications to custom endpoints built on top of the WordPress REST API infrastructure. These endpoints manage background cart actions and execute checkout forms asynchronously, returning uniform JSON results back to the React front-end application.

*Implementation Example:*

Client-Side React entry mounting point inspecting localized PHP settings context:

```jsx
import React from 'react';
import ReactDOM from 'react-dom/client';

function EmbeddedWooCommerceCheckout() {
  // Extract server variables hydrated by PHP wp_localize_script()
  const settings = window.wcReactSettings || {};

  return (
    <div className="embedded-checkout-card">
      <h3>Express React Checkout Gateway</h3>
      <p>Current Total: <strong>{settings.cartTotal}</strong></p>
      <button 
        onClick={() => {
          fetch('/wp-json/wc/v3/checkout', {
            method: 'POST',
            headers: { 'X-WP-Nonce': settings.restNonce }
          });
        }}
      >
        Complete Order Asynchronously
      </button>
    </div>
  );
}

// Mount embedded React application onto injected WordPress container
const container = document.getElementById('react-checkout-gateway');
if (container) {
  const root = ReactDOM.createRoot(container);
  root.render(<EmbeddedWooCommerceCheckout />);
}
```

**What are some best practices for performance optimization in React?**

Key performance optimization practices include:

* Implementing component memoization wrappers (`React.memo`) to block redundant render passes.
* Stabilizing object allocations and functional references via the `useMemo` and `useCallback` hooks.
* Splitting up oversized code bundles into dynamic pieces using lazy loading (`React.lazy` and `Suspense`).
* Virtualizing list elements (`react-window` or `react-virtualized`) to render only the visible viewport items.
* Colocating state updates as close to the target consumer nodes as possible to isolate update scopes.

*Implementation Example:*

Optimizing renders with React.memo, useCallback, and list virtualization using react-window:

```jsx
import React, { useState, useCallback } from 'react';
import { FixedSizeList as List } from 'react-window';

// Memoized child component to avoid unnecessary re-renders
const ListRow = React.memo(({ index, style, data }) => {
  const item = data.items[index];
  return (
    <div style={style} className="list-row">
      <span>{item.name}</span>
      <button onClick={() => data.onSelect(item.id)}>Select</button>
    </div>
  );
});

export function OptimizedDataList({ items }) {
  const [selectedId, setSelectedId] = useState(null);

  // Stabilize function reference to preserve React.memo optimization in ListRow
  const handleSelect = useCallback((id) => {
    setSelectedId(id);
  }, []);

  return (
    <div>
      <p>Selected ID: {selectedId}</p>
      <List
        height={400}
        itemCount={items.length}
        itemSize={35}
        width={300}
        itemData={{ items, onSelect: handleSelect }}
      >
        {ListRow}
      </List>
    </div>
  );
}
```

**How does React handle testing and what are some popular testing frameworks for React?**

React tests component logic by verifying behavior, rendering accuracy, state updates, and user event reactions. Popular testing tools include:

* **Jest** or **Vitest:** Serves as the test runner, assertion engine, and mocking interface.
* **React Testing Library (RTL):** Provides specialized component mount environments to test elements based on user accessibility queries (like roles or visible text), prioritizing real-world user interactions over internal state implementations.

```jsx
import React, { useState } from 'react';
import { render, screen, fireEvent } from '@testing-library/react';
import { describe, it, expect } from 'vitest';

function CounterComponent() {
  const [count, setCount] = useState(0);
  return (
    <div>
      <p data-testid="count-val">Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

describe('CounterComponent Unit Tests', () => {
  it('renders initial state and increments count on user click', () => {
    render(<CounterComponent />);

    // Assert initial state
    expect(screen.getByTestId('count-val')).toHaveTextContent('Count: 0');

    // Simulate user clicking increment button
    const button = screen.getByRole('button', { name: /increment/i });
    fireEvent.click(button);

    // Assert state change in UI
    expect(screen.getByTestId('count-val')).toHaveTextContent('Count: 1');
  });
});
```

**How do you handle asynchronous data loading in React?**

Asynchronous data loading can be managed by calling standard network request abstractions (such as the browser's native `fetch` API or the `axios` library) inside a `useEffect` hook, then saving the resolved payload into local component state. Advanced applications typically use specialized data synchronization libraries like **TanStack Query (React Query)** or **RTK Query**. These tools handle caching, automatic background revalidation, error updates, loading states, and deduplication out of the box.

*Implementation Example:*

Modern asynchronous state management using TanStack Query (React Query):

```jsx
import React from 'react';
import { useQuery } from '@tanstack/react-query';

async function fetchUserProfiles() {
  const res = await fetch('/api/users');
  if (!res.ok) throw new Error('Failed to fetch user profiles');
  return res.json();
}

export function UserProfilesContainer() {
  // React Query automatically handles loading, error, caching, and deduplication
  const { data: users, isLoading, isError, error } = useQuery({
    queryKey: ['users'],
    queryFn: fetchUserProfiles,
    staleTime: 1000 * 60 * 5, // Cache for 5 minutes
  });

  if (isLoading) return <div>Loading asynchronous data...</div>;
  if (isError) return <div>Error loading data: {error.message}</div>;

  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

---
## 🆕 Advanced React 19 Features & Interview Questions

---

### Q53: Explain the `useOptimistic` hook introduced in React 19. Provide a practical implementation program.

**Ans:** `useOptimistic` is a React 19 hook designed to deliver a zero-latency user experience by predicting the success of background asynchronous mutations (like API updates or database writes). It instantly transforms the UI to show the "expected outcome". If the network operation settles successfully, the final state persists; if it fails, React automatically rolls back the interface to the verified source-of-truth state.

#### 🛠️ Implementation Program

```javascript
import React, { useOptimistic, transition } from 'react';

// Mock API endpoint mutation
async function createTodoOnServer(title) {
  await new Promise((resolve, reject) => setTimeout(resolve, 1500)); // Simulate latency
  if (title.toLowerCase() === "error") throw new Error("Server rejected request");
  return { id: Date.now(), title };
}

export function OptimisticTodoList({ initialTodos }) {
  // state holds our real, verified database entries
  const [todos, setTodos] = React.useState(initialTodos);

  // useOptimistic hooks into the real state and maps out temporary updates
  const [optimisticTodos, setOptimisticTodos] = useOptimistic(
    todos,
    (currentTodos, newTodoTitle) => [
      ...currentTodos,
      { id: 'temp-id', title: newTodoTitle, isSending: true } // Optimistic item payload
    ]
  );

  async function formActionHandler(formData) {
    const title = formData.get("todoTitle");
    if (!title) return;

    // 1. Instantly inject the predictive state inside an async transition
    setOptimisticTodos(title);

    try {
      // 2. Execute actual network network dispatch
      const newTodo = await createTodoOnServer(title);
      // 3. Update the stable source-of-truth state on success
      setTodos(prev => [...prev, newTodo]);
    } catch (error) {
      console.error("Mutation failed! Rollback automatically triggered:", error.message);
    }
  }

  return (
    <div>
      <form action={formActionHandler}>
        <input type="text" name="todoTitle" placeholder="Add a task..." />
        <button type="submit">Submit</button>
      </form>

      <ul>
        {optimisticTodos.map(todo => (
          <li key={todo.id} style={{ opacity: todo.isSending ? 0.5 : 1 }}>
            {todo.title} {todo.isSending && <span>(Sending...)</span>}
          </li>
        ))}
      </ul>
    </div>
  );
}

```

---

### Q54: What is the `useFormStatus` hook in React 19 and why can it not be called within the same component that declares the `<form>` element?

**Ans:** `useFormStatus` is an executive utility hook that allows deeply nested layout elements to extract runtime metrics about a parent `<form>` submission without manually passing props or setting up complex context providers. It yields four clean context properties: `pending`, `data`, `method`, and `action`.

* **The Crucial Hook Constraint:** `useFormStatus` acts like a standard Context consumer. It evaluates the state of the **nearest parent `<form>` boundary** above it in the component tree. If called in the same component wrapper that renders the `<form>` element, it will return `undefined` or a false state because it cannot read a boundary that lives parallel to or below its own initialization execution loop.

#### 🛠️ Implementation Program

```javascript
import React from 'react';
import { useFormStatus } from 'react-dom';

// 1. Nested Child Component (Safely reads parent state)
function SubmitButton() {
  const { pending } = useFormStatus(); // Extracts status from the nearest form ancestor

  return (
    <button type="submit" disabled={pending}>
      {pending ? 'Processing Entry...' : 'Save Document'}
    </button>
  );
}

// 2. Parent Container Component
export function MasterDataForm() {
  async function handleAction(formData) {
    await new Promise((resolve) => setTimeout(resolve, 2000)); // Simulate async work
    alert(`Saved: ${formData.get("username")}`);
  }

  return (
    <form action={handleAction}>
      <label>Username</label>
      <input type="text" name="username" required />
      
      {/* 
        CRITICAL: SubmitButton must live inside the form element tag 
        for useFormStatus to accurately catch state loops. 
      */}
      <SubmitButton />
    </form>
  );
}

```

---

### Q55: What are the modern Resource Loading APIs introduced in React 19? How do you use them to optimize asset discovery?

**Ans:** React 19 introduces native, imperative **Resource Loading APIs** (`preload`, `preinit`, `prefetchDNS`, and `preconnect`) directly within the core library workspace. Instead of relying on manual link injection tricks inside secondary `useEffect` runs, these methods allow developers to programmatically signal asset priorities straight to the browser engine during the initial component render pass, cutting down on layout shifting and flash animations.

#### 🛠️ Implementation Program

```javascript
import React from 'react';
import { prefetchDNS, preconnect, preload, preinit } from 'react-dom';

export function MediaRichInterface() {
  // 1. Warm up third-party asset domain discovery routines early
  prefetchDNS("https://fonts.googleapis.com");
  preconnect("https://api.example-cdn.com");

  // 2. Force background retrieval workflows for layout stylesheets
  preload("https://api.example-cdn.com/styles/heavy-widgets.css", { as: "style" });

  // 3. Inject and run crucial operational script files immediately
  preinit("https://api.example-cdn.com/scripts/analytics-bootstrap.js", { as: "script" });

  return (
    <section className="dashboard-layout-container">
      <h1>Optimized Interface Asset Terminal</h1>
      <p>High-priority global resources are preloaded directly during component compilation.</p>
    </section>
  );
}

```

---

### Q56: How does React 19 improve error reporting, and how do you configure custom error logs within `createRoot`?

**Ans:** Prior to React 19, errors caught by an Error Boundary were frequently re-thrown and logged multiple times across the ecosystem, creating cluttered and confusing browser console traces. React 19 fixes this by introducing structured, top-level error lifecycle hooks directly inside the root initialization configuration layer (`createRoot` and `hydrateRoot`).

This allows architects to decouple run-time errors into distinct buckets—uncaught errors, caught boundaries, and hydration mismatches—allowing teams to forward clean crash metrics straight to external monitoring services (like Sentry or LogRocket) without polluting local developer consoles.

#### 🛠️ Implementation Program

```javascript
import React from 'react';
import { createRoot } from 'react-dom/client';
import App from './App';

const container = document.getElementById('root');

// Initializing application roots with absolute control over error tracing vectors
const root = createRoot(container, {
  // Hook 1: Invoked when application errors escape your local Error Boundary setups
  onUncaughtError: (error, errorInfo) => {
    console.warn("Global Root Intercepted an Uncaught Error:");
    // Send telemetry to monitoring services
    // sendToSentry({ error, componentStack: errorInfo.componentStack });
  },

  // Hook 2: Invoked when errors are gracefully caught and stopped by an Error Boundary
  onCaughtError: (error, errorInfo) => {
    console.info("Telemetry Log: Error safely captured by boundary component.");
    // Log component stack metrics cleanly
    // logErrorService(error.message, errorInfo.componentStack);
  },

  // Hook 3: Invoked when the server-rendered HTML fails to match the client's initial execution layout
  onHydrationMismatch: (error) => {
    console.error("Critical System Warning: Hydration mismatch discovered:", error.message);
  }
});

root.render(<App />);

```