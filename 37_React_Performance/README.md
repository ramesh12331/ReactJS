# 🚀 CHAPTER 37 — REACT PERFORMANCE

## 🎯 What You Will Learn

In this chapter:

- ⚡ What is React Performance?
- 🔄 What is a re-render?
- 🤔 Why do unnecessary re-renders happen?
- 🐌 Common performance problems
- 🧠 Memoization
- `React.memo()`
- `useMemo()`
- `useCallback()`
- 🔍 How to optimize components
- 📦 Lazy loading
- ⚠️ Common performance mistakes
- 💼 Interview questions
- 🧩 Mini challenge
- 📝 Quiz

---

# 1. 🧠 What Is React Performance?

**React performance** means making your React application respond quickly and avoid unnecessary work.

A good application should:

```text
⚡ Load quickly
⚡ Respond quickly
⚡ Avoid unnecessary rendering
⚡ Handle large data efficiently
⚡ Avoid unnecessary API requests
```

Think about an e-commerce application.

```text
Products
   ↓
1000 products
   ↓
User changes one filter
```

We don't want every unrelated component to perform expensive work unnecessarily.

---

# 2. 🔄 What Is a Re-render?

A **re-render** means React runs a component again to determine what the UI should look like.

Example:

```jsx
function Counter() {

    const [count, setCount] =
        useState(0);

    return (
        <button
            onClick={() =>
                setCount(count + 1)
            }
        >
            {count}
        </button>
    );
}
```

When:

```js
setCount(count + 1);
```

changes state:

```text
State changes
    ↓
Component renders again
    ↓
React calculates the new UI
    ↓
DOM is updated where necessary
```

---

# 3. 🧠 Important: Re-render ≠ Entire DOM Rebuilt

A common beginner misunderstanding is:

> "Whenever state changes, React completely rebuilds the webpage."

That's not the right mental model.

Conceptually:

```text
State Change
     ↓
Component function runs again
     ↓
React compares the resulting UI
     ↓
Only necessary DOM changes are committed
```

React's reconciliation process helps determine what needs to change.

---

# 4. 🐌 What Is an Unnecessary Re-render?

Suppose:

```text
App
├── Header
├── ProductList
└── Footer
```

If some state changes in `App`, React may render child components again as part of the normal rendering process.

But perhaps:

```text
Footer
```

doesn't depend on that state.

If `Footer` is expensive, repeatedly rendering it may become unnecessary work.

This is where optimization techniques can help.

---

# 5. 🧠 First Rule of Performance

Don't optimize everything immediately.

Start with:

```text
Correct code
     ↓
Measure
     ↓
Find bottleneck
     ↓
Optimize
     ↓
Measure again
```

Memory:

> **Measure first, optimize second.**

---

# 6. 🧠 What Is Memoization?

**Memoization** means remembering a previously calculated result so that the calculation doesn't need to be repeated when the relevant inputs haven't changed.

Simple analogy:

You calculate:

```text
100 × 100 = 10,000
```

If somebody asks the exact same calculation again, you can remember:

```text
100 × 100
     ↓
10,000
```

instead of doing the work again.

In React, memoization is commonly associated with:

```text
React.memo()
useMemo()
useCallback()
```

---

# 7. 🧩 Three Important Tools

Remember:

```text
React.memo()
    ↓
Memoize a COMPONENT

useMemo()
    ↓
Memoize a CALCULATED VALUE

useCallback()
    ↓
Memoize a FUNCTION
```

This distinction is extremely important.

---

# 8. 🧩 `React.memo()`

`React.memo()` can prevent a component from re-rendering when its props have not changed.

Example:

```jsx
import React from "react";

const User = React.memo(function User({
    name
}) {

    console.log("User rendered");

    return (
        <h2>
            {name}
        </h2>
    );
});

export default User;
```

Now `User` is memoized.

---

# 9. 🧠 Why Use `React.memo()`?

Suppose:

```text
Parent
├── Counter
└── User
```

The counter changes:

```text
Counter: 0
 ↓
Counter: 1
```

If `User` receives the same props, `React.memo()` can allow React to skip rendering `User` when its props are unchanged.

Conceptually:

```text
Counter changes
     ↓
Parent renders
     ↓
React.memo(User)
     ↓
Are User props unchanged?
     ↓
YES
     ↓
Skip User render
```

---

# 10. 🧪 Example

```jsx
import { useState } from "react";
import React from "react";


const User = React.memo(function User({
    name
}) {

    console.log("User rendered");

    return (
        <h2>
            User: {name}
        </h2>
    );
});


function App() {

    const [count, setCount] =
        useState(0);


    return (
        <div>

            <button
                onClick={() =>
                    setCount(count + 1)
                }
            >
                Count: {count}
            </button>

            <User name="Ramesh" />

        </div>
    );
}


export default App;
```

When the button is clicked:

```text
count changes
```

but:

```text
name = "Ramesh"
```

doesn't change.

So `React.memo()` can help avoid an unnecessary `User` render.

---

# 11. ⚠️ Important: `React.memo()` Uses Prop Comparison

Consider:

```jsx
<User name="Ramesh" />
```

The string is the same.

But objects are different.

Example:

```jsx
<User
    user={{
        name: "Ramesh"
    }}
/>
```

On every parent render, a new object can be created.

Conceptually:

```text
Render 1
user → Object A

Render 2
user → Object B
```

Even though the contents look identical:

```text
Object A !== Object B
```

Therefore, memoization may not help as you expect.

---

# 12. 🧠 Referential Equality

This is important for React performance.

Primitive:

```js
const name1 = "Ramesh";
const name2 = "Ramesh";
```

Objects:

```js
const user1 = {
    name: "Ramesh"
};

const user2 = {
    name: "Ramesh"
};
```

These are separate object references.

```text
user1 !== user2
```

Even though:

```text
user1.name === user2.name
```

This matters for:

```text
React.memo
useMemo
useCallback
```

---

# 13. 🧠 What Is `useMemo()`?

`useMemo()` memoizes a **calculated value**.

Syntax:

```jsx
const result = useMemo(
    () => expensiveCalculation(data),
    [data]
);
```

Think:

```text
useMemo()
   ↓
Calculate value
   ↓
Remember value
   ↓
Recalculate only when dependencies change
```

---

# 14. 🧪 Simple `useMemo()` Example

Suppose we have:

```jsx
import {
    useMemo,
    useState
} from "react";


function App() {

    const [count, setCount] =
        useState(0);

    const [number, setNumber] =
        useState(10);


    const doubledNumber = useMemo(() => {

        console.log("Calculating...");

        return number * 2;

    }, [number]);


    return (
        <div>

            <h2>
                Count: {count}
            </h2>

            <h2>
                Number: {number}
            </h2>

            <h2>
                Double: {doubledNumber}
            </h2>

            <button
                onClick={() =>
                    setCount(count + 1)
                }
            >
                Increase Count
            </button>

            <button
                onClick={() =>
                    setNumber(number + 1)
                }
            >
                Increase Number
            </button>

        </div>
    );
}
```

---

# 15. 🔍 Dry Run of `useMemo()`

Initially:

```text
number = 10
```

Calculation:

```text
10 × 2
 ↓
20
```

Stored:

```text
doubledNumber = 20
```

Now:

```text
count changes
```

But:

```text
number = 10
```

hasn't changed.

Therefore React can reuse the memoized value:

```text
20
```

When:

```text
number = 11
```

changes:

```text
useMemo()
   ↓
dependency changed
   ↓
calculate again
   ↓
22
```

---

# 16. 🧠 When Should You Use `useMemo()`?

Use it when you have an **expensive calculation** that doesn't need to run again when unrelated values change.

Examples:

```text
Large filtering
Large sorting
Complex calculations
Large data transformations
```

Don't automatically use it for every calculation.

---

# 17. ❌ Bad Use of `useMemo()`

Don't do this everywhere:

```jsx
const total = useMemo(() => {

    return price + tax;

}, [price, tax]);
```

For a tiny calculation like:

```text
price + tax
```

memoization may provide little or no benefit and adds complexity.

Often this is simpler:

```jsx
const total = price + tax;
```

Remember:

> Memoization has a cost too.

---

# 18. 🧠 What Is `useCallback()`?

`useCallback()` memoizes a **function reference**.

Syntax:

```jsx
const handleClick = useCallback(() => {

    // logic

}, [dependencies]);
```

Think:

```text
useMemo()
   ↓
remember VALUE

useCallback()
   ↓
remember FUNCTION
```

---

# 19. 🧪 `useCallback()` Example

```jsx
import {
    useCallback,
    useState
} from "react";


function App() {

    const [count, setCount] =
        useState(0);


    const handleClick = useCallback(() => {

        console.log("Button clicked");

    }, []);


    return (
        <div>

            <button
                onClick={handleClick}
            >
                Click
            </button>

            <button
                onClick={() =>
                    setCount(count + 1)
                }
            >
                Count: {count}
            </button>

        </div>
    );
}
```

Here:

```text
handleClick
```

has a stable function reference between renders as long as its dependencies don't change.

---

# 20. 🧠 Why Does Function Reference Matter?

Consider:

```jsx
<Child onClick={handleClick} />
```

If `handleClick` is recreated on every render:

```text
Render 1
handleClick → Function A

Render 2
handleClick → Function B

Render 3
handleClick → Function C
```

Even if the function does the same thing:

```text
Function A !== Function B
```

If `Child` is memoized:

```jsx
const Child = React.memo(...);
```

the changing function reference can cause the child to render again.

`useCallback()` can help when function identity matters.

---

# 21. 🔗 `React.memo()` + `useCallback()`

These are often used together.

```jsx
import {
    useCallback,
    useState
} from "react";

import React from "react";


const Child = React.memo(function Child({
    onClick
}) {

    console.log("Child rendered");

    return (
        <button onClick={onClick}>
            Child Button
        </button>
    );
});


function App() {

    const [count, setCount] =
        useState(0);


    const handleClick = useCallback(() => {

        console.log("Clicked");

    }, []);


    return (
        <div>

            <button
                onClick={() =>
                    setCount(count + 1)
                }
            >
                Count: {count}
            </button>

            <Child
                onClick={handleClick}
            />

        </div>
    );
}
```

Conceptually:

```text
Parent renders
     ↓
handleClick reference remains stable
     ↓
Child receives same function reference
     ↓
React.memo()
     ↓
Child can skip unnecessary render
```

---

# 22. 🧠 The Three Tools Again

This is one of the most important things to remember.

```text
┌──────────────────────────────────┐
│ React.memo()                     │
│ ↓                                │
│ Memoizes component rendering     │
└──────────────────────────────────┘

┌──────────────────────────────────┐
│ useMemo()                        │
│ ↓                                │
│ Memoizes calculated value        │
└──────────────────────────────────┘

┌──────────────────────────────────┐
│ useCallback()                    │
│ ↓                                │
│ Memoizes function reference      │
└──────────────────────────────────┘
```

Memory:

> **Component → memo, Value → useMemo, Function → useCallback**

---

# 23. 📊 Performance Example

Imagine:

```text
App
│
├── SearchBox
├── ProductList
│     ├── ProductCard
│     ├── ProductCard
│     ├── ProductCard
│     └── ...
│
└── Footer
```

There are 1,000 products.

If every small state change causes expensive calculations for all 1,000 products:

```text
State change
   ↓
Large calculation
   ↓
1000 products
   ↓
Slow UI
```

We might optimize by:

```text
React.memo()
useMemo()
useCallback()
```

But only after identifying the actual bottleneck.

---

# 24. 🔍 Large List Optimization

Suppose:

```js
const filteredProducts = products.filter(
    product =>
        product.name
            .toLowerCase()
            .includes(search.toLowerCase())
);
```

For a tiny list, that's fine.

For a very large list, repeated filtering can become expensive.

We can potentially use:

```jsx
const filteredProducts = useMemo(() => {

    return products.filter((product) =>
        product.name
            .toLowerCase()
            .includes(search.toLowerCase())
    );

}, [products, search]);
```

Now the filtering calculation is repeated when:

```text
products changes
OR
search changes
```

not merely because some unrelated state changed.

---

# 25. ⚠️ Don't Overuse Memoization

A common beginner mistake is:

```text
"I learned useMemo, so I'll use useMemo everywhere."
```

Don't.

Use:

```text
useMemo
```

when the calculation is expensive or referential stability is important.

Use:

```text
useCallback
```

when function identity matters, often when passing callbacks to memoized children or when required by another dependency.

Use:

```text
React.memo
```

when preventing unnecessary child renders provides a real benefit.

---

# 26. 📦 Lazy Loading

Another performance technique is **lazy loading**.

Instead of loading every component immediately:

```text
App
 ├── Home
 ├── Dashboard
 ├── Admin
 ├── Reports
 └── Settings
```

we can load some components only when needed.

Example:

```jsx
import {
    lazy,
    Suspense
} from "react";
```

Then:

```jsx
const Dashboard = lazy(
    () => import("./Dashboard")
);
```

---

# 27. ⏳ `Suspense`

Lazy-loaded components need a loading fallback.

```jsx
<Suspense
    fallback={<h2>⏳ Loading...</h2>}
>
    <Dashboard />
</Suspense>
```

Flow:

```text
User needs Dashboard
       ↓
React loads Dashboard chunk
       ↓
⏳ Suspense fallback
       ↓
Dashboard downloaded
       ↓
Dashboard displayed
```

---

# 28. 🧠 Why Lazy Loading?

Suppose your application contains:

```text
Home
Dashboard
Admin
Reports
Analytics
Settings
```

A user visiting only Home may not need all of these immediately.

Lazy loading can split code so some code is loaded when needed.

Conceptually:

```text
Without lazy loading:

One large JavaScript bundle
        ↓
Load everything


With lazy loading:

Home bundle
Dashboard bundle
Admin bundle
Reports bundle
        ↓
Load when needed
```

---

# 29. 🧩 Practical Lazy Loading

```jsx
import {
    lazy,
    Suspense
} from "react";


const Dashboard = lazy(
    () => import("./Dashboard")
);


function App() {

    return (

        <Suspense
            fallback={
                <h2>
                    ⏳ Loading Dashboard...
                </h2>
            }
        >

            <Dashboard />

        </Suspense>

    );
}

export default App;
```

---

# 30. 🧠 Performance Optimization Checklist

When your React app becomes slow:

```text
1️⃣ Measure
2️⃣ Find bottleneck
3️⃣ Check unnecessary renders
4️⃣ Check expensive calculations
5️⃣ Check large lists
6️⃣ Check API requests
7️⃣ Check bundle size
8️⃣ Optimize only where needed
9️⃣ Measure again
```

---

# 31. 🔍 React DevTools

React Developer Tools can help inspect:

```text
Components
Props
State
Rendering behavior
```

For performance work, profiling tools can help identify components that are taking time to render.

The important idea is:

```text
Don't guess.
Measure.
```

---

# 32. 🧠 Common Performance Problems

### Problem 1

Expensive calculation on every render.

Possible solution:

```text
useMemo()
```

---

### Problem 2

Memoized child receives a new callback every render.

Possible solution:

```text
useCallback()
```

---

### Problem 3

Child repeatedly renders even when props don't change.

Possible solution:

```text
React.memo()
```

---

### Problem 4

Huge page loads too much JavaScript initially.

Possible solution:

```text
lazy()
Suspense
```

---

### Problem 5

Rendering thousands of list items.

Possible solutions may include:

```text
Pagination
Virtualization
Memoization
Better data handling
```

The correct solution depends on the actual bottleneck.

---

# 33. ⚠️ Common Mistake #1

Thinking every re-render is bad.

It isn't.

Re-rendering is a normal part of React.

The problem is **unnecessary or expensive work**, not rendering itself.

---

# 34. ⚠️ Common Mistake #2

Using `useMemo()` everywhere.

Wrong mindset:

```text
Every variable
     ↓
useMemo()
```

Better:

```text
Is this calculation expensive?
        ↓
Does memoization actually help?
        ↓
Use useMemo if appropriate
```

---

# 35. ⚠️ Common Mistake #3

Using `useCallback()` everywhere.

A function doesn't automatically need `useCallback()`.

Ask:

```text
Does function identity matter?
Is it passed to a memoized child?
Is it part of a dependency relationship?
```

If not, a normal function may be simpler.

---

# 36. ⚠️ Common Mistake #4

Using `React.memo()` without understanding props.

If props are constantly changing references:

```text
new object
new function
```

memoization may not provide the expected benefit.

---

# 37. ⚠️ Common Mistake #5

Optimizing before measuring.

Don't immediately rewrite everything with:

```text
memo
useMemo
useCallback
```

First find the actual bottleneck.

---

# 38. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is React performance?

It refers to making React applications efficient and responsive by reducing unnecessary or expensive work.

### Q2. What is a re-render?

A re-render occurs when React runs a component again to determine its updated UI.

### Q3. Is every re-render bad?

No. Re-rendering is normal React behavior. The goal is to avoid unnecessary expensive work.

---

# 39. 🟡 Intermediate

### Q4. What is `React.memo()`?

`React.memo()` is a higher-order component that can skip rendering a component when its props have not changed according to its comparison.

### Q5. What is `useMemo()`?

`useMemo()` memoizes a calculated value.

### Q6. What is `useCallback()`?

`useCallback()` memoizes a function reference.

### Q7. Difference between `useMemo()` and `useCallback()`?

```text
useMemo()
→ memoized value

useCallback()
→ memoized function
```

---

# 40. 🔴 Advanced

### Q8. When should you use `React.memo()`?

When avoiding unnecessary child renders provides a measurable or meaningful performance benefit, especially when props remain stable.

### Q9. Why can an object prop defeat memoization?

Because objects are compared by reference, and creating a new object produces a new reference.

### Q10. What is lazy loading?

Lazy loading means loading code only when it is needed instead of loading everything immediately.

---

# 41. 📝 PRACTICE

## Practice 1 — React.memo

Create:

```text
Parent
Child
```

Add:

```text
React.memo()
```

to Child.

Use a counter in Parent.

Observe rendering with:

```js
console.log("Child rendered");
```

---

## Practice 2 — useMemo

Create a large array:

```js
const numbers = [...]
```

Create a filtering/sorting calculation.

Use:

```text
useMemo()
```

and observe when the calculation runs.

---

## Practice 3 — useCallback

Create:

```text
Parent
Child
```

Pass a function from Parent to Child.

Try both:

```text
normal function
```

and:

```text
useCallback()
```

with `React.memo()`.

Observe the difference.

---

## Practice 4 — Lazy Loading

Create:

```text
Home
Dashboard
```

Load Dashboard using:

```text
lazy()
Suspense
```

---

# 42. 🧩 MINI CHALLENGE

Build:

# ⚡ Optimized Product Dashboard

Create:

```text
App
│
├── SearchBox
├── ProductList
│      └── ProductCard
└── Counter
```

Requirements:

### 1️⃣ Search

Filter products.

### 2️⃣ Expensive filtering

Use:

```text
useMemo()
```

where appropriate.

### 3️⃣ ProductCard

Use:

```text
React.memo()
```

where it provides a benefit.

### 4️⃣ Event handler

Use:

```text
useCallback()
```

where function identity matters.

### 5️⃣ Dashboard

Lazy-load a Dashboard component.

### Architecture

```text
App
 │
 ├── SearchBox
 │
 ├── ProductList
 │      │
 │      ├── ProductCard
 │      ├── ProductCard
 │      └── ProductCard
 │
 └── Dashboard
        ↓
      lazy()
        ↓
    Suspense
```

---

# 43. 📝 10-QUESTION QUIZ

### Q1. What does `React.memo()` memoize?

**Answer:** A component's rendering based on its props.

---

### Q2. What does `useMemo()` memoize?

**Answer:** A calculated value.

---

### Q3. What does `useCallback()` memoize?

**Answer:** A function reference.

---

### Q4. What happens when state changes?

**Answer:** React schedules an update and the relevant component logic runs again to determine the new UI.

---

### Q5. Is every re-render bad?

**Answer:** No.

---

### Q6. Why can an object prop cause a memoized child to render?

**Answer:** A newly created object has a new reference.

---

### Q7. What is lazy loading?

**Answer:** Loading code when it is needed instead of loading everything immediately.

---

### Q8. Which React APIs are commonly used for lazy loading?

**Answer:**

```text
lazy()
Suspense
```

---

### Q9. Should `useMemo()` be used everywhere?

**Answer:** No. Use it when memoization provides a meaningful benefit.

---

### Q10. What is the best first step when optimizing performance?

**Answer:**

> **Measure and identify the actual bottleneck first.**

---

# 🧠 FINAL MEMORY TRICK

Remember this table:

| Tool | What it remembers |
|---|---|
| `React.memo()` | 🧩 Component rendering |
| `useMemo()` | 📦 Calculated value |
| `useCallback()` | 🔧 Function reference |
| `lazy()` | 📦 Component code loaded later |
| `Suspense` | ⏳ Loading fallback |

### ⭐ Easy Memory

```text
🧩 Component
     ↓
React.memo()

📦 Value
     ↓
useMemo()

🔧 Function
     ↓
useCallback()

📦 Code
     ↓
lazy()

⏳ Waiting
     ↓
Suspense
```

### ⭐ Golden Rule

> **Don't optimize because you can. Optimize because you measured a real problem.**

---

# ✅ CHAPTER 37 COMPLETE

You now understand:

- ⚡ React performance
- 🔄 Re-rendering
- 🧠 Memoization
- `React.memo()`
- `useMemo()`
- `useCallback()`
- Referential equality
- Large-list optimization
- Lazy loading
- `Suspense`
- Performance measurement mindset
- Common performance mistakes
- Interview questions
- Practice exercises
- Optimization challenge

---

# 🚀 NEXT CHAPTER

## Chapter 38 — Lazy Loading and Suspense

We will go deeper into:

```text
📦 Code Splitting
⚡ lazy()
⏳ Suspense
🧩 Route-based Lazy Loading
📄 Component Lazy Loading
🔄 Loading Fallbacks
🚀 Performance Benefits
⚠️ Common Mistakes
```

Type **NEXT** to continue.