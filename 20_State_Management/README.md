# 🗃️ ReactJS — Chapter 20: State Management

In the previous chapter, you learned **component communication**. Now we will learn how to manage data when an application becomes bigger.

Your course source covers these Chapter 20 topics:

- 📦 Local State
- ⬆️ Lifted State
- 🌐 Global State
- Context API
- Redux
- Zustand overview
- 🤔 Choosing the right approach

Pasted markdown

---

## 1️⃣ What Is State Management?

First remember:

> **State = data that can change while the application is running.**

Examples:

```text
count
username
login status
cart items
selected product
theme
form data
```

For example:

```jsx
const [count, setCount] = useState(0);
```

Here:

```text
count
  ↓
State
```

When the value changes:

```text
0
↓
1
↓
2
↓
3
```

React updates the UI.

---

# 2️⃣ Why Do We Need State Management?

For a small application:

```text
App
 ├── Header
 └── Home
```

Managing state is easy.

But imagine an e-commerce application:

```text
App
│
├── Header
│    └── Cart Count
│
├── ProductList
│    └── ProductCard
│
├── Cart
│
├── Checkout
│
└── Profile
```

Now several components may need the same data.

For example:

```text
🛒 Cart items
```

might be needed by:

```text
Header
Cart
Checkout
ProductList
```

Passing the data through many components can become difficult.

That's where **state management** becomes important.

---

# 3️⃣ Real-World Analogy 🏢

Imagine a company.

### Small company

One employee keeps information:

```text
Employee
 ↓
Data
```

Easy.

### Large company

Many departments need the same information:

```text
             Company
                │
      ┌─────────┼─────────┐
      ↓         ↓         ↓
    Sales      HR       Finance
```

You need a good system to manage shared information.

React applications are similar.

---

# 4️⃣ Four Levels You Should Understand 🧠

Think of state management like this:

```text
1️⃣ Local State
       ↓
2️⃣ Lifted State
       ↓
3️⃣ Context / Shared State
       ↓
4️⃣ Global State Management
```

Let's understand each one.

---

# 5️⃣ Level 1 — Local State 📦

Local state belongs to one component.

Example:

```jsx
import { useState } from "react";

function Counter() {

    const [count, setCount] = useState(0);

    return (
        <div>

            <h1>{count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

        </div>
    );
}

export default Counter;
```

Here:

```text
Counter
   ↓
count
```

Only the `Counter` component needs the state.

So this is:

# 📦 Local State

---

# 6️⃣ When Should You Use Local State?

Use local state when the data belongs mainly to one component.

Examples:

```text
Input value
Modal open/close
Dropdown open/close
Counter
Selected tab
Form field
```

For example:

```jsx
const [isOpen, setIsOpen] = useState(false);
```

This could control a dropdown or modal.

---

# 7️⃣ Local State Diagram

```text
        Counter
           │
           ↓
        count
           │
           ↓
      Counter UI
```

Simple.

No need for Redux.

No need for Context.

---

# 8️⃣ Level 2 — Lifted State ⬆️

Suppose two components need the same state:

```text
       App
      /   \
     ↓     ↓
    A       B
```

A needs the data.

B also needs the data.

Instead of keeping separate copies:

```text
A → state

B → state
```

move the state to their common parent:

```text
       App
        │
   shared state
      /   \
     ↓     ↓
    A       B
```

This is:

# ⬆️ Lifting State Up

You learned this in Chapter 19.

---

# 9️⃣ Example — Search

Imagine:

```text
App
│
├── SearchBox
│
└── ProductList
```

Both components are related to the search value.

Keep the state in `App`:

```jsx
function App() {

    const [search, setSearch] = useState("");

    return (
        <>
            <SearchBox
                search={search}
                setSearch={setSearch}
            />

            <ProductList
                search={search}
            />
        </>
    );
}
```

Flow:

```text
SearchBox
    ↓
setSearch()
    ↓
App state
    ↓
search
    ↓
ProductList
```

---

# 🔟 When Is Lifted State Useful?

When:

```text
Two or more nearby components
        ↓
need the same state
```

Example:

```text
Parent
├── SearchBox
└── ProductList
```

This is often enough.

You don't need a global state library for every situation.

---

# 1️⃣1️⃣ Level 3 — Shared / Global State 🌐

Now imagine many unrelated parts of the application need the same data.

For example:

```text
Current User
```

might be needed by:

```text
Header
Sidebar
Dashboard
Profile
Settings
Orders
```

Passing the user through every level can become difficult.

You may need a shared state solution.

---

# 1️⃣2️⃣ What Is Global State?

Global state means data that is shared across many parts of the application.

Examples:

```text
👤 Logged-in user
🛒 Shopping cart
🌙 Theme
🔐 Authentication status
🌍 Language
⚙️ Application settings
```

Conceptually:

```text
             Global State
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    Header    Dashboard    Cart
```

---

# 1️⃣3️⃣ Context API 🌐

React provides **Context** for sharing values across a component tree.

Conceptually:

```text
Context
   ↓
Provider
   ↓
Components
   ↓
useContext()
```

For example:

```text
Theme
```

can be shared with:

```text
Header
Sidebar
Dashboard
Footer
```

without manually passing `theme` through every component.

---

# 1️⃣4️⃣ Context Is Not the Same as Local State

This distinction is important.

### Local state

```jsx
const [count, setCount] = useState(0);
```

belongs to a component.

### Context

Provides a way for components deeper in the tree to access shared values.

You can even combine them:

```text
useState
   ↓
Context
   ↓
Many components
```

---

# 1️⃣5️⃣ Example — Theme

Imagine:

```text
Theme:
"dark"
```

Several components need it:

```text
Header
Sidebar
Dashboard
Footer
```

Instead of:

```text
App
 ↓ theme
Layout
 ↓ theme
Dashboard
 ↓ theme
Sidebar
```

Context can provide the shared value.

Conceptually:

```text
             Theme Context
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      Header    Sidebar    Dashboard
```

---

# 1️⃣6️⃣ Redux 🧰

When applications become more complex, another common state-management approach is:

# Redux

Redux provides a structured way to manage shared application state.

A simplified picture:

```text
Component
    ↓
 Action
    ↓
 Redux Store
    ↓
 Reducer
    ↓
 New State
    ↓
 Components
```

Don't worry if this looks complicated.

You will learn Redux separately in later chapters.

Your course has:

```text
Chapter 32 → Redux Fundamentals
Chapter 33 → Redux Toolkit
Chapter 34 → Async Redux
Chapter 35 → RTK Query
```

So **don't try to master Redux in this chapter**.

Right now, understand why it exists.

---

# 1️⃣7️⃣ What Is a Store?

In Redux, the store is where the application's shared state is managed.

Conceptually:

```text
             Redux Store
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
      user       cart       theme
```

Components can interact with the state through Redux mechanisms.

Later you will learn:

```text
store
actions
reducers
dispatch
selectors
Redux Toolkit
```

---

# 1️⃣8️⃣ Zustand Overview

Another state-management library you may encounter is:

# Zustand

Zustand provides a simple way to create shared state.

Conceptually:

```text
Components
    ↓
Zustand Store
    ↓
Shared State
```

Example idea:

```text
cart
user
theme
```

can be stored in a shared store.

For now:

> **Know what Zustand is. Don't spend time learning its syntax yet.**

Your course source specifically lists it as an overview topic here. Pasted markdown

---

# 1️⃣9️⃣ Local vs Lifted vs Global

This is one of the most important tables.

| State type | Used when |
|---|---|
| 📦 Local State | One component needs it |
| ⬆️ Lifted State | Nearby components share it |
| 🌐 Context | Many components need shared values |
| 🧰 Redux/Zustand | Larger or more complex shared state |

---

# 2️⃣0️⃣ Example: Login Status 🔐

Suppose your application has:

```text
isLoggedIn
```

Several components need it:

```text
Header
Dashboard
Profile
Sidebar
```

You could have:

```text
                isLoggedIn
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
    Header       Dashboard       Profile
```

This is a good example of shared application state.

Depending on application complexity, Context or a dedicated state-management solution may be considered.

---

# 2️⃣1️⃣ Example: Shopping Cart 🛒

Cart state can be needed by:

```text
ProductCard
Header
CartPage
Checkout
OrderSummary
```

Architecture:

```text
                  Cart State
                      │
       ┌──────────────┼──────────────┐
       ↓              ↓              ↓
     Header       Cart Page       Checkout
```

For a larger application, centralized state can make this easier to manage.

---

# 2️⃣2️⃣ State Management Decision Tree 🌳

When you need some data, ask:

### Question 1

Does only one component need it?

```text
YES
 ↓
Local State
```

### Question 2

Do nearby components need it?

```text
YES
 ↓
Lift State Up
```

### Question 3

Do many/deep components need it?

```text
YES
 ↓
Consider Context
```

### Question 4

Is the application state large or complex?

```text
YES
 ↓
Consider Redux/Zustand
```

---

# 2️⃣3️⃣ Very Important Beginner Rule ⭐

Don't think:

> "I am learning Redux, so I should use Redux everywhere."

❌ Wrong.

Instead:

```text
Simple problem
     ↓
Simple solution
```

Use the smallest tool that solves the problem clearly.

---

# 2️⃣4️⃣ Example Architecture

### Small application

```text
App
│
├── Header
└── Home
```

Use:

```text
useState
```

when appropriate.

---

### Medium application

```text
App
│
├── Header
├── Search
└── ProductList
```

Use:

```text
useState
+
Props
+
Lifted State
```

when appropriate.

---

### Larger application

```text
App
│
├── Header
├── Dashboard
├── Products
├── Cart
├── Orders
└── Profile
```

You may consider:

```text
Context
Redux
Zustand
```

depending on requirements.

---

# 2️⃣5️⃣ Real-World Example — E-Commerce 🛒

Imagine Amazon-like functionality.

You have:

```text
Product
Cart
Checkout
User
Orders
```

### Product

```text
ProductCard
   ↓
Add to Cart
```

### Cart

```text
Cart
   ↓
items
quantity
total
```

### Header

```text
Cart Count
```

### Checkout

```text
Cart Items
Total
User
```

Many components depend on related information.

A good state architecture becomes important.

---

# 2️⃣6️⃣ Data Flow Example

```text
                App
                 │
            Shared State
                 │
       ┌─────────┼─────────┐
       ↓         ↓         ↓
    Product     Cart     Checkout
       │         │
       └────┬────┘
            ↓
        User Action
            ↓
       State Update
            ↓
         UI Update
```

---

# 2️⃣7️⃣ State Is the Source of Truth 🧠

Suppose:

```text
cartItems = 3
```

You don't want:

```text
Header → 3
Cart → 3
Checkout → 3
```

to become three completely separate pieces of state.

Instead, ideally there should be a clear source of truth:

```text
             Cart State
                 │
        ┌────────┼────────┐
        ↓        ↓        ↓
      Header    Cart   Checkout
```

Then the UI can stay synchronized.

---

# 2️⃣8️⃣ Common Mistake ❌

### Using global state for everything

Bad idea:

```text
name
email
modal
inputValue
buttonLoading
dropdown
```

all inside one huge global store without a reason.

This can make applications harder to understand.

Prefer local state when data is local.

---

# 2️⃣9️⃣ Common Mistake ❌

### Creating duplicate state

Suppose:

```text
Parent
  ↓
products
```

and the child creates another copy unnecessarily:

```text
Child
  ↓
productsCopy
```

Now you can have:

```text
Parent products
       ≠
Child productsCopy
```

Keeping one clear source of truth is usually easier.

---

# 3️⃣0️⃣ Common Mistake ❌

### Using Context for every value

Context is useful.

But:

```text
Every state
   ↓
Context
```

is not automatically good architecture.

Use it when shared access actually helps.

---

# 3️⃣1️⃣ Common Mistake ❌

### Learning Redux before understanding state

If you don't understand:

```text
useState
props
lifting state
callbacks
```

Redux will feel confusing.

That's why this course teaches state management concepts first.

---

# 3️⃣2️⃣ Best Practices ⭐

### ✅ Start simple

```text
useState
```

first.

### ✅ Lift state when necessary

```text
Common Parent
```

can own shared state.

### ✅ Use Context for suitable shared values

For example:

```text
theme
user
language
```

### ✅ Use dedicated state libraries when complexity requires them

For example:

```text
Redux
Zustand
```

### ✅ Keep one clear source of truth

Avoid unnecessary duplicate state.

---

# 3️⃣3️⃣ Interview Question 🎤

## Q1. What is state management?

State management is the way an application stores, updates, and shares changing data between components.

---

## Q2. What is local state?

State that mainly belongs to one component.

Example:

```jsx
const [count, setCount] = useState(0);
```

---

## Q3. What is lifted state?

State moved to a common parent so multiple child components can share it.

---

## Q4. What is global state?

State that is shared across multiple areas of an application.

---

## Q5. What is Context?

A React mechanism for making values available to components deeper in the component tree without manually passing props through every level.

---

## Q6. What is Redux?

A state-management library commonly used for predictable management of shared application state.

---

## Q7. What is Zustand?

A state-management library that provides a relatively simple approach to shared state.

---

## Q8. Should Redux be used for every application?

No.

Choose the simplest state-management approach that fits the application's requirements.

---

## Q9. What is the source of truth?

The main place where a particular piece of state is maintained.

---

## Q10. What should you try first for simple component state?

Usually local state with `useState()`.

---

# 🧪 3️⃣4️⃣ Practice

## Practice 1 — Local State

Create:

```text
Counter
```

with:

```text
Count: 0

[Increase]
[Decrease]
```

Use local state.

---

## Practice 2 — Lifted State

Create:

```text
App
├── SearchBox
└── SearchResult
```

Keep search state in `App`.

---

## Practice 3 — Shared Theme

Imagine:

```text
App
├── Header
├── Sidebar
└── Dashboard
```

All three need:

```text
theme
```

Decide whether Context would be useful.

---

## Practice 4 — Shopping Cart

Design:

```text
App
├── ProductList
├── Header
├── Cart
└── Checkout
```

Ask yourself:

> Which data should be shared?

Possible answers:

```text
cartItems
cartCount
cartTotal
```

---

# 🏆 3️⃣5️⃣ Mini Challenge

## 🛒 Build a State Architecture

Imagine this application:

```text
                 App
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    Header     Products     Cart
                              │
                              ↓
                          Checkout
```

You have these data:

```text
1. searchText
2. cartItems
3. currentUser
4. modalOpen
5. theme
```

### Your task

Decide where each should live.

Try first:

| Data | Your choice |
|---|---|
| `searchText` | ? |
| `cartItems` | ? |
| `currentUser` | ? |
| `modalOpen` | ? |
| `theme` | ? |

Think about:

```text
Local?
Lifted?
Context?
Global state?
```

### 🧠 Hint

Don't automatically choose Redux.

First ask:

> **Who actually needs this data?**

That question will help you become better at React architecture.

---

# 📝 3️⃣6️⃣ 10-Question Quiz

## Q1. What is local state?

### ✅ Answer

State mainly used by one component.

---

## Q2. What is lifted state?

### ✅ Answer

State moved to a common parent so multiple components can share it.

---

## Q3. What is global state?

### ✅ Answer

Shared application data used by multiple parts of the application.

---

## Q4. What can Context help with?

### ✅ Answer

Sharing values with components deeper in the component tree without manually passing props through every level.

---

## Q5. Is Redux required in every React project?

### ❌ Answer

No.

---

## Q6. What should you usually use for simple component state?

### ✅ Answer

`useState()`.

---

## Q7. Why can duplicate state be a problem?

### ✅ Answer

Different copies can become out of sync and create confusing behavior.

---

## Q8. Name two state-management libraries/approaches discussed in this chapter.

### ✅ Answer

Redux and Zustand.

---

## Q9. What question should you ask before choosing a state-management solution?

### ✅ Answer

> Who needs this data?

Also consider how complex and widely shared the state is.

---

## Q10. What is the most important beginner rule?

### ✅ Answer

Start with the simplest solution and increase complexity only when needed.

---

# 📚 3️⃣7️⃣ Quick Revision

| Concept | Remember |
|---|---|
| 📦 Local State | One component |
| ⬆️ Lifted State | Common parent |
| 🌐 Shared State | Many components |
| Context | Share values through tree |
| Redux | Structured shared state management |
| Zustand | Simple shared-state library |
| 🎯 Source of Truth | Clear owner of data |
| ❌ Duplicate State | Can cause inconsistency |
| ⭐ Simple First | Don't over-engineer |

---

# 🧠 3️⃣8️⃣ Final Memory Trick

Remember this ladder:

```text
        🟢 SIMPLE
           │
           ↓
      useState()
           │
           ↓
     ⬆️ Lift State
           │
           ↓
      🌐 Context
           │
           ↓
    🧰 Redux / Zustand
           │
           ↓
       🔴 COMPLEX
```

### ⭐ The main question

Whenever you create state, ask:

```text
❓ Who needs this data?
```

Then decide:

```text
One component?
    ↓
Local State

Nearby components?
    ↓
Lift State

Many/deep components?
    ↓
Context

Complex application-wide state?
    ↓
Redux / Zustand may be considered
```

---

# ✅ Chapter 20 Checklist

Before moving forward, make sure you understand:

- [ ] What is state management?
- [ ] Local state
- [ ] Lifted state
- [ ] Shared state
- [ ] Global state
- [ ] Context
- [ ] Redux
- [ ] Zustand
- [ ] Source of truth
- [ ] Duplicate state
- [ ] When to use local state
- [ ] When to lift state
- [ ] When Context may help
- [ ] When Redux/Zustand may be considered
- [ ] Why simple solutions are better for simple problems

---

# 🚀 NEXT CHAPTER

## 🧭 CHAPTER 21 — React Router

Next we will learn how to create multiple pages/views in a React application:

```text
🏠 Home
      ↓
👤 About
      ↓
📦 Products
      ↓
📄 Product Details
      ↓
🔐 Login
      ↓
📊 Dashboard
```

We will learn:

```text
React Router
Routes
Route
Link
NavLink
BrowserRouter
Navigation
Dynamic Routes
```

Type **`NEXT`** when you are ready for **Chapter 21 — React Router**.
:::