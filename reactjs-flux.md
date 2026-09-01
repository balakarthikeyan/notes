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