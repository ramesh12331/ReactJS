
# ⚙️ ReactJS — Chapter 33
# Redux Toolkit

---

# 🎯 What You Will Learn

In this chapter:

- ⚙️ What is Redux Toolkit?
- 🤔 Why Redux Toolkit?
- 🆚 Redux vs Redux Toolkit
- 🏪 `configureStore()`
- ✂️ `createSlice()`
- 🔄 Reducers
- 📨 Actions
- 🎁 Payload
- 🔍 `useSelector()`
- 🚀 `useDispatch()`
- 🛠️ Redux DevTools
- 🏗️ Project structure
- 🔢 Complete Counter App
- 🧪 Dry run
- ⚠️ Common mistakes
- 💼 Interview questions
- 📝 Practice
- 🧩 Mini challenge
- 📝 Quiz

---

# 1. ⚙️ What is Redux Toolkit?

**Redux Toolkit**, commonly called **RTK**, is the recommended way to write Redux logic.

It provides tools that make Redux:

- easier
- shorter
- cleaner
- more maintainable

Remember:

```text
Redux
  ↓
State management

Redux Toolkit
  ↓
Modern tools for writing Redux
```

---

# 2. 🤔 Why Do We Need Redux Toolkit?

In traditional Redux, we may have to write a lot of code.

For example:

```text
Action Types
    ↓
Actions
    ↓
Action Creators
    ↓
Reducers
    ↓
Store
```

This can become repetitive.

Redux Toolkit simplifies this.

```text
Redux Toolkit
     ↓
createSlice()
     ↓
Actions + Reducers
```

So instead of managing many separate files and pieces manually, RTK gives us a cleaner approach.

---

# 3. 🆚 Redux vs Redux Toolkit

### Traditional Redux

```text
Store
Actions
Action Types
Reducers
Dispatch
Selectors
```

More boilerplate.

### Redux Toolkit

```text
configureStore()
createSlice()
useSelector()
useDispatch()
```

Much simpler.

### Memory Trick

> **Redux is the state-management system.**

> **Redux Toolkit is the modern toolkit for writing Redux code.**

---

# 4. 🏗️ Redux Toolkit Architecture

The basic architecture is:

```text
React Component
       ↓
useDispatch()
       ↓
Action
       ↓
Slice Reducer
       ↓
Redux Store
       ↓
Updated State
       ↓
useSelector()
       ↓
React Component
```

Remember:

```text
Component
   ↓
dispatch
   ↓
slice
   ↓
store
   ↓
selector
   ↓
UI
```

---

# 5. 📦 Installing Redux Toolkit

For a React application, install:

```bash
npm install @reduxjs/toolkit react-redux
```

Two important packages:

```text
@reduxjs/toolkit
        ↓
Redux Toolkit

react-redux
        ↓
Connect Redux with React
```

---

# 6. 📁 Recommended Project Structure

A simple project can look like:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   └── counter/
│       └── counterSlice.js
│
├── App.jsx
├── main.jsx
└── index.css
```

The idea is:

```text
app/
   ↓
Store configuration

features/
   ↓
Feature-specific Redux logic
```

---

# 7. 🏪 `configureStore()`

`configureStore()` creates the Redux store using Redux Toolkit.

Example:

```js
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({
    reducer: {}
});
```

Here:

```text
configureStore()
       ↓
creates Redux store
```

---

# 8. ✂️ What is `createSlice()`?

`createSlice()` is one of the most important Redux Toolkit functions.

It lets us define:

```text
Name
Initial State
Reducers
Actions
```

in one place.

Basic syntax:

```js
const exampleSlice = createSlice({
    name: "example",

    initialState: {},

    reducers: {
        // reducer functions
    }
});
```

---

# 9. 🔢 Create Our First Counter Slice

Create:

```text
src/features/counter/counterSlice.js
```

Code:

```js
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
    count: 0
};

const counterSlice = createSlice({
    name: "counter",

    initialState,

    reducers: {

        increment: (state) => {
            state.count += 1;
        },

        decrement: (state) => {
            state.count -= 1;
        },

        reset: (state) => {
            state.count = 0;
        }

    }
});

export const {
    increment,
    decrement,
    reset
} = counterSlice.actions;

export default counterSlice.reducer;
```

---

# 10. 🔍 Understanding `createSlice()`

Look at:

```js
const counterSlice = createSlice({
```

We are creating a Redux slice.

A slice is basically one logical part of our application state.

For example:

```text
counterSlice
userSlice
cartSlice
productSlice
orderSlice
```

---

# 11. 🏷️ `name`

```js
name: "counter"
```

This identifies the slice.

Think:

```text
counter
   ↓
Counter-related Redux state
```

---

# 12. 📦 `initialState`

```js
initialState: {
    count: 0
}
```

This is the starting state.

```text
count = 0
```

---

# 13. 🔄 `reducers`

```js
reducers: {
```

Inside this object we define how the state can change.

For example:

```js
increment: (state) => {
    state.count += 1;
}
```

This means:

```text
increment
    ↓
count + 1
```

---

# 14. ➕ Increment Reducer

```js
increment: (state) => {
    state.count += 1;
}
```

Suppose:

```text
count = 5
```

After increment:

```text
count = 6
```

---

# 15. ➖ Decrement Reducer

```js
decrement: (state) => {
    state.count -= 1;
}
```

Suppose:

```text
count = 5
```

After decrement:

```text
count = 4
```

---

# 16. 🔄 Reset Reducer

```js
reset: (state) => {
    state.count = 0;
}
```

Suppose:

```text
count = 15
```

After reset:

```text
count = 0
```

---

# 17. 🤯 Important Question

You may be thinking:

> "In the previous chapter we learned that Redux state should not be directly mutated. Why are we writing `state.count += 1`?"

Excellent question.

Redux Toolkit uses **Immer** internally.

Immer lets us write code that looks like mutation:

```js
state.count += 1;
```

while producing the appropriate immutable state update internally.

So this is normal Redux Toolkit code.

---

# 18. 📨 Actions from `createSlice()`

Look at:

```js
export const {
    increment,
    decrement,
    reset
} = counterSlice.actions;
```

`createSlice()` automatically creates action creators for our reducers.

So:

```js
increment()
```

creates the corresponding Redux action.

Similarly:

```js
decrement()
```

and:

```js
reset()
```

---

# 19. 🧠 Traditional Redux vs Redux Toolkit

### Traditional Redux

You might manually define:

```js
{
    type: "INCREMENT"
}
```

and separate action logic.

### Redux Toolkit

You write:

```js
increment()
```

RTK generates the action structure for you.

Conceptually:

```text
createSlice()
     ↓
reducers
     ↓
automatic action creators
```

---

# 20. 🏪 Add Slice to Store

Now open:

```text
src/app/store.js
```

Write:

```js
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "../features/counter/counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});
```

---

# 21. 🔍 Understanding the Store

This:

```js
reducer: {
    counter: counterReducer
}
```

means our Redux state will contain:

```js
{
    counter: {
        count: 0
    }
}
```

Notice the structure.

```text
Redux Store
│
└── counter
      │
      └── count
```

Therefore:

```text
state.counter.count
```

gives us the counter value.

---

# 22. 🔗 Connect Redux with React

Now we need to provide the Redux store to our React application.

Open:

```text
main.jsx
```

Example:

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import { Provider } from "react-redux";

import App from "./App";
import { store } from "./app/store";

ReactDOM.createRoot(document.getElementById("root")).render(
    <React.StrictMode>
        <Provider store={store}>
            <App />
        </Provider>
    </React.StrictMode>
);
```

---

# 23. 🧠 What is `Provider`?

`Provider` comes from:

```js
react-redux
```

It makes the Redux store available to React components.

Think:

```text
Provider
   ↓
makes store available
   ↓
React components
```

Without the provider, components cannot use the Redux store through React-Redux hooks.

---

# 24. 🔍 `useSelector()`

`useSelector()` is used to **read data from Redux state**.

Example:

```jsx
import { useSelector } from "react-redux";

function App() {

    const count = useSelector(
        (state) => state.counter.count
    );

    return <h1>{count}</h1>;
}
```

---

# 25. 🧠 Understand `useSelector()`

This:

```js
useSelector(
    (state) => state.counter.count
);
```

means:

```text
Redux State
    ↓
counter
    ↓
count
    ↓
return count
```

If state is:

```js
{
    counter: {
        count: 5
    }
}
```

then:

```js
state.counter.count
```

returns:

```text
5
```

---

# 26. 🚀 `useDispatch()`

`useDispatch()` is used to send actions to Redux.

Example:

```jsx
import { useDispatch } from "react-redux";
import { increment } from "./features/counter/counterSlice";

function App() {

    const dispatch = useDispatch();

    return (
        <button onClick={() => dispatch(increment())}>
            Increment
        </button>
    );
}
```

---

# 27. 🔍 Understand `useDispatch()`

First:

```js
const dispatch = useDispatch();
```

Now:

```js
dispatch(increment());
```

means:

```text
Dispatch
   ↓
increment action
   ↓
counter reducer
   ↓
state changes
```

---

# 28. 🎯 Complete Counter Application

Now let's put everything together.

## `counterSlice.js`

```js
import { createSlice } from "@reduxjs/toolkit";

const initialState = {
    count: 0
};

const counterSlice = createSlice({
    name: "counter",

    initialState,

    reducers: {

        increment: (state) => {
            state.count += 1;
        },

        decrement: (state) => {
            state.count -= 1;
        },

        reset: (state) => {
            state.count = 0;
        }

    }
});

export const {
    increment,
    decrement,
    reset
} = counterSlice.actions;

export default counterSlice.reducer;
```

---

# 29. `store.js`

```js
import { configureStore } from "@reduxjs/toolkit";

import counterReducer from "../features/counter/counterSlice";

export const store = configureStore({
    reducer: {
        counter: counterReducer
    }
});
```

---

# 30. `main.jsx`

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import { Provider } from "react-redux";

import App from "./App";
import { store } from "./app/store";

ReactDOM.createRoot(document.getElementById("root")).render(
    <React.StrictMode>
        <Provider store={store}>
            <App />
        </Provider>
    </React.StrictMode>
);
```

---

# 31. `App.jsx`

```jsx
import {
    useDispatch,
    useSelector
} from "react-redux";

import {
    increment,
    decrement,
    reset
} from "./features/counter/counterSlice";

function App() {

    const count = useSelector(
        (state) => state.counter.count
    );

    const dispatch = useDispatch();

    return (
        <div>

            <h1>Redux Counter</h1>

            <h2>{count}</h2>

            <button
                onClick={() => dispatch(increment())}
            >
                +
            </button>

            <button
                onClick={() => dispatch(decrement())}
            >
                -
            </button>

            <button
                onClick={() => dispatch(reset())}
            >
                Reset
            </button>

        </div>
    );
}

export default App;
```

---

# 32. 🧪 Complete Dry Run

Initial Redux state:

```text
count = 0
```

UI:

```text
Redux Counter

0

[ + ] [ - ] [ Reset ]
```

---

## User clicks `+`

Code:

```js
dispatch(increment());
```

Flow:

```text
Button
  ↓
dispatch()
  ↓
increment()
  ↓
increment reducer
  ↓
count += 1
  ↓
count = 1
  ↓
useSelector()
  ↓
UI updates
```

Output:

```text
1
```

---

## User clicks `+` again

```text
1 + 1
```

Output:

```text
2
```

---

## User clicks `-`

```text
2 - 1
```

Output:

```text
1
```

---

## User clicks Reset

```text
count = 0
```

Output:

```text
0
```

---

# 33. 🔄 Complete Architecture

Now you should understand the complete flow:

```text
                    Redux Store
                        │
                        │
                 counter: { count }
                        │
                        ↑
                    Reducer
                        ↑
                     Action
                        ↑
                   dispatch()
                        ↑
                 React Component
                        │
                        ↓
                    useSelector()
                        │
                        ↓
                       UI
```

Or the easier version:

```text
        👤 USER
           ↓
      🖱️ CLICK BUTTON
           ↓
    🚀 useDispatch()
           ↓
      📨 ACTION
           ↓
      🔄 REDUCER
           ↓
       🏪 STORE
           ↓
      🔍 useSelector()
           ↓
       🖥️ UI UPDATE
```

---

# 34. 🎁 Payload

Now let's make the example more realistic.

Suppose we want:

```text
Add a specific amount
```

We can send a payload.

Reducer:

```js
addByAmount: (state, action) => {
    state.count += action.payload;
}
```

Then:

```js
dispatch(addByAmount(5));
```

The payload is:

```text
5
```

So:

```text
count = 10

addByAmount(5)

10 + 5

count = 15
```

---

# 35. 💻 Payload Example

Add:

```js
addByAmount: (state, action) => {
    state.count += action.payload;
}
```

Export:

```js
export const {
    increment,
    decrement,
    reset,
    addByAmount
} = counterSlice.actions;
```

Then:

```jsx
<button
    onClick={() => dispatch(addByAmount(5))}
>
    Add 5
</button>
```

---

# 36. 🔍 Payload Flow

```text
dispatch(addByAmount(5))
             ↓
          Action
             ↓
{
    type: "...",
    payload: 5
}
             ↓
          Reducer
             ↓
state.count += 5
```

This pattern is extremely important for real applications.

---

# 37. 🛒 Real-World Cart Example

Imagine:

```js
addToCart: (state, action) => {
    state.items.push(action.payload);
}
```

Then:

```jsx
dispatch(
    addToCart(product)
);
```

The product becomes the payload.

```text
Product Card
     ↓
dispatch(addToCart(product))
     ↓
Action
     ↓
payload = product
     ↓
Cart Reducer
     ↓
items.push(product)
     ↓
Cart updated
```

---

# 38. 🧠 Why Redux Toolkit Is Easier

Without RTK:

```text
Action type
Action creator
Reducer
Reducer combination
Store configuration
```

With RTK:

```text
createSlice()
      ↓
Reducers
      ↓
Actions automatically generated
```

And:

```text
configureStore()
```

simplifies store setup.

---

# 39. 🛠️ Redux DevTools

Redux Toolkit's standard store setup is designed to work well with Redux DevTools.

Redux DevTools helps you inspect:

```text
State
Actions
State changes
Action sequence
```

Conceptually:

```text
User clicks +
      ↓
INCREMENT action
      ↓
State changes
      ↓
DevTools shows:
      ↓
Previous State
Action
Next State
```

This is extremely useful when debugging complex applications.

---

# 40. ⚠️ Common Mistake #1 — Forgetting `<Provider>`

Wrong:

```jsx
<App />
```

when Redux-connected components expect a provider above them.

Correct:

```jsx
<Provider store={store}>
    <App />
</Provider>
```

---

# 41. ⚠️ Common Mistake #2 — Wrong selector path

Suppose store is:

```js
reducer: {
    counter: counterReducer
}
```

Then:

```js
state.counter.count
```

is correct.

Not:

```js
state.count
```

The state structure matters.

---

# 42. ⚠️ Common Mistake #3 — Forgetting `dispatch()`

Wrong:

```jsx
onClick={increment}
```

A Redux action creator is not the same as dispatching its action.

Typically:

```jsx
onClick={() => dispatch(increment())}
```

---

# 43. ⚠️ Common Mistake #4 — Calling the action too early

Be careful with:

```jsx
onClick={dispatch(increment())}
```

This executes while rendering.

Instead:

```jsx
onClick={() => dispatch(increment())}
```

The function runs when the user clicks.

---

# 44. ⚠️ Common Mistake #5 — Wrong import

Make sure:

```js
import {
    increment,
    decrement,
    reset
} from "./features/counter/counterSlice";
```

matches the exports from your slice.

---

# 45. 🧠 Important Difference

Remember:

### `useSelector()`

Reads state.

```text
Redux
  ↓
useSelector
  ↓
Component
```

### `useDispatch()`

Sends actions.

```text
Component
  ↓
useDispatch
  ↓
Redux
```

Memory trick:

```text
🔍 SELECTOR = READ

🚀 DISPATCH = CHANGE
```

---

# 46. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is Redux Toolkit?

Redux Toolkit is the recommended modern way to write Redux logic.

### Q2. Why use Redux Toolkit?

It reduces boilerplate and simplifies Redux development.

### Q3. What is `configureStore()`?

It is a Redux Toolkit function used to configure and create the Redux store.

### Q4. What is `createSlice()`?

It creates a Redux slice containing state and reducers and automatically generates action creators.

---

# 🟡 Intermediate

### Q5. What is `useSelector()`?

It reads selected data from the Redux store.

### Q6. What is `useDispatch()`?

It provides the `dispatch` function used to send Redux actions.

### Q7. What is a slice?

A slice represents one logical portion of Redux state and its update logic.

Examples:

```text
userSlice
cartSlice
productSlice
counterSlice
```

### Q8. What is a payload?

Payload is additional data sent along with an action.

---

# 🔴 Advanced

### Q9. Why can we write `state.count += 1` in Redux Toolkit?

Redux Toolkit uses Immer internally to handle immutable state updates while allowing reducer code to use a simpler mutation-like syntax.

### Q10. What is the difference between `useSelector()` and `useDispatch()`?

```text
useSelector()
    ↓
Read Redux state

useDispatch()
    ↓
Send Redux actions
```

---

# 47. 📝 Practice

### Practice 1 — Counter

Create:

```text
+
-
Reset
```

using Redux Toolkit.

---

### Practice 2 — Add Amount

Create:

```text
Add 5
Add 10
```

using payload.

---

### Practice 3 — User Slice

Create a slice:

```text
userSlice
```

State:

```js
{
    name: "",
    isLoggedIn: false
}
```

Actions:

```text
login
logout
```

---

### Practice 4 — Cart Slice

Create:

```text
cartSlice
```

State:

```js
{
    items: []
}
```

Actions:

```text
addToCart
removeFromCart
clearCart
```

---

# 48. 🧩 MINI CHALLENGE

Build a:

# 🛒 Redux Shopping Cart

Requirements:

```text
Product 1
Product 2
Product 3
```

Each product should have:

```text
id
name
price
```

Create:

```text
cartSlice
```

with:

```text
addToCart
removeFromCart
clearCart
```

Your architecture:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   └── cart/
│       └── cartSlice.js
│
├── App.jsx
└── main.jsx
```

Flow:

```text
Product
   ↓
Add to Cart
   ↓
dispatch(addToCart(product))
   ↓
cartSlice
   ↓
Redux Store
   ↓
Cart Component
   ↓
useSelector()
   ↓
Display Cart
```

---

# 49. 📝 10-Question Quiz

### Q1. What does RTK stand for?

**Answer:** Redux Toolkit.

---

### Q2. Which function creates the Redux store?

**Answer:**

```js
configureStore()
```

---

### Q3. Which function creates a slice?

**Answer:**

```js
createSlice()
```

---

### Q4. Which hook reads Redux state?

**Answer:**

```js
useSelector()
```

---

### Q5. Which hook gives us `dispatch`?

**Answer:**

```js
useDispatch()
```

---

### Q6. What is the purpose of `Provider`?

**Answer:** It makes the Redux store available to React components through React-Redux.

---

### Q7. Where do we normally define reducers with Redux Toolkit?

**Answer:** Inside `createSlice()`.

---

### Q8. What is payload?

**Answer:** Additional data passed with an action.

---

### Q9. What happens when we call:

```js
dispatch(increment())
```

**Answer:**

```text
dispatch
 ↓
increment action
 ↓
reducer
 ↓
state update
 ↓
UI update
```

---

### Q10. What is the difference between these?

```js
useSelector()
```

and

```js
useDispatch()
```

**Answer:**

```text
useSelector()
 → reads state

useDispatch()
 → sends actions
```

---

# 🧠 FINAL MEMORY TRICK

Remember these **7 Redux Toolkit concepts**:

```text
⚙️ configureStore
        ↓
🏪 Store
        ↓
✂️ createSlice
        ↓
🔄 Reducers
        ↓
📨 Actions
        ↓
🚀 useDispatch
        ↓
🔍 useSelector
```

The most important pattern:

```text
              React
                │
        ┌───────┴───────┐
        ↓               ↓
 useDispatch()     useSelector()
        ↓               ↑
      Action             │
        ↓                │
     Reducer             │
        ↓                │
       Store ────────────┘
```

### ⭐ One-line memory

> **`useDispatch()` sends actions, reducers update state, and `useSelector()` reads the updated state.**

---

# ✅ CHAPTER 33 COMPLETE

You now understand:

- Redux Toolkit
- `configureStore()`
- `createSlice()`
- reducers
- actions
- payload
- `Provider`
- `useSelector()`
- `useDispatch()`
- Redux Toolkit project structure
- Redux DevTools concept
- Complete Counter example
- Cart example
- Redux Toolkit data flow

---

# 🚀 NEXT CHAPTER

## Chapter 34 — Async Redux

We will learn:

```text
⏳ createAsyncThunk
📡 API calls
🔄 Loading state
❌ Error state
✅ Success state
📦 Redux + Axios
🔐 Authentication with Redux
```

We will build the logic step-by-step using a **real API example**.

Type **NEXT** when you're ready.