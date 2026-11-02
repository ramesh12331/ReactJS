# ⚛️ ReactJS — Chapter 18
# `React.memo`

## 🎯 Chapter Goal

In this chapter, you will learn:

- 🔄 What is component re-rendering?
- ⚛️ What is `React.memo`?
- 📦 How props comparison works
- ⚡ How `React.memo` can improve performance
- 🔗 `React.memo` + `useCallback()`
- 🧠 When to use `React.memo`
- ❌ When NOT to use it
- ⚠️ Common mistakes
- 🛠️ Practical examples
- 🎤 Interview questions
- 🧪 Practice
- 🏆 Mini challenge

These are the exact Chapter 18 topics in your React course source.

---

# 1️⃣ First: What Is a Re-render? 🔄

Before learning `React.memo`, you must understand **re-rendering**.

A React component can render again when its state or props change.

For example:

```jsx
import { useState } from "react";

function App() {

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

export default App;
```

When you click:

```text
Increase
```

the state changes:

```text
0
↓
1
```

React renders the component again.

---

# 2️⃣ Why Is Re-rendering Important?

Re-rendering is **normal** in React.

It is not automatically a problem.

Think:

```text
State changes
     ↓
React updates component
     ↓
UI shows new data
```

That is how React works.

The problem can happen when:

```text
Parent changes
     ↓
Many children render again
     ↓
Some children didn't actually need to update
```

For a small application, this may not matter.

For a large application, unnecessary rendering can sometimes affect performance.

---

# 3️⃣ Real-World Analogy 🏢

Imagine an office with:

```text
Manager
│
├── Employee A
├── Employee B
├── Employee C
└── Employee D
```

Suppose only Employee A's information changes.

You don't want every employee to redo all their work.

You want:

```text
Employee A
    ↓
Update
```

while unchanged employees can continue.

`React.memo` provides a way to memoize a component based on its props.

---

# 4️⃣ What Is `React.memo()`?

`React.memo()` is a React API that lets React **memoize a component**.

Simple idea:

```text
Component
    ↓
React.memo()
    ↓
Check props
    ↓
Props unchanged?
    ↓
Potentially skip re-render
```

The important word is:

# 📦 PROPS

`React.memo()` mainly helps when a component receives the same props between renders.

---

# 5️⃣ Basic Syntax

```jsx
const MyComponent = React.memo(function MyComponent(props) {

    return <h1>{props.name}</h1>;

});
```

Or:

```jsx
const MyComponent = memo(function MyComponent(props) {

    return <h1>{props.name}</h1>;

});
```

With the modern import:

```jsx
import { memo } from "react";
```

Then:

```jsx
const MyComponent = memo(function MyComponent() {

    return <h1>Hello</h1>;

});
```

---

# 6️⃣ Very Simple Example

```jsx
import { memo } from "react";

const Child = memo(function Child() {

    console.log("Child rendered");

    return (
        <h2>Child Component</h2>
    );
});

function App() {

    return (
        <div>
            <Child />
        </div>
    );
}

export default App;
```

Here:

```jsx
memo(...)
```

wraps the `Child` component.

---

# 7️⃣ Why Do We Use `React.memo()`?

Suppose:

```text
Parent
   ↓
Child
```

The parent renders again.

Normally, React may render the child again as part of rendering the parent subtree.

But if the child is memoized:

```text
Parent renders
      ↓
React.memo Child
      ↓
Are props unchanged?
      ↓
YES
      ↓
Child may skip re-render
```

This can reduce unnecessary work.

---

# 8️⃣ Important: `React.memo()` Is a Performance Optimization ⚡

Don't think:

> "React.memo is required for every component."

No.

It is an optimization tool.

Normal React:

```text
Correctness first
       ↓
Performance optimization second
```

First make the application work correctly.

Then optimize when there is a real reason.

---

# 9️⃣ First Parent → Child Example

```jsx
function Child({ name }) {

    console.log("Child rendered");

    return <h2>Hello {name}</h2>;
}
```

Parent:

```jsx
function App() {

    return (
        <Child name="Ramesh" />
    );
}
```

Flow:

```text
App
 ↓
name="Ramesh"
 ↓
Child
 ↓
Hello Ramesh
```

Now let's add memoization.

---

# 🔟 Using `memo`

```jsx
import { memo } from "react";

const Child = memo(function Child({ name }) {

    console.log("Child rendered");

    return <h2>Hello {name}</h2>;
});
```

Now React can compare the child's props between renders.

---

# 1️⃣1️⃣ Props Comparison 📦

Suppose the child receives:

```jsx
<Child name="Ramesh" />
```

On another parent render, it receives:

```jsx
<Child name="Ramesh" />
```

The prop value is unchanged.

Conceptually:

```text
Previous:
name = "Ramesh"

Current:
name = "Ramesh"

        ↓

Same
        ↓

Memoized child may skip rendering
```

---

# 1️⃣2️⃣ What If the Prop Changes?

First:

```jsx
<Child name="Ramesh" />
```

Later:

```jsx
<Child name="Suresh" />
```

Now:

```text
Previous:
Ramesh

Current:
Suresh

      ↓

Different
      ↓
Child needs to render
```

So:

```text
Props changed
    ↓
Child renders
```

---

# 1️⃣3️⃣ Important Mental Model 🧠

Remember:

```text
React.memo
     ↓
Compare props
     ↓
Same?
 ┌───┴───┐
YES     NO
 ↓       ↓
Skip    Render
```

This is the basic idea.

---

# 1️⃣4️⃣ `React.memo()` Does NOT Mean "Never Render"

Very important.

This is wrong:

> "`React.memo` means the component will render only once."

❌ No.

A memoized component can still render when its props change.

It can also render due to its own state changes or other relevant React behavior.

Think:

```text
React.memo
    ↓
Can skip some unnecessary renders
```

Not:

```text
React.memo
    ↓
Never render again
```

---

# 1️⃣5️⃣ Example With State

```jsx
import { memo, useState } from "react";

const Child = memo(function Child({ name }) {

    console.log("Child rendered");

    return (
        <h2>Hello {name}</h2>
    );
});

function App() {

    const [count, setCount] = useState(0);

    return (
        <div>

            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child name="Ramesh" />

        </div>
    );
}

export default App;
```

Here:

```text
count changes
   ↓
App renders
```

But:

```text
Child prop:
name = "Ramesh"
```

has not changed.

So `React.memo()` can allow the child to skip an unnecessary render.

---

# 1️⃣6️⃣ Dry Run 🧠

Initial render:

```text
count = 0
name = Ramesh
```

UI:

```text
Count: 0
Hello Ramesh
```

Click:

```text
Increase
```

State becomes:

```text
count = 1
```

Parent renders again.

Child receives:

```text
name = Ramesh
```

Previous:

```text
Ramesh
```

Current:

```text
Ramesh
```

Props are unchanged.

Therefore:

```text
React.memo
    ↓
Child may skip rendering
```

---

# 1️⃣7️⃣ Why Does `useCallback()` Matter Here? 🔗

This connects directly to Chapter 17.

Suppose the parent passes a function.

```jsx
function App() {

    const handleClick = () => {
        console.log("Clicked");
    };

    return (
        <Child onClick={handleClick} />
    );
}
```

Now imagine:

```text
Parent renders
      ↓
new function reference
      ↓
Child receives new function prop
```

Even though the function code looks the same, the function reference can be different.

Therefore:

```text
React.memo
      +
new function reference
      ↓
Prop appears changed
      ↓
Child may render again
```

---

# 1️⃣8️⃣ Solution: `useCallback()` + `memo`

This is one of the most important React performance patterns.

```jsx
import { memo, useCallback, useState } from "react";

const Child = memo(function Child({ onClick }) {

    console.log("Child rendered");

    return (
        <button onClick={onClick}>
            Child Button
        </button>
    );
});

function App() {

    const [count, setCount] = useState(0);

    const handleClick = useCallback(() => {
        console.log("Child clicked");
    }, []);

    return (
        <div>

            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

            <Child onClick={handleClick} />

        </div>
    );
}

export default App;
```

Flow:

```text
Parent
   ↓
useCallback()
   ↓
stable function reference
   ↓
Child
   ↓
memo()
   ↓
props comparison
```

This is why your course placed:

```text
Chapter 16 → useMemo
Chapter 17 → useCallback
Chapter 18 → React.memo
```

The concepts are related.

---

# 1️⃣9️⃣ `React.memo` + Normal Function ❌

Consider:

```jsx
const Child = memo(function Child({ onClick }) {
    return <button onClick={onClick}>Click</button>;
});

function App() {

    const handleClick = () => {
        console.log("Hello");
    };

    return <Child onClick={handleClick} />;
}
```

The problem can be:

```text
App render
   ↓
new handleClick reference
   ↓
Child receives changed function prop
   ↓
memo sees changed prop
```

In a performance-sensitive situation, this is where `useCallback()` may help.

---

# 2️⃣0️⃣ Props With Objects ⚠️

Another important concept.

Consider:

```jsx
const user = {
    name: "Ramesh",
    age: 25
};
```

Then:

```jsx
<Child user={user} />
```

Objects are reference-based.

A new object can have the same content but a different reference.

For example:

```jsx
const user = {
    name: "Ramesh",
    age: 25
};
```

and another newly created object:

```jsx
const user = {
    name: "Ramesh",
    age: 25
};
```

They contain the same data, but they are different object references.

So memoization can be affected by object identity.

---

# 2️⃣1️⃣ Simple Example

```jsx
const Child = memo(function Child({ user }) {

    return (
        <h2>{user.name}</h2>
    );
});
```

Parent:

```jsx
function App() {

    const user = {
        name: "Ramesh"
    };

    return <Child user={user} />;
}
```

If the `user` object is recreated on every parent render, its reference can change.

Therefore:

```text
Previous user object
        ≠
New user object
```

even if:

```text
name = Ramesh
```

in both.

---

# 2️⃣2️⃣ Why This Matters

`React.memo()` normally compares props using a shallow comparison.

That means React checks prop values/references rather than deeply comparing every nested property.

For primitive values:

```text
string
number
boolean
```

comparison is usually straightforward.

For:

```text
object
array
function
```

reference identity becomes important.

---

# 2️⃣3️⃣ Example: Primitive Props

```jsx
<Child
    name="Ramesh"
    age={25}
    active={true}
/>
```

These are:

```text
string
number
boolean
```

Simple values.

---

# 2️⃣4️⃣ Example: Reference Props

```jsx
<Child
    user={user}
    items={items}
    onClick={handleClick}
/>
```

These can involve references:

```text
user     → object
items    → array
onClick  → function
```

Therefore stable references can matter for memoized components.

---

# 2️⃣5️⃣ `React.memo()` and Arrays

Suppose:

```jsx
const products = [
    "Laptop",
    "Mouse",
    "Keyboard"
];

<Child products={products} />
```

If a new array is created on every render:

```text
Old array
   ≠
New array
```

then the prop can appear changed.

For expensive derived arrays, `useMemo()` can sometimes help create a stable reference.

```jsx
const products = useMemo(() => {
    return [
        "Laptop",
        "Mouse",
        "Keyboard"
    ];
}, []);
```

Now:

```text
useMemo
   ↓
stable array reference
   ↓
React.memo
   ↓
props comparison
```

Use this only when there is a real reason for the optimization.

---

# 2️⃣6️⃣ Three Hooks Together 🧠

Now you can understand:

```text
useMemo
   ↓
Memoize VALUE

useCallback
   ↓
Memoize FUNCTION

React.memo
   ↓
Memoize COMPONENT rendering based on props
```

### ⭐ Easy table

| Tool | Main purpose |
|---|---|
| `useMemo()` | Memoize calculated value |
| `useCallback()` | Memoize function reference |
| `React.memo()` | Skip unnecessary component render when props are unchanged |

---

# 2️⃣7️⃣ When Should You Use `React.memo()`? ✅

Consider it when:

### 1. A component renders frequently

```text
Parent
 ↓
many renders
 ↓
Child
```

### 2. The child is expensive to render

For example:

```text
Large table
Complex chart
Large list
Complex UI
```

### 3. Props often remain unchanged

If props rarely change, memoization may be useful.

### 4. You have identified a real performance problem

Optimization should be based on actual need.

---

# 2️⃣8️⃣ When NOT to Use `React.memo()` ❌

Don't wrap every component with:

```jsx
memo(...)
```

without a reason.

For example:

```jsx
const SmallText = memo(function SmallText() {
    return <p>Hello</p>;
});
```

If the component is tiny and performance isn't an issue, this may add unnecessary complexity.

---

# 2️⃣9️⃣ Common Mistake — Thinking Memo Means No Render

Wrong:

```text
React.memo
    ↓
Component never renders again
```

Correct:

```text
React.memo
    ↓
Compare props
    ↓
If props are unchanged
    ↓
May skip rendering
```

---

# 3️⃣0️⃣ Common Mistake — Ignoring Function Props

Suppose:

```jsx
<Child onClick={() => console.log("Hello")} />
```

The arrow function can be a new function reference when the parent renders.

If `Child` is memoized:

```text
new function reference
       ↓
prop changed
       ↓
memo may not skip child render
```

If reference stability is useful:

```jsx
const handleClick = useCallback(() => {
    console.log("Hello");
}, []);
```

---

# 3️⃣1️⃣ Common Mistake — Using `useCallback()` Everywhere

Don't do:

```jsx
const handleA = useCallback(...);
const handleB = useCallback(...);
const handleC = useCallback(...);
const handleD = useCallback(...);
```

just because you learned it.

First ask:

```text
❓ Is there a real performance reason?
```

If no:

```text
Keep the code simple.
```

---

# 3️⃣2️⃣ Common Mistake — Deep Object Comparison

A beginner may think:

```text
React.memo
   ↓
checks every nested property
```

That is not the basic behavior.

React memoization uses shallow prop comparison by default.

For reference values:

```text
object
array
function
```

reference identity matters.

---

# 3️⃣3️⃣ Custom Comparison Function

`React.memo()` can also accept a comparison function.

Basic idea:

```jsx
const Child = memo(
    function Child({ user }) {
        return <h2>{user.name}</h2>;
    },
    arePropsEqual
);
```

The second argument lets you provide custom prop comparison logic.

But as a beginner:

> Don't use custom comparison unless you understand why you need it.

Default behavior is enough for most basic examples.

---

# 3️⃣4️⃣ Practical Example — Product Card 🛒

Imagine an e-commerce application:

```text
ProductList
│
├── ProductCard
├── ProductCard
├── ProductCard
└── ProductCard
```

Each card displays:

```text
Laptop
₹50,000

[Add to Cart]
```

Suppose another part of the page changes.

If a product card's props haven't changed, memoization may help prevent unnecessary work.

Example:

```jsx
import { memo } from "react";

const ProductCard = memo(function ProductCard({
    name,
    price
}) {

    console.log("ProductCard rendered");

    return (
        <div>
            <h2>{name}</h2>
            <p>₹{price}</p>
        </div>
    );
});

export default ProductCard;
```

Parent:

```jsx
<ProductCard
    name="Laptop"
    price={50000}
/>
```

---

# 3️⃣5️⃣ Practical Example — Employee Dashboard 👨‍💼

Imagine:

```text
EmployeeDashboard
        ↓
EmployeeTable
        ↓
EmployeeRow
```

There are:

```text
1,000 employees
```

If the dashboard changes something unrelated, you don't necessarily want every row to do expensive rendering work.

Memoization can be considered for components where props remain stable and rendering is expensive.

This is a real-world performance optimization scenario.

---

# 3️⃣6️⃣ Component Tree Example

```text
App
│
├── Header
├── Sidebar
│
├── Dashboard
│    │
│    ├── SalesCard
│    ├── RevenueCard
│    └── EmployeeCard
│
└── Footer
```

Suppose only:

```text
SalesCard
```

needs to update.

Good component design plus appropriate memoization can help keep unrelated rendering work lower.

---

# 3️⃣7️⃣ Performance Optimization Flow ⚡

```text
Application
     ↓
Observe rendering
     ↓
Find unnecessary work
     ↓
Identify expensive component
     ↓
Check props
     ↓
Consider React.memo
     ↓
Check function props
     ↓
Consider useCallback
     ↓
Check calculated values
     ↓
Consider useMemo
```

### 🧠 This is better than:

```text
Use memo everywhere ❌
```

---

# 3️⃣8️⃣ `React.memo` vs `useMemo`

Very common interview question.

### `React.memo`

Works with:

```text
COMPONENT
```

Example:

```jsx
const Child = memo(function Child() {
    return <h1>Hello</h1>;
});
```

### `useMemo`

Works with:

```text
VALUE
```

Example:

```jsx
const total = useMemo(() => {
    return price * quantity;
}, [price, quantity]);
```

Memory:

```text
React.memo
→ COMPONENT

useMemo
→ VALUE

useCallback
→ FUNCTION
```

---

# 3️⃣9️⃣ `React.memo` vs `useCallback`

### `React.memo`

```text
Memoize component rendering based on props
```

### `useCallback`

```text
Memoize callback function reference
```

Together:

```text
React.memo
     +
useCallback
     ↓
Useful for memoized child + callback prop
```

---

# 🎤 4️⃣0️⃣ Interview Questions

## Q1. What is `React.memo()`?

`React.memo()` is a performance optimization that memoizes a component and can skip re-rendering when its props have not changed.

---

## Q2. What does `React.memo()` compare?

By default, it performs a shallow comparison of props.

---

## Q3. Does `React.memo()` prevent all renders?

No.

The component can still render when its props change or due to its own relevant updates.

---

## Q4. Why is `React.memo()` useful?

It can reduce unnecessary rendering work for components whose props remain unchanged.

---

## Q5. What is shallow comparison?

It compares values at the top level rather than deeply checking every nested object property.

---

## Q6. Why can function props cause a memoized component to render?

Because a function created during rendering can have a new reference.

---

## Q7. How can `useCallback()` help?

It can keep a callback function reference stable when its dependencies don't change.

---

## Q8. What is the difference between `React.memo` and `useMemo`?

```text
React.memo
→ component

useMemo
→ calculated value
```

---

## Q9. Should every component use `React.memo()`?

No.

It should be used when there is a meaningful performance reason.

---

## Q10. What is the common combination for optimized child components receiving callbacks?

```text
React.memo
+
useCallback
```

---

# 🧪 4️⃣1️⃣ Practice Questions

## Practice 1 — Basic Memo

Create:

```text
Parent
 ↓
Child
```

Wrap `Child` with:

```jsx
memo()
```

Add:

```jsx
console.log("Child rendered");
```

Observe when it renders.

---

## Practice 2 — Props Change

Pass:

```jsx
<Child name="Ramesh" />
```

Then change the name.

Observe:

```text
Props changed
     ↓
Child renders
```

---

## Practice 3 — Counter + Child

Create:

```text
Count
[Increase]

Child
Hello Ramesh
```

Use:

```text
useState
+
React.memo
```

Observe what happens when count changes.

---

## Practice 4 — Callback

Pass a function from parent:

```text
Parent
   ↓
onClick
   ↓
Child
```

Then use:

```text
useCallback
+
React.memo
```

Compare the behavior.

---

# 🏆 4️⃣2️⃣ Mini Challenge

Build:

# 🛒 Product Dashboard

Create:

```text
App
│
├── Counter
│
└── ProductCard
```

Product:

```text
Laptop
₹50,000
```

Requirements:

### Step 1

Create counter:

```text
Count: 0

[Increase]
```

### Step 2

Create:

```text
ProductCard
```

### Step 3

Wrap ProductCard:

```jsx
memo(ProductCard)
```

### Step 4

Add:

```jsx
console.log("ProductCard rendered");
```

### Step 5

Increase the counter.

Observe:

```text
Parent state changes
        ↓
App renders
        ↓
Product props unchanged
        ↓
React.memo
        ↓
Child may skip render
```

### Step 6

Now pass:

```text
onAddToCart
```

from the parent.

### Step 7

Use:

```text
useCallback()
```

for that function.

Now you have learned the complete relationship:

```text
useState
   ↓
Parent render
   ↓
useCallback
   ↓
stable callback
   ↓
React.memo
   ↓
props comparison
```

---

# 📝 4️⃣3️⃣ 10-Question Quiz

## Q1. What is `React.memo()`?

### ✅ Answer

A performance optimization that can skip unnecessary rendering of a component when its props have not changed.

---

## Q2. What does `React.memo()` mainly work with?

### ✅ Answer

Component props.

---

## Q3. What kind of comparison does `React.memo()` use by default?

### ✅ Answer

A shallow comparison of props.

---

## Q4. Does `React.memo()` mean the component renders only once?

### ❌ Answer

No.

It can still render when relevant props or state change.

---

## Q5. What happens when a memoized component receives a changed prop?

### ✅ Answer

The component needs to render with the new prop.

---

## Q6. Why can an inline function affect memoization?

### ✅ Answer

Because a new function reference can be created during each parent render.

---

## Q7. Which Hook can help stabilize a callback function?

### ✅ Answer

```jsx
useCallback()
```

---

## Q8. What does `useMemo()` memoize?

### ✅ Answer

A calculated value.

---

## Q9. What does `useCallback()` memoize?

### ✅ Answer

A function reference.

---

## Q10. Should you use `React.memo()` on every component?

### ❌ Answer

No.

Use it when it provides a meaningful performance benefit.

---

# 📚 4️⃣4️⃣ Quick Revision

| Concept | Meaning |
|---|---|
| 🔄 Re-render | Component renders again |
| ⚛️ `React.memo` | Memoizes a component |
| 📦 Props | Main values checked by memoization |
| 🔍 Shallow comparison | Top-level prop comparison |
| 🔗 Function reference | Important for callback props |
| 🧠 `useMemo` | Memoizes value |
| 🎯 `useCallback` | Memoizes function |
| ⚡ Performance | Main reason for memoization |
| 🟢 Same props | Child may skip rendering |
| 🔴 Changed props | Child needs to render |

---

# 🧠 4️⃣5️⃣ Final Memory Trick

Remember these three together:

```text
┌─────────────────────────┐
│      React.memo         │
│       COMPONENT         │
└────────────┬────────────┘
             │
             ↓
        checks props


┌─────────────────────────┐
│       useMemo           │
│         VALUE           │
└────────────┬────────────┘
             │
             ↓
      remembers result


┌─────────────────────────┐
│      useCallback        │
│       FUNCTION          │
└────────────┬────────────┘
             │
             ↓
    remembers reference
```

### ⭐ Easy formula

```text
React.memo
    ↓
Component

useMemo
    ↓
Value

useCallback
    ↓
Function
```

And the most important performance pattern:

```text
Parent
  ↓
Child
  ↓
React.memo
  +
useCallback for callback props
  ↓
Potentially fewer unnecessary renders
```

---

# ✅ Chapter 18 Checklist

Before moving forward, make sure you can explain:

- [ ] What is a re-render?
- [ ] Why re-rendering happens
- [ ] What is `React.memo()`?
- [ ] Why `React.memo()` is used
- [ ] Props comparison
- [ ] Shallow comparison
- [ ] Primitive vs reference props
- [ ] Function props
- [ ] Object props
- [ ] Array props
- [ ] `React.memo()` + `useCallback()`
- [ ] `React.memo()` vs `useMemo()`
- [ ] When to use `React.memo()`
- [ ] When NOT to use it
- [ ] Common memoization mistakes

---

# 🚀 NEXT CHAPTER

## 🔗 CHAPTER 19 — Component Communication

Now we move from **performance optimization** into an important real-world React skill.

We will learn:

```text
👨‍👦 Parent → Child
        ↓
📦 Props

👶 Child → Parent
        ↓
📞 Callback Functions

👫 Sibling Communication
        ↓
⬆️ State Lifting

🌐 Context
        ↓
🔄 Practical Examples
```

Your course source defines Chapter 19 around **Parent → Child, Child → Parent, sibling communication, props, callback functions, Context, state lifting, and practical examples**.

Type **`NEXT`** when you are ready for **Chapter 19 — Component Communication**.