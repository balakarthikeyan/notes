# 1. What is Redux?

**Definition/Explanation**
Redux is a predictable state management library for JavaScript applications. It consolidates application state into a single store and modifies that state exclusively by dispatching actions to reducers. In React applications, Redux is designed for state needed by many components, state that benefits from action debugging history, or server/cache state managed through Redux Toolkit Query. Rather than treating Redux as "global variables for React," it should be viewed as a system for state transitions that require structure, traceability, and consistent reads across distant components.

**Workflow / Architecture**

```text
UI Event ──> Dispatch(Action) ──> Reducer ──> Updated Store State ──> UI Re-render

```

---

# 2. What Problem Does Redux Solve?

**Definition/Explanation**
Redux resolves challenges around deep prop chains through multiple component layers, shared state reads across distant components, and complex state transitions requiring a traceable action history. It is not required for every piece of application state; localized state—such as input field text, modal visibility, hover states, and component-specific behavior—belongs in React local state.

**Use Case Comparison**

* **React Local State**: Text input fields, modal open/close toggles, hover states, and component-only state.


* **Redux State Payoff**: Shopping cart contents, user authentication session metadata, feature flags, document editor state, and cross-page notification queues.



---

# 3. What Are the Three Core Redux Concepts?

**Definition/Explanation**
The architecture of Redux relies on three primary building blocks:

* **Store**: The central object that holds the application's state tree.


* **Actions**: Plain JavaScript objects describing an event that occurred.


* **Reducers**: Pure functions that take the previous state and an action, and compute the next state.



---

# 4. What is a Redux Store?

**Definition/Explanation**
The Redux store is the central object containing the complete state tree of the application. It exposes methods to read state via `getState()`, trigger updates via `dispatch(action)`, and attach listener callbacks via `subscribe()`. In modern Redux, stores are instantiated using Redux Toolkit's `configureStore()` rather than the legacy `createStore()` API.

**Example**

```javascript
import { configureStore } from '@reduxjs/toolkit';

export const store = configureStore({
  reducer: {
    cart: cartReducer,
    user: userReducer,
  },
});

```

---

# 5. What is an Action?

**Definition/Explanation**
An action is a plain JavaScript object representing an event that occurred in the application. It requires a `type` string field and optional `payload` data. Action names should describe events that happened rather than setter commands (e.g., `cart/itemAdded` is preferred over `SET_CART`).

**Example**

```javascript
{
  type: "cart/itemAdded",
  payload: { id: "p1", quantity: 1 }
}

```

---

# 6. What is a Reducer?

**Definition/Explanation**
A reducer is a pure function that calculates the next application state given the previous state and an action. Reducers must remain purely deterministic: they must never fetch network data, mutate external variables, generate random numbers, read the current time, or dispatch secondary actions. Any asynchronous or non-deterministic work (like reading time or localStorage) belongs before the action dispatch or within middleware/thunks.

**Example**

```typescript
function counterReducer(state = { value: 0 }, action: { type: string }) {
  if (action.type === 'counter/incremented') {
    return { value: state.value + 1 };
  }
  return state;
}

```

---

# 7. Why Must Reducers Be Pure?

**Definition/Explanation**
Reducer purity makes state changes deterministic and predictable. Given identical inputs (previous state and action), a pure reducer guarantees the exact same next state. This predictability facilitates straightforward unit testing, action replaying, time-travel debugging, and audit trails within Redux DevTools.

---

# 8. What is Dispatch?

**Definition/Explanation**
`dispatch()` is the method used to send action objects to the Redux store. Once dispatched, the store executes the root reducer using the current state and the dispatched action, storing the resulting new state. In React, components dispatch actions using the `useDispatch()` hook.

**Example**

```javascript
const dispatch = useDispatch();

dispatch(cartItemAdded({ id: 'p1' }));

```

---

# 9. What is Unidirectional Data Flow in Redux?

**Definition/Explanation**
Redux strictly enforces a single-directional data flow cycle to prevent unpredictable side effects across independent UI nodes.

**Flow Steps**

1. User interaction in the UI triggers an event.


2. The application dispatches an action object.


3. Reducer functions process the action and calculate the next state.


4. The Redux store updates, notifying subscriber components to read the new state and re-render.



---

# 10. What is Redux Toolkit (RTK)?

**Definition/Explanation**
Redux Toolkit (RTK) is the official standard for writing modern Redux logic. It provides abstractions such as `configureStore()`, `createSlice()`, `createAsyncThunk()`, and RTK Query. RTK eliminates setup boilerplate, establishes safe store defaults, warns against common mistakes (like non-serializable values), and integrates Immer to enable draft mutation syntax inside reducers.

---

# 11. What is `configureStore()`?

**Definition/Explanation**
`configureStore()` is an RTK helper that sets up a Redux store with development-ready defaults. It automatically combines slice reducers, configures default middleware (including thunks and mutation/serializability checks), and connects Redux DevTools support.

**Example**

```typescript
import { configureStore } from '@reduxjs/toolkit';

export const store = configureStore({
  reducer: {
    cart: cartReducer,
    user: userReducer,
  },
});

```

---

# 12. What is `createSlice()`?

**Definition/Explanation**
`createSlice()` is an RTK function that encapsulates a feature's initial state, reducers, generated action creators, and action types in one definition. While the code inside its reducers looks like direct mutation, RTK uses Immer under the hood to perform safe immutable updates.

**Example**

```javascript
import { createSlice } from '@reduxjs/toolkit';

const counterSlice = createSlice({
  name: 'counter',
  initialState: { value: 0 },
  reducers: {
    incremented(state) {
      state.value += 1; // Handled safely by Immer
    },
  },
});

```

Note: Draft mutation syntax is only valid inside Immer-powered reducers (such as those inside `createSlice()`). Immutable update rules must still be followed outside this boundary.

---

# 13. What is a Slice in Redux?

**Definition/Explanation**
A slice represents the Redux state, actions, and reducer logic for a single application domain or feature (such as `cart`, `auth`, or `todos`). Organizing code into feature-based slices keeps related logic localized in one file rather than split across separate folders for actions, reducers, and constants.

---

# 14. How Does Redux Toolkit Allow "Mutating" Reducer Code?

**Definition/Explanation**
Redux Toolkit embeds the Immer library, which intercepts state modifications by providing a temporary `draft` version of state inside the reducer. Developers write standard JavaScript object mutations against this draft, and Immer calculates the resulting immutable state tree automatically. While `state.value += 1` is safe inside a `createSlice()` reducer, direct object mutation in hand-written plain reducers remains an anti-pattern.

---

# 15. What is Immutability in Redux?

**Definition/Explanation**
Immutability dictates that existing state objects must never be directly modified in place. Instead, state updates must produce new object or array references for changed parts while maintaining references to unchanged parts. Redux relies on reference equality (`===`) to detect changes; mutating an existing state object in place prevents React Redux from detecting updates and re-rendering components.

---

# 16. What is React Redux?

**Definition/Explanation**
React Redux is the official binding library that integrates React component hierarchies with a Redux store. It provides core APIs including the `<Provider>` component, as well as the `useSelector()` and `useDispatch()` hooks.

---

# 17. What Does `<Provider>` Do?

**Definition/Explanation**
The `<Provider>` component wraps the React application and passes the Redux store down through React Context. This makes the store accessible to any nested component using React Redux hooks (`useSelector()`, `useDispatch()`).

**Example**

```jsx
import { Provider } from 'react-redux';
import { store } from './store';

export default function App() {
  return (
    <Provider store={store}>
      <RootComponent />
    </Provider>
  );
}

```

---

# 18. What is `useSelector()`?

**Definition/Explanation**
`useSelector()` is a React Redux hook that extracts data from the Redux store state. It subscribes to the store and triggers a component re-render whenever the selected state changes. Components should select the minimal subset of data required to minimize unnecessary re-renders.

**Optimal vs. Suboptimal Selection**

```javascript
// Recommended: Extract specific values independently
const cartCount = useSelector((state: RootState) => state.cart.items.length);

// Anti-pattern: Returning a new object reference on every store update triggers unnecessary re-renders
const cart = useSelector((state) => ({
  count: state.cart.items.length,
  total: state.cart.total,
}));

```

---

# 19. What is `useDispatch()`?

**Definition/Explanation**
`useDispatch()` is a React Redux hook that returns the Redux store's `dispatch` function, allowing React components to dispatch actions. In TypeScript applications, teams typically create a custom pre-typed `useAppDispatch()` hook based on the store's dispatch type.

---

# 20. What Are Selectors?

**Definition/Explanation**
Selectors are reusable functions that accept the Redux state tree and extract specific pieces of data. They abstract state structure away from components; if the underlying Redux store layout changes, updates are made to the selector functions rather than every consuming component.

**Example**

```javascript
const selectCartItems = (state: RootState) => state.cart.items;

```

---

# 21. What Are Memoized Selectors?

**Definition/Explanation**
Memoized selectors (typically created with Reselect) cache calculated outputs based on input arguments. They are recommended when performing expensive derivations (such as filtering or sorting lists) or when returning new object/array references that would otherwise trigger unnecessary component re-renders.

---

# 22. How Do You Handle Async Logic in Redux?

**Definition/Explanation**
Because reducers must remain pure functions, all asynchronous side-effects must occur outside reducers. Common approaches include Redux Thunks, `createAsyncThunk()`, RTK Query, or custom middleware.

**Standard Async Cycle**

```text
Dispatch Pending Action ──> Execute Network Request ──> Dispatch Success/Failure Action ──> Update State in Reducer

```

* Use **RTK Query** for general server data caching and CRUD.


* Use **Thunks** for complex workflows that require reading state (`getState`), triggering multiple actions, or combining API data with client state.



---

# 23. What is a Thunk?

**Definition/Explanation**
A thunk is a function that wraps asynchronous logic and delayed actions. Redux Toolkit includes thunk middleware out of the box. Thunks receive `dispatch` and `getState` as arguments, enabling asynchronous operations before dispatching plain action objects.

**Example**

```javascript
export const fetchUser = (id: string) => async (dispatch: AppDispatch) => {
  dispatch(userRequested());
  try {
    const user = await api.getUser(id);
    dispatch(userReceived(user));
  } catch (error) {
    dispatch(userFailed(error));
  }
};

```

---

# 24. What is `createAsyncThunk()`?

**Definition/Explanation**
`createAsyncThunk()` is an RTK abstraction designed for standard HTTP requests. It accepts an action type string and a payload creator promise function, automatically generating `pending`, `fulfilled`, and `rejected` action types. Slices handle these lifecycle states inside `extraReducers`.

---

# 25. What is RTK Query?

**Definition/Explanation**
RTK Query is an advanced data-fetching and caching tool included within Redux Toolkit. It automatically generates React hooks for queries and mutations, caches network responses, deduplicates concurrent requests, and handles cache invalidation using tags. RTK Query is recommended for server state management, while standard slices handle client-owned state.

---

# 26. When Should You Use Redux Instead of React Context?

**Definition/Explanation**

* **React Context**: Best for low-frequency, dependency-style updates like UI themes, current locale, or static configuration.


* **Redux**: Recommended when state changes frequently, many components consume distinct slices of state, complex update logic is required, or debugging history and middleware are needed. Context alone lacks reducers, middleware, DevTools history, normalized state structures, and built-in caching.



---

# 27. What is Middleware in Redux?

**Definition/Explanation**
Redux middleware provides a extension point between the moment an action is dispatched and the moment it reaches the reducers. Middleware can intercept actions, generate logging, trigger side-effects, run async code, or transform actions. While reducers must remain free of side-effects, middleware is a safe location for side-effect logic.

---

# 28. What Are Redux DevTools?

**Definition/Explanation**
Redux DevTools is a browser extension that tracks dispatched actions, inspects state changes, and allows developers to review the store timeline. It features time-travel debugging (replaying state transitions), making state changes across complex apps predictable and easy to inspect.

---

# 29. What Should Not Go Into Redux State?

**Definition/Explanation**
Redux state should consist strictly of plain, serializable values. Non-serializable items—such as DOM element references, class instances, Promises, functions, and raw JavaScript `Date` instances—should never be stored in Redux. Additionally, short-lived component UI state (such as input field text or hover flags) should remain in local component state rather than global Redux state.

---

# 30. How Do You Test Redux Logic?

**Definition/Explanation**
Because reducers are pure functions, unit testing them involves providing an initial state and an action, then asserting on the returned state. Connected components should be tested using integration tests with a configured Redux test store. For asynchronous workflows, tests should mock network calls at the HTTP boundary rather than testing internal implementation details.

**Example Reducer Test**

```javascript
test('should handle adding item to empty cart', () => {
  const initialState = { items: [] };
  const action = itemAdded({ id: 'p1' });
  
  const result = cartReducer(initialState, action);

  expect(result).toEqual({
    items: [{ id: 'p1', quantity: 1 }],
  });
});

```