
# 🧰 ReactJS — Chapter 32
# Redux Fundamentals

---

# 🎯 What You Will Learn

In this chapter:

1. 🧠 What is Redux?
2. 🤔 Why do we need Redux?
3. 🏗️ Redux Architecture
4. 🗃️ Store
5. 📦 State
6. 📨 Action
7. 🔄 Reducer
8. 🚀 Dispatch
9. 👂 Subscribe
10. 🔁 Redux Data Flow
11. 🛒 Real-world example
12. 💻 Basic Redux example
13. ⚠️ Common mistakes
14. 💼 Interview questions
15. 📝 Practice
16. 🧩 Mini challenge
17. 📝 Quiz
18. 🧠 Final memory trick

---

# 1. 🧠 What is Redux?

**Redux is a state management library.**

It helps us manage application state in a predictable way.

Simple definition:

> Redux provides a centralized place to store and manage shared application state.

For example, imagine an e-commerce application.

We may have:

```text
User
Products
Cart
Wishlist
Orders
Authentication
Theme
```

Many components may need the same data.

Redux can keep shared state in one central store.

```text
                 Redux Store
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Navbar       Product       Cart
        │            │            │
        └────────────┼────────────┘
                     ↓
              Application
```

---

# 2. 🤔 Why Do We Need Redux?

Before Redux, we learned:

- Props
- State
- State lifting
- Context

Suppose we have:

```text
App
 ├── Navbar
 │    └── Cart
 │
 ├── Products
 │
 └── Checkout
```

Suppose the cart count is stored inside `App`.

Then:

```text
App
 ↓
Navbar
 ↓
Cart
```

We may need to pass the cart data through several components.

This is called:

# ⚠️ Prop Drilling

Example:

```text
App
 ↓ props
Layout
 ↓ props
Navbar
 ↓ props
Header
 ↓ props
Cart
```

This can become difficult in large applications.

Redux provides a centralized state store.

```text
                Redux Store
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
    Navbar        Products       Checkout
```

Components can interact with the shared state through Redux.

---

# 3. 🏠 Real-World Analogy

Imagine a company.

There is one central office that maintains important information.

```text
                 🏢 Central Office
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
      Sales           HR           Accounts
```

Instead of every department maintaining duplicate information, there is a central source.

Redux is similar.

```text
                 Redux Store
                       │
        ┌──────────────┼──────────────┐
        ↓              ↓              ↓
     Product          Cart          User
```

The **Store** is the central place.

---

# 4. 🏗️ Redux Architecture

The basic Redux architecture is:

```text
        React Component
               │
               │ dispatch()
               ↓
             Action
               │
               ↓
            Reducer
               │
               ↓
             Store
               │
               ↓
          Updated State
               │
               ↓
        React Component
```

This flow is very important.

Remember:

```text
Component
   ↓
Action
   ↓
Reducer
   ↓
Store
   ↓
New State
   ↓
Component
```

---

# 5. 🗃️ What is a Store?

The **store** contains the application's Redux state.

Think:

```text
Store = central state container
```

Example:

```js
const state = {
    counter: 0,
    user: null,
    cart: []
};
```

The store can contain different parts of application state.

For example:

```text
Store
│
├── user
├── products
├── cart
├── orders
└── settings
```

---

# 6. 📦 What is State?

State is the data currently maintained by the application.

Example:

```js
const state = {
    count: 10
};
```

Here:

```text
count = 10
```

is state.

Another example:

```js
const state = {
    user: {
        name: "Ramesh",
        city: "Hyderabad"
    }
};
```

Here the state contains user information.

---

# 7. 📨 What is an Action?

An **action** describes what happened.

An action is normally a JavaScript object.

Example:

```js
{
    type: "INCREMENT"
}
```

Another:

```js
{
    type: "ADD_TO_CART",
    payload: product
}
```

The important property is:

```text
type
```

The action tells Redux:

> "Something happened."

For example:

```text
INCREMENT
DECREMENT
LOGIN
LOGOUT
ADD_TO_CART
REMOVE_FROM_CART
```

---

# 8. 🎁 What is Payload?

Sometimes an action needs additional data.

That additional data is commonly stored in:

```text
payload
```

Example:

```js
{
    type: "ADD_TO_CART",
    payload: {
        id: 1,
        name: "Laptop",
        price: 55000
    }
}
```

Here:

```text
type
 ↓
ADD_TO_CART

payload
 ↓
Product information
```

---

# 9. 🔄 What is a Reducer?

A **reducer** is a function that determines how the state changes when an action occurs.

Simple definition:

> Reducer = function that receives the current state and an action and returns the updated state.

Conceptually:

```js
function reducer(state, action) {
    // decide new state
}
```

The reducer receives:

```text
Current State
     +
   Action
     ↓
 New State
```

---

# 10. 🧮 Reducer Example

Suppose:

```js
state = {
    count: 0
};
```

Action:

```js
{
    type: "INCREMENT"
}
```

Reducer:

```js
function reducer(state, action) {

    if (action.type === "INCREMENT") {
        return {
            count: state.count + 1
        };
    }

    return state;
}
```

If:

```text
count = 0
```

then:

```text
INCREMENT
    ↓
count becomes 1
```

---

# 11. 🚀 What is Dispatch?

`dispatch()` sends an action to Redux.

Example:

```js
dispatch({
    type: "INCREMENT"
});
```

Think:

```text
dispatch()
   ↓
"Redux, something happened!"
```

Example:

```js
dispatch({
    type: "ADD_TO_CART",
    payload: product
});
```

This tells Redux:

> Add this product to the cart.

---

# 12. 👂 What is Subscribe?

`subscribe()` allows us to listen for store changes.

Conceptually:

```js
store.subscribe(() => {
    console.log("Store changed");
});
```

Whenever the Redux store changes, the subscriber can be notified.

Think:

```text
Store
 ↓
State changes
 ↓
Subscribers are notified
```

In modern React applications, React-Redux normally handles the connection between Redux and React components, so you generally won't manually call `store.subscribe()` in everyday component code.

But understanding `subscribe()` is important for understanding Redux fundamentals.

---

# 13. 🔁 Redux Data Flow

This is one of the **most important concepts**.

Suppose the user clicks:

```text
[ + ]
```

### Step 1 — User clicks button

```text
User
 ↓
Button click
```

### Step 2 — Component dispatches action

```js
dispatch({
    type: "INCREMENT"
});
```

### Step 3 — Reducer receives action

```text
Reducer
   ↓
INCREMENT
```

### Step 4 — Reducer creates updated state

```text
count: 0
   ↓
count: 1
```

### Step 5 — Store contains new state

```text
Store
 ↓
count = 1
```

### Step 6 — React UI updates

```text
UI
 ↓
1
```

Complete flow:

```text
┌──────────────┐
│ User clicks  │
└──────┬───────┘
       ↓
┌──────────────┐
│  Component   │
└──────┬───────┘
       │ dispatch()
       ↓
┌──────────────┐
│    Action    │
└──────┬───────┘
       ↓
┌──────────────┐
│   Reducer    │
└──────┬───────┘
       ↓
┌──────────────┐
│     Store    │
└──────┬───────┘
       ↓
┌──────────────┐
│ Updated State│
└──────┬───────┘
       ↓
┌──────────────┐
│   React UI   │
└──────────────┘
```

---

# 14. 🧠 Important Redux Rule

Redux follows a predictable pattern:

```text
Action
  ↓
Reducer
  ↓
New State
```

The reducer should not directly modify the existing state in traditional Redux principles.

Conceptually:

```js
return {
    count: state.count + 1
};
```

instead of:

```js
state.count++;
return state;
```

Modern Redux Toolkit uses Immer internally, which makes reducer code look mutable while safely producing immutable updates. We will learn this properly in **Chapter 33 — Redux Toolkit**.

---

# 15. 💻 Basic Redux Concept Example

Let's understand the concept without React first.

```js
const initialState = {
    count: 0
};

function reducer(state, action) {

    switch (action.type) {

        case "INCREMENT":
            return {
                ...state,
                count: state.count + 1
            };

        case "DECREMENT":
            return {
                ...state,
                count: state.count - 1
            };

        default:
            return state;
    }
}
```

---

# 16. 🔍 Line-by-Line Explanation

### Initial state

```js
const initialState = {
    count: 0
};
```

Starting state:

```text
count = 0
```

---

### Reducer

```js
function reducer(state, action) {
```

The reducer receives:

```text
state
+
action
```

---

### Switch

```js
switch (action.type) {
```

We check what action happened.

---

### Increment

```js
case "INCREMENT":
```

If the action is:

```js
{
    type: "INCREMENT"
}
```

then:

```js
count: state.count + 1
```

---

### Decrement

```js
case "DECREMENT":
```

Then:

```js
count: state.count - 1
```

---

### Default

```js
default:
    return state;
```

If the action is unknown, keep the existing state.

---

# 17. 🧪 Dry Run

Initial:

```js
{
    count: 0
}
```

Action:

```js
{
    type: "INCREMENT"
}
```

Reducer receives:

```text
state.count = 0

action.type = "INCREMENT"
```

Reducer executes:

```js
state.count + 1
```

Result:

```text
0 + 1 = 1
```

New state:

```js
{
    count: 1
}
```

---

# 18. 🔁 Another Dry Run

Current state:

```js
{
    count: 1
}
```

Action:

```js
{
    type: "INCREMENT"
}
```

Result:

```text
1 + 1
```

New state:

```js
{
    count: 2
}
```

Again:

```text
Component
   ↓
dispatch(INCREMENT)
   ↓
Reducer
   ↓
count = 2
   ↓
UI updates
```

---

# 19. 🛒 Real-World Example — Shopping Cart

Imagine:

```text
Product Page
```

User clicks:

```text
[ Add to Cart ]
```

Component:

```js
dispatch({
    type: "ADD_TO_CART",
    payload: product
});
```

Action:

```text
ADD_TO_CART
```

Payload:

```text
Product
```

Reducer:

```text
ADD_TO_CART
      ↓
Add product to cart
```

Store:

```js
{
    cart: [
        laptop,
        phone
    ]
}
```

Navbar can read:

```text
Cart Items = 2
```

Checkout can also read:

```text
Cart Items = 2
```

This is where centralized state becomes useful.

---

# 20. 🏪 Redux Store Example

Think of the store as:

```js
{
    user: {
        name: "Ramesh"
    },

    cart: {
        items: [],
        total: 0
    },

    products: [],

    theme: "dark"
}
```

Different components can use different parts.

```text
Navbar
 ↓
user

ProductList
 ↓
products

Cart
 ↓
cart

ThemeButton
 ↓
theme
```

---

# 21. 🧩 Redux Terminology

You should remember these terms:

| Term | Meaning |
|---|---|
| Store | Central state container |
| State | Current application data |
| Action | Describes what happened |
| Payload | Extra data sent with action |
| Reducer | Calculates new state |
| Dispatch | Sends action |
| Subscribe | Listen for store changes |

---

# 22. 🧠 Easy Memory Trick

Remember:

```text
S → A → R → D
```

But for the actual flow, remember:

```text
Component
    ↓
Dispatch
    ↓
Action
    ↓
Reducer
    ↓
Store
    ↓
Updated State
    ↓
UI
```

### Easy sentence:

> **Component dispatches an Action, Reducer changes State in the Store, and UI updates.**

---

# 23. 🆚 React State vs Redux State

### React state

```jsx
const [count, setCount] = useState(0);
```

Good for:

```text
Input value
Modal open/close
Dropdown
Local UI state
Small component state
```

### Redux state

Good for shared application data such as:

```text
User
Cart
Products
Authentication
Orders
Global settings
```

Simple rule:

```text
Only one/few components need it
        ↓
React state

Many components need it
        ↓
Consider shared/global state
```

Redux is not automatically required for every piece of state.

---

# 24. ⚠️ Common Mistake #1

Don't put every tiny state variable into Redux.

Bad idea:

```text
isButtonHovered
inputTypingValue
modalOpen
dropdownOpen
```

if they are only needed by one component.

Prefer local React state when appropriate.

---

# 25. ⚠️ Common Mistake #2

Don't directly change Redux state outside the proper update mechanism.

Don't think:

```js
store.state.count = 10;
```

Instead, state changes should happen through the Redux update flow.

```text
Action
 ↓
Reducer
 ↓
New State
```

---

# 26. ⚠️ Common Mistake #3

Confusing action and reducer.

### Action

Says:

> What happened?

Example:

```js
{
    type: "INCREMENT"
}
```

### Reducer

Says:

> What should the new state be?

Example:

```js
return {
    count: state.count + 1
};
```

---

# 27. ⚠️ Common Mistake #4

Thinking Redux automatically makes an application faster.

Redux is mainly about **state management and predictable data flow**.

It does not automatically make every application faster.

---

# 28. 🏗️ Redux Architecture in a Real App

A larger application may look conceptually like:

```text
React Application
│
├── Components
│
├── Redux Store
│   │
│   ├── User State
│   ├── Product State
│   ├── Cart State
│   └── Order State
│
└── API
```

Flow:

```text
API
 ↓
Redux
 ↓
Store
 ↓
React Components
 ↓
User interaction
 ↓
Action
 ↓
Redux
```

---

# 29. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is Redux?

Redux is a state management library used to manage shared application state in a predictable way.

### Q2. Why do we use Redux?

To manage shared state across multiple components and provide predictable state updates.

### Q3. What is a Redux store?

The store is the central container that holds Redux application state.

### Q4. What is an action?

An action is an object describing what happened.

Example:

```js
{
    type: "LOGIN"
}
```

---

## 🟡 Intermediate

### Q5. What is a reducer?

A reducer is a function that receives the current state and an action and determines the next state.

### Q6. What is dispatch?

`dispatch()` sends an action to the Redux store.

### Q7. What is payload?

Payload contains additional data associated with an action.

Example:

```js
{
    type: "ADD_PRODUCT",
    payload: product
}
```

### Q8. What is Redux data flow?

```text
Component
 ↓
Action
 ↓
Reducer
 ↓
Store
 ↓
Updated State
 ↓
Component
```

---

## 🔴 Advanced

### Q9. Why should Redux state updates be predictable?

Because state changes occur through defined actions and reducers, making application behavior easier to understand and debug.

### Q10. Should every React application use Redux?

No.

Small applications may work perfectly with:

```text
useState
useReducer
Context
```

Redux becomes useful when shared application state becomes more complex.

---

# 30. 📝 Practice Questions

### Practice 1

Create a reducer for:

```text
INCREMENT
DECREMENT
RESET
```

Initial state:

```js
{
    count: 0
}
```

---

### Practice 2

Create an action for:

```text
LOGIN
```

with:

```text
user information
```

as the payload.

---

### Practice 3

Create an action:

```text
ADD_TO_CART
```

with:

```text
product
```

as the payload.

---

### Practice 4

Draw the Redux flow for:

```text
User clicks "Add to Cart"
```

Your answer should look like:

```text
User
 ↓
Component
 ↓
dispatch()
 ↓
Action
 ↓
Reducer
 ↓
Store
 ↓
Updated State
 ↓
UI
```

---

# 31. 🧩 MINI CHALLENGE

Create a **Counter Redux Logic** on paper first.

### Requirement

The counter should support:

```text
➕ Increment
➖ Decrement
🔄 Reset
```

### Step 1 — State

```js
{
    count: 0
}
```

### Step 2 — Actions

```text
INCREMENT
DECREMENT
RESET
```

### Step 3 — Reducer logic

Think:

```text
INCREMENT
    ↓
count + 1

DECREMENT
    ↓
count - 1

RESET
    ↓
count = 0
```

### Step 4 — Data flow

```text
Button
 ↓
dispatch()
 ↓
Action
 ↓
Reducer
 ↓
Store
 ↓
New count
 ↓
UI
```

Try writing the reducer yourself before looking at the next chapter.

---

# 32. 📝 10-Question Quiz

### Q1. What is Redux?

**Answer:** A state management library for managing shared application state.

---

### Q2. What is the Redux Store?

**Answer:** The central container that holds Redux state.

---

### Q3. What is an Action?

**Answer:** An object describing what happened.

---

### Q4. What property does an action normally contain?

**Answer:**

```js
type
```

---

### Q5. What is payload?

**Answer:** Additional data carried by an action.

---

### Q6. What does a reducer do?

**Answer:** It determines the next state based on the current state and action.

---

### Q7. What does dispatch do?

**Answer:** It sends an action to Redux.

---

### Q8. What does subscribe do?

**Answer:** It allows listeners to be notified when the store changes.

---

### Q9. What is the basic Redux flow?

**Answer:**

```text
Component
 ↓
Action
 ↓
Reducer
 ↓
Store
 ↓
Updated State
 ↓
UI
```

---

### Q10. Do we need Redux for every React state?

**Answer:** No. Use local React state when the state only belongs to a component or small part of the UI.

---

# 🧠 FINAL MEMORY TRICK

Remember these **7 Redux words**:

```text
🗃️ Store
   ↓
📦 State
   ↓
📨 Action
   ↓
🎁 Payload
   ↓
🔄 Reducer
   ↓
🚀 Dispatch
   ↓
👂 Subscribe
```

And remember the most important flow:

```text
        USER
          ↓
      COMPONENT
          ↓
       dispatch()
          ↓
        ACTION
          ↓
       REDUCER
          ↓
        STORE
          ↓
     NEW STATE
          ↓
       REACT UI
```

### ⭐ One-line definition

> **Redux is a predictable state management system where components dispatch actions, reducers calculate new state, and the store holds that state.**

---

# ✅ CHAPTER 32 COMPLETE

You now understand the foundation of Redux:

- Redux
- Store
- State
- Action
- Payload
- Reducer
- Dispatch
- Subscribe
- Redux architecture
- Redux data flow
- When Redux is useful
- When local React state is enough

## 🚀 NEXT CHAPTER

# Chapter 33 — Redux Toolkit

We will learn the **modern way to use Redux**:

```text
⚙️ configureStore
✂️ createSlice
🔄 reducers
📨 actions
🎁 payload
🔍 useSelector
🚀 useDispatch
🛠️ Redux DevTools
```

Then we will build a **real Counter application with Redux Toolkit**.

Type **NEXT** to continue.