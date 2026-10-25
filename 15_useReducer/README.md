# 🔄 ReactJS — Chapter 15
# `useReducer`

## 🎯 Chapter Goal

In this chapter, you will learn:

- What is `useReducer()`?
- Why do we use `useReducer()`?
- Reducer function
- Initial state
- Actions
- `dispatch()`
- `action.type`
- `action.payload`
- `useReducer()` syntax
- `useState` vs `useReducer`
- Counter example
- Multiple actions
- Form example
- Common mistakes
- Best practices
- Interview questions
- Practice
- Mini challenge

---

# 1️⃣ What Is `useReducer()`?

`useReducer()` is a React Hook used to manage state.

It is especially useful when:

- state is more complex
- many actions can change the state
- state has multiple related values
- you want state-update logic in one place

Basic idea:

```text
User Action
    ↓
dispatch()
    ↓
Reducer
    ↓
New State
    ↓
React UI
```

---

# 2️⃣ Why Do We Need `useReducer()`?

You already learned:

```jsx
useState()
```

For simple state, `useState` is usually easy.

Example:

```jsx
const [count, setCount] = useState(0);
```

But imagine a shopping cart.

The cart might need:

```text
➕ Add product
➖ Remove product
🔢 Change quantity
🗑️ Clear cart
💰 Calculate total
```

There can be many different state changes.

Instead of putting all the logic inside many event handlers, `useReducer()` lets us organize the state-changing logic in one reducer function.

---

# 3️⃣ Real-World Analogy 🏦

Think about a bank.

You don't directly change the bank balance yourself.

You request an action:

```text
Deposit
Withdraw
Transfer
```

The bank processes the request and calculates the new balance.

React's reducer pattern is similar:

```text
User
 ↓
Action
 ↓
dispatch()
 ↓
Reducer
 ↓
New State
```

---

# 4️⃣ Important Words ⭐⭐⭐

There are five important terms:

```text
1️⃣ State
2️⃣ Reducer
3️⃣ Action
4️⃣ dispatch()
5️⃣ Initial State
```

Remember:

```text
State
 ↓
Action
 ↓
dispatch()
 ↓
Reducer
 ↓
New State
```

---

# 5️⃣ What Is State?

State is the data that belongs to a component and can change over time.

Example:

```jsx
const initialState = {
    count: 0
};
```

Here:

```text
count = 0
```

is our state.

---

# 6️⃣ What Is a Reducer?

A reducer is a function that decides:

> **When an action happens, what should the new state be?**

Basic structure:

```jsx
function reducer(state, action) {

    // decide new state

}
```

A reducer normally receives:

```text
state
+
action
```

and returns:

```text
new state
```

Flow:

```text
Old State
    +
 Action
    ↓
Reducer
    ↓
New State
```

---

# 7️⃣ What Is an Action?

An action describes **what happened**.

For example:

```jsx
{
    type: "increment"
}
```

Another action:

```jsx
{
    type: "decrement"
}
```

Think of `type` as the instruction name.

```text
"increment" → increase
"decrement" → decrease
"reset"     → reset
```

---

# 8️⃣ What Is `dispatch()`?

`dispatch()` sends an action to the reducer.

Example:

```jsx
dispatch({
    type: "increment"
});
```

Think:

```text
dispatch()
   ↓
"Hey reducer,
something happened!"
```

Then the reducer decides what to do.

---

# 9️⃣ Basic Syntax

The basic syntax is:

```jsx
const [state, dispatch] = useReducer(
    reducer,
    initialState
);
```

There are three important parts:

```text
state
dispatch
reducer
```

Example:

```jsx
const [state, dispatch] = useReducer(
    reducer,
    initialState
);
```

---

# 🔟 First Simple Example

Let's build a counter.

```jsx
import { useReducer } from "react";

function reducer(state, action) {

    if (action.type === "increment") {
        return {
            count: state.count + 1
        };
    }

    return state;
}

function App() {

    const initialState = {
        count: 0
    };

    const [state, dispatch] = useReducer(
        reducer,
        initialState
    );

    return (
        <div>

            <h1>{state.count}</h1>

            <button
                onClick={() =>
                    dispatch({ type: "increment" })
                }
            >
                Increase
            </button>

        </div>
    );
}

export default App;
```

---

# 1️⃣1️⃣ Understand the Code Step by Step

First:

```jsx
import { useReducer } from "react";
```

We import the Hook.

---

Then:

```jsx
const initialState = {
    count: 0
};
```

Our starting state is:

```text
count = 0
```

---

Then:

```jsx
const [state, dispatch] = useReducer(
    reducer,
    initialState
);
```

React gives us:

```text
state
dispatch
```

---

Then:

```jsx
dispatch({
    type: "increment"
});
```

We send the action.

---

The reducer receives:

```jsx
function reducer(state, action) {
```

For example:

```text
state
 ↓
{ count: 0 }

action
 ↓
{ type: "increment" }
```

---

Then:

```jsx
return {
    count: state.count + 1
};
```

New state becomes:

```text
count = 1
```

---

# 1️⃣2️⃣ Dry Run 🧠

Initial state:

```text
count = 0
```

User clicks:

```text
Increase
```

This executes:

```jsx
dispatch({
    type: "increment"
});
```

Reducer receives:

```text
state = { count: 0 }

action = {
    type: "increment"
}
```

Reducer checks:

```jsx
action.type === "increment"
```

True.

Then:

```text
0 + 1
```

New state:

```text
count = 1
```

React updates the UI.

Next click:

```text
1 → 2
```

Next:

```text
2 → 3
```

---

# 1️⃣3️⃣ Better Reducer With `switch`

Reducers commonly use `switch`.

```jsx
function reducer(state, action) {

    switch (action.type) {

        case "increment":
            return {
                count: state.count + 1
            };

        default:
            return state;
    }
}
```

This is easier when we have many actions.

---

# 1️⃣4️⃣ Multiple Actions

Let's add:

```text
➕ Increment
➖ Decrement
🔄 Reset
```

Reducer:

```jsx
function reducer(state, action) {

    switch (action.type) {

        case "increment":
            return {
                count: state.count + 1
            };

        case "decrement":
            return {
                count: state.count - 1
            };

        case "reset":
            return {
                count: 0
            };

        default:
            return state;
    }
}
```

Now we have three actions.

---

# 1️⃣5️⃣ Complete Counter

```jsx
import { useReducer } from "react";

const initialState = {
    count: 0
};

function reducer(state, action) {

    switch (action.type) {

        case "increment":
            return {
                count: state.count + 1
            };

        case "decrement":
            return {
                count: state.count - 1
            };

        case "reset":
            return {
                count: 0
            };

        default:
            return state;
    }
}

function App() {

    const [state, dispatch] = useReducer(
        reducer,
        initialState
    );

    return (
        <div>

            <h1>Count: {state.count}</h1>

            <button
                onClick={() =>
                    dispatch({ type: "increment" })
                }
            >
                +
            </button>

            <button
                onClick={() =>
                    dispatch({ type: "decrement" })
                }
            >
                -
            </button>

            <button
                onClick={() =>
                    dispatch({ type: "reset" })
                }
            >
                Reset
            </button>

        </div>
    );
}

export default App;
```

---

# 1️⃣6️⃣ Visual Flow

```text
                User clicks +
                     ↓
              dispatch()
                     ↓
        { type: "increment" }
                     ↓
                 reducer
                     ↓
             count + 1
                     ↓
                new state
                     ↓
                 React
                     ↓
                   UI
```

---

# 1️⃣7️⃣ Why Is This Better Than Many `setState()` Calls?

With `useState`, you might write:

```jsx
setCount(count + 1);
```

and:

```jsx
setCount(count - 1);
```

and:

```jsx
setCount(0);
```

That's fine for a simple counter.

With `useReducer`, the possible state transitions are clearly grouped:

```text
increment
decrement
reset
```

inside:

```jsx
reducer()
```

This becomes more useful when the state logic grows.

---

# 1️⃣8️⃣ `action.type`

The `type` tells the reducer what happened.

Examples:

```jsx
{
    type: "increment"
}
```

```jsx
{
    type: "delete"
}
```

```jsx
{
    type: "login"
}
```

```jsx
{
    type: "logout"
}
```

```jsx
{
    type: "reset"
}
```

Think:

```text
type = WHAT HAPPENED?
```

---

# 1️⃣9️⃣ `action.payload`

Sometimes an action needs extra data.

For example:

```text
Add 5
```

We need to tell the reducer:

```text
5
```

We can use `payload`.

```jsx
dispatch({
    type: "incrementBy",
    payload: 5
});
```

Reducer:

```jsx
function reducer(state, action) {

    switch (action.type) {

        case "incrementBy":
            return {
                count: state.count + action.payload
            };

        default:
            return state;
    }
}
```

---

# 2️⃣0️⃣ Dry Run With Payload

Initial:

```text
count = 10
```

Dispatch:

```jsx
dispatch({
    type: "incrementBy",
    payload: 5
});
```

Reducer receives:

```text
state.count = 10
action.type = "incrementBy"
action.payload = 5
```

Then:

```text
10 + 5
```

New state:

```text
15
```

---

# 2️⃣1️⃣ Another Payload Example

Imagine adding a product.

```jsx
dispatch({
    type: "addProduct",
    payload: {
        id: 1,
        name: "Laptop",
        price: 50000
    }
});
```

Here:

```text
type
 ↓
"addProduct"

payload
 ↓
product object
```

This pattern is extremely common.

---

# 2️⃣2️⃣ Action Structure

A common pattern is:

```jsx
{
    type: "actionName",
    payload: data
}
```

Example:

```jsx
{
    type: "addUser",
    payload: {
        name: "Ramesh",
        city: "Hyderabad"
    }
}
```

Remember:

```text
type
 ↓
What happened?

payload
 ↓
What data is needed?
```

---

# 2️⃣3️⃣ `useState` vs `useReducer` ⭐⭐⭐

This is an important interview question.

## `useState`

Best for simpler state.

```jsx
const [name, setName] = useState("");
```

---

## `useReducer`

Useful when state logic becomes more complex.

```jsx
const [state, dispatch] = useReducer(
    reducer,
    initialState
);
```

---

# 2️⃣4️⃣ Simple Comparison

| `useState` | `useReducer` |
|---|---|
| Simple state | Complex state logic |
| Direct setter | Dispatch action |
| Easy for small components | Useful for larger state logic |
| `setState()` | `dispatch()` |
| Update logic may be in handlers | Update logic centralized in reducer |
| Good for simple forms/counters | Good for complex forms/cart/state machines |

---

# 2️⃣5️⃣ Example — When `useState` Is Enough

```jsx
const [name, setName] = useState("");
```

If all you need is:

```text
Enter name
 ↓
Update name
```

`useState` is probably enough.

Don't use `useReducer` just because you know it.

---

# 2️⃣6️⃣ Example — When `useReducer` Helps

Imagine a shopping cart:

```text
Cart
│
├── Add product
├── Remove product
├── Increase quantity
├── Decrease quantity
└── Clear cart
```

Now you have many related actions.

A reducer can organize them:

```jsx
function cartReducer(state, action) {

    switch (action.type) {

        case "add":
            // add product

        case "remove":
            // remove product

        case "increase":
            // increase quantity

        case "decrease":
            // decrease quantity

        case "clear":
            // clear cart

        default:
            return state;
    }
}
```

This is much easier to maintain.

---

# 2️⃣7️⃣ Important Rule — Don't Mutate State ❌

Suppose state is:

```jsx
const initialState = {
    count: 0
};
```

Don't directly modify:

```jsx
state.count++;
```

Instead return a new state:

```jsx
return {
    count: state.count + 1
};
```

React state should be treated as immutable.

---

# 2️⃣8️⃣ Why Return a New State?

Think:

```text
Old State
   ↓
{ count: 0 }

Action
   ↓
increment

New State
   ↓
{ count: 1 }
```

The reducer calculates the new state.

```jsx
return {
    count: state.count + 1
};
```

---

# 2️⃣9️⃣ Reducer Should Be Predictable 🧠

A reducer should generally:

```text
Input
 ↓
State + Action
 ↓
Output
 ↓
New State
```

Avoid putting unrelated side effects inside the reducer.

For example, don't normally do API calls directly inside the reducer.

Keep the reducer focused on calculating state.

---

# 3️⃣0️⃣ Practical Example — Login State 🔐

Imagine:

```text
Login
Logout
```

Initial state:

```jsx
const initialState = {
    isLoggedIn: false,
    user: null
};
```

Reducer:

```jsx
function reducer(state, action) {

    switch (action.type) {

        case "login":
            return {
                isLoggedIn: true,
                user: action.payload
            };

        case "logout":
            return {
                isLoggedIn: false,
                user: null
            };

        default:
            return state;
    }
}
```

Login action:

```jsx
dispatch({
    type: "login",
    payload: {
        name: "Ramesh",
        role: "Developer"
    }
});
```

Logout:

```jsx
dispatch({
    type: "logout"
});
```

---

# 3️⃣1️⃣ Login Flow

```text
User clicks Login
       ↓
dispatch()
       ↓
type: "login"
       ↓
payload: user
       ↓
reducer
       ↓
isLoggedIn = true
       ↓
user = Ramesh
       ↓
UI updates
```

---

# 3️⃣2️⃣ Practical Example — Shopping Cart

Initial state:

```jsx
const initialState = {
    items: []
};
```

Add product:

```jsx
dispatch({
    type: "add",
    payload: product
});
```

Remove product:

```jsx
dispatch({
    type: "remove",
    payload: productId
});
```

Clear:

```jsx
dispatch({
    type: "clear"
});
```

The reducer handles the logic.

---

# 3️⃣3️⃣ `useReducer` + Context ⭐⭐⭐

Later, you can combine:

```text
useReducer
+
Context
```

This is a very useful React pattern.

For example:

```text
              App
               │
        Context Provider
               │
          useReducer
               │
       ┌───────┼───────┐
       ↓       ↓       ↓
    Header   Cart    Product
```

Context can share:

```text
state
dispatch
```

with multiple components.

You already learned Context in Chapter 14.

Later, this combination becomes useful for application-wide state.

---

# 3️⃣4️⃣ Common Mistake — Calling Reducer Yourself

Don't normally do:

```jsx
reducer(state, action);
```

from your button.

Instead:

```jsx
dispatch({
    type: "increment"
});
```

React uses the reducer to calculate the next state.

---

# 3️⃣5️⃣ Common Mistake — Wrong Action Type

Suppose reducer has:

```jsx
case "increment":
```

but you dispatch:

```jsx
dispatch({
    type: "increase"
});
```

These are different strings.

Result:

```text
"increase"
≠
"increment"
```

The reducer will reach:

```jsx
default:
    return state;
```

No expected change happens.

---

# 3️⃣6️⃣ Common Mistake — Forgetting `default`

Always handle unexpected actions.

```jsx
default:
    return state;
```

This gives the reducer a safe fallback.

---

# 3️⃣7️⃣ Common Mistake — Forgetting `return`

Wrong:

```jsx
case "increment":
    state.count + 1;
```

Correct:

```jsx
case "increment":
    return {
        count: state.count + 1
    };
```

The reducer must return the next state.

---

# 3️⃣8️⃣ Common Mistake — Mutating Arrays

Suppose:

```jsx
state.items.push(product);
```

Avoid directly mutating state.

Instead create a new array:

```jsx
return {
    ...state,
    items: [...state.items, product]
};
```

You will use the spread operator heavily when working with reducer state.

---

# ⭐ 3️⃣9️⃣ Best Practices

### ✅ Use meaningful action types

Good:

```text
"addProduct"
"removeProduct"
"clearCart"
```

Avoid unclear names:

```text
"doIt"
"change"
"abc"
```

---

### ✅ Keep reducer logic together

Put related state-changing logic inside the reducer.

---

### ✅ Don't mutate state

Return new objects/arrays.

---

### ✅ Use `payload` for additional data

```jsx
{
    type: "addProduct",
    payload: product
}
```

---

### ✅ Use `useState` when state is simple

Don't use `useReducer` just because it looks advanced.

---

# 🎤 4️⃣0️⃣ Interview Questions

## Q1. What is `useReducer()`?

`useReducer()` is a React Hook used to manage state with reducer logic and dispatched actions.

---

## Q2. What are the two values returned by `useReducer()`?

```jsx
const [state, dispatch] = useReducer(...);
```

They are:

```text
state
dispatch
```

---

## Q3. What is a reducer?

A function that receives the current state and an action and returns the next state.

---

## Q4. What is an action?

An object describing what happened.

Example:

```jsx
{
    type: "increment"
}
```

---

## Q5. What does `dispatch()` do?

It sends an action to the reducer.

---

## Q6. What is `action.type`?

It identifies the action that occurred.

---

## Q7. What is `action.payload`?

It contains additional data needed by the reducer.

---

## Q8. When should you use `useReducer()` instead of `useState()`?

When state logic becomes complex or there are many related state transitions/actions.

---

## Q9. Should a reducer mutate state?

No.

A reducer should return a new state instead of directly mutating the existing state.

---

## Q10. Can `useReducer()` work with Context?

Yes.

`Context + useReducer` is a common pattern for sharing complex state and dispatch functions across components.

---

# 🧪 4️⃣1️⃣ Practice Tasks

## Task 1 — Counter

Create:

```text
Count: 0

[ + ]
[ - ]
[ Reset ]
```

Use:

```text
useReducer
```

Actions:

```text
increment
decrement
reset
```

---

## Task 2 — Increment By Value

Create a button:

```text
[ Add 5 ]
```

Dispatch:

```jsx
{
    type: "incrementBy",
    payload: 5
}
```

---

## Task 3 — User Login

Create state:

```jsx
{
    isLoggedIn: false,
    user: null
}
```

Actions:

```text
login
logout
```

---

## Task 4 — Cart

Create:

```text
Add
Remove
Clear
```

Use a reducer to manage cart items.

---

# 🏆 4️⃣2️⃣ Mini Challenge — Shopping Cart Reducer

Create:

```text
Product
Laptop
₹50,000

[ Add to Cart ]
```

State:

```jsx
{
    items: []
}
```

Actions:

```text
add
remove
clear
```

Your reducer should handle:

```text
ADD PRODUCT
     ↓
items increases

REMOVE PRODUCT
     ↓
item removed

CLEAR CART
     ↓
items = []
```

Flow:

```text
             dispatch()
                 ↓
              action
                 ↓
              reducer
                 ↓
            new state
                 ↓
               Cart
```

### ⭐ Bonus

Add:

```text
quantity
```

and create:

```text
increase
decrease
```

actions.

---

# 📝 4️⃣3️⃣ 10-Question Quiz

## Q1. Which Hook is used for reducer-based state management?

### ✅ Answer

```jsx
useReducer()
```

---

## Q2. What does `dispatch()` do?

### ✅ Answer

It sends an action to the reducer.

---

## Q3. What does the reducer return?

### ✅ Answer

The new state.

---

## Q4. What property usually identifies an action?

### ✅ Answer

```jsx
action.type
```

---

## Q5. What is `payload` used for?

### ✅ Answer

To carry additional data required by an action.

---

## Q6. Which is usually simpler for a basic counter?

### ✅ Answer

`useState()` is usually simpler.

---

## Q7. Should we mutate reducer state directly?

### ❌ Answer

No.

Return a new state instead.

---

## Q8. What happens if an unknown action is dispatched?

### ✅ Answer

The reducer should normally handle it with a `default` case, commonly returning the current state.

---

## Q9. Can `useReducer` manage object state?

### ✅ Answer

Yes.

It is especially useful when multiple related values change through different actions.

---

## Q10. Can `useReducer` and Context be combined?

### ✅ Answer

Yes. This is a common pattern for sharing complex state and `dispatch` across components.

---

# 📚 4️⃣4️⃣ Quick Revision

| Concept | Meaning |
|---|---|
| 🔄 `useReducer` | Hook for reducer-based state management |
| 📦 State | Current application/component data |
| ⚙️ Reducer | Calculates next state |
| 📢 Action | Describes what happened |
| 🚀 `dispatch()` | Sends an action |
| 🏷️ `type` | Identifies the action |
| 📦 `payload` | Extra action data |
| 🔢 Initial state | Starting state |
| 🛡️ `default` | Fallback action handling |
| 🚫 Mutation | Should be avoided |
| 🟢 `useState` | Simple state |
| 🔵 `useReducer` | More complex state logic |

---

# 🧠 4️⃣5️⃣ Final Memory Trick

Remember this sentence:

> **Dispatch an Action → Reducer calculates → State changes → UI updates.**

Short version:

```text
📢 dispatch
   ↓
🏷️ action
   ↓
⚙️ reducer
   ↓
📦 new state
   ↓
🖥️ UI
```

### ⭐ Five words

```text
STATE
ACTION
DISPATCH
REDUCER
NEW STATE
```

If you understand these five words, you understand the basic idea of `useReducer()`.

---

# ✅ Chapter 15 Checklist

Before moving ahead, make sure you can explain:

- [ ] What is `useReducer()`?
- [ ] Why do we use it?
- [ ] What is a reducer?
- [ ] What is an action?
- [ ] What is `dispatch()`?
- [ ] What is `action.type`?
- [ ] What is `action.payload`?
- [ ] How to use `switch`
- [ ] Why `default` is important
- [ ] Why state should not be mutated
- [ ] `useState` vs `useReducer`
- [ ] How to build a counter
- [ ] How to build login state
- [ ] How to manage cart state
- [ ] How Context and `useReducer` can work together

---

# 🚀 NEXT CHAPTER

## ⚡ CHAPTER 16 — `useMemo`

Next we will learn:

```text
⚡ useMemo
      ↓
Performance
      ↓
Expensive Calculations
      ↓
Memoization
      ↓
Dependency Array
      ↓
useMemo vs useEffect
      ↓
Common Mistakes
```

Type **`NEXT`** when you are ready for **Chapter 16 — `useMemo`**.