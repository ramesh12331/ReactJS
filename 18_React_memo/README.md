# 🎯 ReactJS — Chapter 17
# `useCallback()`

## 🎯 Chapter Goal

In this chapter, you will learn:

- 🔗 What is a function reference?
- 🔄 Why functions can cause unnecessary re-renders
- 🎯 What is `useCallback()`?
- 📝 `useCallback()` syntax
- 📦 Dependency array
- 🧩 `useCallback()` with child components
- ⚡ `useCallback()` + `React.memo`
- 🛠️ Practical examples
- ⚠️ Common mistakes
- 🎤 Interview questions
- 🧪 Practice
- 🏆 Mini challenge

These are the topics specified for Chapter 17 in your React course source.

---

# 1️⃣ What Is `useCallback()`?

`useCallback()` is a React Hook used to **remember a function reference** between renders.

The important word is:

# 🔗 FUNCTION

You learned in the previous chapter:

```text
useMemo()
    ↓
Remember a VALUE
```

Now:

```text
useCallback()
    ↓
Remember a FUNCTION
```

### 🧠 Easy memory trick

```text
useMemo
→ value

useCallback
→ function
```

---

# 2️⃣ Why Do We Need `useCallback()`?

Let's first understand a problem.

Suppose we have a parent component:

```jsx
function Parent() {

    const handleClick = () => {
        console.log("Clicked");
    };

    return (
        <Child onClick={handleClick} />
    );
}
```

Every time `Parent` renders, JavaScript creates a new function:

```jsx
const handleClick = () => {
    console.log("Clicked");
};
```

Even though the code looks the same, the function reference can be different.

This becomes important when we pass the function to a child component.

---

# 3️⃣ What Is a Function Reference? 🔗

This is a very important JavaScript concept.

Consider:

```jsx
const sayHello = () => {
    console.log("Hello");
};
```

` sayHello ` refers to a function.

When we write:

```jsx
<Child onClick={sayHello} />
```

we are passing the function reference.

We are **not calling** the function.

---

# 4️⃣ Reference vs Calling

### Function reference ✅

```jsx
<Child onClick={sayHello} />
```

Meaning:

> Here is the function. Use it when needed.

---

### Function call ❌

```jsx
<Child onClick={sayHello()} />
```

Meaning:

> Execute the function now and pass its result.

This is an important difference.

---

# 5️⃣ Real-World Analogy 📞

Imagine you give your friend a phone number.

```text
📱 Phone Number
      ↓
Friend can call later
```

You didn't make the call.

You only gave the contact.

Similarly:

```jsx
onClick={handleClick}
```

means:

```text
Give React the function
        ↓
React can call it later
```

---

# 6️⃣ The Re-Creation Problem 🔄

Consider:

```jsx
function App() {

    const handleClick = () => {
        console.log("Hello");
    };

    return <button onClick={handleClick}>Click</button>;
}
```

When the component renders again, the function can be created again.

Conceptually:

```text
Render 1
   ↓
handleClick → Function A

Render 2
   ↓
handleClick → Function B

Render 3
   ↓
handleClick → Function C
```

The function code is the same.

But the function reference can be different.

---

# 7️⃣ Why Can This Matter?

For a normal button:

```jsx
<button onClick={handleClick}>
    Click
</button>
```

this usually isn't a problem.

You should **not** immediately use `useCallback()` everywhere.

The problem becomes more important when:

```text
Parent
   ↓
passes function
   ↓
Child
   ↓
Child is optimized with React.memo
```

Then changing function references can cause the child to render again.

Your course specifically connects `useCallback()` with `React.memo`, which we will explore more in Chapter 18.

---

# 8️⃣ Basic `useCallback()` Syntax

First import it:

```jsx
import { useCallback } from "react";
```

Then:

```jsx
const handleClick = useCallback(() => {
    console.log("Hello");
}, []);
```

Basic structure:

```jsx
const functionName = useCallback(
    () => {
        // function logic
    },
    [dependencies]
);
```

There are two main parts:

```text
1️⃣ Function
2️⃣ Dependency array
```

---

# 9️⃣ Simple Example

```jsx
import { useCallback } from "react";

function App() {

    const handleClick = useCallback(() => {
        console.log("Button clicked");
    }, []);

    return (
        <button onClick={handleClick}>
            Click Me
        </button>
    );
}

export default App;
```

---

# 🔟 What Is Happening?

First render:

```text
App renders
   ↓
handleClick function created
   ↓
React remembers the function reference
```

Later render:

```text
App renders again
   ↓
Dependencies haven't changed
   ↓
React can reuse the function reference
```

---

# 1️⃣1️⃣ Empty Dependency Array

Example:

```jsx
const handleClick = useCallback(() => {
    console.log("Hello");
}, []);
```

The dependency array is:

```jsx
[]
```

This tells React that the callback has no listed reactive dependencies.

The important idea is:

```text
No dependency changes
        ↓
Function reference can remain stable
```

---

# 1️⃣2️⃣ Dependency Example

Suppose our function uses:

```jsx
const name = "Ramesh";
```

We could write:

```jsx
const handleClick = useCallback(() => {
    console.log(name);
}, [name]);
```

Here:

```text
Function uses
    ↓
name
```

Therefore:

```jsx
[name]
```

is the dependency.

---

# 1️⃣3️⃣ What Happens When a Dependency Changes?

Suppose:

```text
name = "Ramesh"
```

The function is created/memoized.

Later:

```text
name = "Kumar"
```

The dependency changed.

Therefore React needs to create a new callback function reference.

Flow:

```text
name = Ramesh
      ↓
callback A

name changes
      ↓
name = Kumar
      ↓
callback B
```

---

# 1️⃣4️⃣ Dry Run 🧠

Consider:

```jsx
const handleMessage = useCallback(() => {
    console.log(name);
}, [name]);
```

Initial:

```text
name = "Ramesh"
```

React creates:

```text
Function A
```

Next render:

```text
name = "Ramesh"
```

Dependency unchanged.

React can reuse:

```text
Function A
```

Later:

```text
name = "Suresh"
```

Dependency changed.

React creates:

```text
Function B
```

---

# 1️⃣5️⃣ Parent → Child Example

Let's create two components.

### Parent

```jsx
function Parent() {

    const handleClick = () => {
        console.log("Hello from Parent");
    };

    return (
        <Child onClick={handleClick} />
    );
}
```

### Child

```jsx
function Child({ onClick }) {

    return (
        <button onClick={onClick}>
            Click
        </button>
    );
}
```

Flow:

```text
Parent
   ↓
handleClick
   ↓
Child
   ↓
onClick
```

This is a common React pattern.

---

# 1️⃣6️⃣ Where Does `useCallback()` Help?

Now imagine:

```text
Parent
  ↓
Child
  ↓
GrandChild
```

The parent passes many functions.

If the parent renders again, function references may change.

That can affect memoized children.

`useCallback()` can help keep a callback reference stable when its dependencies haven't changed.

---

# 1️⃣7️⃣ `useCallback()` + `React.memo()` ⭐⭐⭐

This is the most important practical combination.

Imagine:

```jsx
const Child = React.memo(function Child({ onClick }) {

    console.log("Child rendered");

    return (
        <button onClick={onClick}>
            Click
        </button>
    );
});
```

`React.memo()` can skip a child re-render when its props are considered unchanged.

But:

```jsx
const handleClick = () => {};
```

can create a new function reference on every parent render.

Then:

```text
Old function
    ≠
New function
```

From the prop comparison perspective, the function prop changed.

---

# 1️⃣8️⃣ Solution

Use:

```jsx
useCallback()
```

Example:

```jsx
const handleClick = useCallback(() => {
    console.log("Hello");
}, []);
```

Now the callback reference can remain stable while its dependencies remain unchanged.

Together:

```text
useCallback
     +
React.memo
     ↓
Stable function prop
     ↓
Potentially fewer unnecessary child renders
```

---

# 1️⃣9️⃣ Complete Example

```jsx
import { useCallback, useState, memo } from "react";

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
        console.log("Child button clicked");
    }, []);

    return (
        <div>

            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase Count
            </button>

            <Child onClick={handleClick} />

        </div>
    );
}

export default App;
```

---

# 2️⃣0️⃣ Understand This Example

We have:

```text
App
 ├── Count
 ├── Increase Button
 └── Child
```

When we click:

```text
Increase Count
```

the parent state changes.

Therefore:

```text
Parent renders again
```

But:

```jsx
const handleClick = useCallback(() => {
    console.log("Child button clicked");
}, []);
```

keeps the callback reference stable because its dependencies haven't changed.

And:

```jsx
memo(...)
```

helps the child avoid unnecessary rendering when its props remain unchanged.

---

# 2️⃣1️⃣ Important: `useCallback()` Alone Is Not Magic 🪄

This is very important.

Don't think:

```text
useCallback()
    ↓
No re-render anywhere
```

That is incorrect.

`useCallback()` only memoizes the function reference.

It does not automatically stop every component from rendering.

The benefit is especially useful when a function is passed to a memoized child.

---

# 2️⃣2️⃣ `useCallback()` vs `useMemo()`

You learned `useMemo()` in Chapter 16.

Let's compare.

### `useMemo()`

```jsx
const total = useMemo(() => {
    return price * quantity;
}, [price, quantity]);
```

It memoizes:

```text
VALUE
```

---

### `useCallback()`

```jsx
const handleClick = useCallback(() => {
    console.log("Clicked");
}, []);
```

It memoizes:

```text
FUNCTION REFERENCE
```

---

# 2️⃣3️⃣ Easy Comparison Table

| Hook | What it remembers? | Example |
|---|---|---|
| `useMemo()` | Value | calculated total |
| `useCallback()` | Function reference | event handler |
| `useState()` | State | count |
| `useEffect()` | Side effect logic | API call |

### 🧠 Memory Trick

```text
useState
→ STATE

useMemo
→ VALUE

useCallback
→ FUNCTION

useEffect
→ SIDE EFFECT
```

---

# 2️⃣4️⃣ Practical Example — Search Button 🔎

Suppose:

```jsx
function Search() {

    const [query, setQuery] = useState("");

    const handleSearch = useCallback(() => {

        console.log("Searching:", query);

    }, [query]);

    return (
        <button onClick={handleSearch}>
            Search
        </button>
    );
}
```

The function uses:

```text
query
```

Therefore:

```jsx
[query]
```

is a dependency.

When query changes:

```text
query changes
    ↓
callback updates
```

---

# 2️⃣5️⃣ Practical Example — Passing ID

Suppose we have:

```jsx
function ProductList() {

    const handleDelete = useCallback((id) => {

        console.log("Delete:", id);

    }, []);

    return (
        <Product
            id={101}
            onDelete={handleDelete}
        />
    );
}
```

Child:

```jsx
function Product({ id, onDelete }) {

    return (
        <button onClick={() => onDelete(id)}>
            Delete
        </button>
    );
}
```

Flow:

```text
ProductList
     ↓
handleDelete
     ↓
Product
     ↓
onDelete(id)
     ↓
Delete product
```

This pattern is common in lists and CRUD applications.

---

# 2️⃣6️⃣ CRUD Example 🗑️

Imagine an employee list:

```text
Employee
----------------
1. Ramesh
2. Suresh
3. Kumar
```

You want:

```text
[Delete]
```

The parent can provide:

```jsx
const handleDelete = useCallback((id) => {

    console.log("Delete employee:", id);

}, []);
```

Then:

```jsx
<Employee
    id={101}
    onDelete={handleDelete}
/>
```

The child calls:

```jsx
onDelete(id);
```

This is a practical pattern you will use later in CRUD applications.

---

# 2️⃣7️⃣ Passing Arguments

Sometimes your callback needs an argument.

```jsx
const handleDelete = useCallback((id) => {

    console.log(id);

}, []);
```

Child:

```jsx
<button onClick={() => onDelete(id)}>
    Delete
</button>
```

Notice:

```jsx
onClick={() => onDelete(id)}
```

We use an arrow function because we need to pass `id` when the user clicks.

---

# 2️⃣8️⃣ Common Mistake ❌

Wrong:

```jsx
<button onClick={onDelete(id)}>
    Delete
</button>
```

Why?

Because:

```text
onDelete(id)
```

calls the function immediately during rendering.

Correct:

```jsx
<button onClick={() => onDelete(id)}>
    Delete
</button>
```

Now it executes when the button is clicked.

---

# 2️⃣9️⃣ Common Mistake — Empty Dependency Array Without Thinking

Suppose:

```jsx
const handleMessage = useCallback(() => {

    console.log(name);

}, []);
```

If `name` is a reactive value that the callback uses, blindly leaving it out can cause incorrect behavior.

Instead, the dependency list should correctly represent the values used by the callback.

For example:

```jsx
const handleMessage = useCallback(() => {

    console.log(name);

}, [name]);
```

### ⭐ Rule

> Don't choose dependencies just to make the function "stable." Choose dependencies based on what the callback actually uses.

---

# 3️⃣0️⃣ Common Mistake — Using `useCallback()` Everywhere

Don't do this:

```jsx
const add = useCallback(() => {
    return 10 + 20;
}, []);
```

for a tiny operation without a real reason.

The purpose is performance optimization, not making every function use a Hook.

---

# 3️⃣1️⃣ Common Mistake — Thinking `useCallback()` Makes the Function Faster

`useCallback()` does not make the function itself execute faster.

Its main purpose is to preserve the function reference between renders when dependencies haven't changed.

Think:

```text
❌ Faster function

✅ Stable function reference
```

---

# 3️⃣2️⃣ Common Mistake — Forgetting `React.memo`

Suppose you do:

```jsx
const handleClick = useCallback(() => {
    console.log("Hello");
}, []);
```

but the child is not memoized and there is no other reason the stable reference matters.

You may not get a meaningful rendering optimization.

This is why:

```text
useCallback
+
React.memo
```

is a common combination.

---

# 3️⃣3️⃣ When Should You Use `useCallback()`? ✅

Consider it when:

### 1. A function is passed to a child

```text
Parent
 ↓
callback
 ↓
Child
```

### 2. The child uses `React.memo`

```text
React.memo
+
useCallback
```

### 3. Function identity matters

For example, when another Hook or optimization depends on the callback reference.

### 4. You have identified a real rendering/performance issue

Measure first when possible.

---

# 3️⃣4️⃣ When Should You NOT Use It? ❌

Avoid unnecessary `useCallback()` when:

- the function is very simple
- the function isn't passed to a memoized child
- there is no meaningful performance issue
- it makes your code harder to understand

Beginner rule:

> **First make the application correct. Then optimize where needed.**

---

# 3️⃣5️⃣ Visual Flow 🧠

Without `useCallback()`:

```text
Parent Render
      ↓
New function reference
      ↓
Child receives new function
      ↓
Child may render again
```

With `useCallback()`:

```text
Parent Render
      ↓
Dependencies checked
      ↓
Dependencies unchanged
      ↓
Same callback reference
      ↓
Memoized child may skip render
```

---

# 3️⃣6️⃣ Important Difference: Value vs Reference

Suppose:

```jsx
const name = "Ramesh";
```

A string is a value.

But:

```jsx
const handleClick = () => {};
```

is a function object.

React's prop comparison cares about the function reference.

So:

```text
Function A
   ↓
different reference
   ↓
Function B
```

even if they contain the same code.

This is one reason `useCallback()` exists.

---

# 3️⃣7️⃣ Practical Architecture

In a real React application:

```text
App
│
├── Header
├── Sidebar
│
├── ProductList
│      │
│      ├── ProductCard
│      ├── ProductCard
│      └── ProductCard
│
└── Footer
```

A parent might pass:

```text
onDelete
onEdit
onAddToCart
onFavorite
```

to many children.

For performance-sensitive areas, stable callback references can become useful.

---

# 3️⃣8️⃣ Best Practices ✅

### ✅ Use `useCallback()` for a reason

Don't use it everywhere.

### ✅ Understand dependencies

If the callback uses:

```text
userId
productId
search
```

consider those dependencies when they are reactive values.

### ✅ Understand `React.memo()`

`useCallback()` becomes especially useful when passing callbacks to memoized children.

### ✅ Keep code simple

Optimization should not make simple components unnecessarily complicated.

### ✅ Optimize after understanding rendering

First understand:

```text
render
props
state
references
```

Then optimize.

---

# 🎤 3️⃣9️⃣ Interview Questions

## Q1. What is `useCallback()`?

`useCallback()` is a React Hook that memoizes a function reference between renders until its dependencies change.

---

## Q2. Why do we use `useCallback()`?

It can help avoid unnecessary changes to function props, especially when callbacks are passed to memoized child components.

---

## Q3. What does `useCallback()` return?

It returns a memoized callback function.

---

## Q4. What is the difference between `useMemo()` and `useCallback()`?

```text
useMemo()
→ memoizes a value

useCallback()
→ memoizes a function reference
```

---

## Q5. What is a function reference?

It is the reference to a function object.

Example:

```jsx
const handleClick = () => {};
```

`handleClick` refers to that function.

---

## Q6. Does `useCallback()` prevent component rendering?

No.

It only memoizes the function reference.

---

## Q7. Why is `useCallback()` often used with `React.memo()`?

Because a memoized child can skip rendering when its props are unchanged, and `useCallback()` can keep a callback prop reference stable when dependencies don't change.

---

## Q8. What is the dependency array?

It contains reactive values that the callback depends on.

---

## Q9. Should every function use `useCallback()`?

No.

Use it when there is a meaningful reason, especially for performance-sensitive cases.

---

## Q10. Does `useCallback()` make a function execute faster?

No.

Its main purpose is to preserve the function reference between renders when dependencies remain unchanged.

---

# 🧪 4️⃣0️⃣ Practice Questions

## Practice 1 — Basic Callback

Create:

```jsx
const handleClick = ...
```

Use `useCallback()`.

When the button is clicked, print:

```text
Hello React
```

---

## Practice 2 — User Name

Create:

```text
name = "Ramesh"
```

Create a callback that prints:

```text
Hello Ramesh
```

Think about what dependency is required.

---

## Practice 3 — Delete Product

Create:

```text
productId = 101
```

Create:

```text
handleDelete(id)
```

Pass it to a child component.

The child should have:

```text
[Delete]
```

button.

---

## Practice 4 — Parent and Child

Create:

```text
Parent
   ↓
Child
```

Parent should pass:

```text
onClick
```

to Child.

Use:

```text
useCallback
+
React.memo
```

and observe rendering using:

```jsx
console.log("Child rendered");
```

---

# 🏆 4️⃣1️⃣ Mini Challenge

Build a small:

# 👨‍💼 Employee List

Employees:

```text
101 → Ramesh
102 → Suresh
103 → Kumar
```

Create:

```text
EmployeeList
       ↓
EmployeeCard
```

Each card should have:

```text
Ramesh

[Delete]
```

Parent should have:

```jsx
const handleDelete = ...
```

Use:

```text
useCallback()
```

and:

```text
React.memo()
```

for the child.

### 🧠 Think Before Coding

1. What is the parent?
2. What is the child?
3. What function should the parent create?
4. What argument should the function receive?
5. Which component receives the callback?
6. Why might `useCallback()` help here?
7. Why might `React.memo()` help here?

Don't immediately copy a solution.

Try to build the logic yourself first.

---

# 📝 4️⃣2️⃣ 10-Question Quiz

## Q1. What does `useCallback()` memoize?

### ✅ Answer

A **function reference**.

---

## Q2. What does `useMemo()` memoize?

### ✅ Answer

A **calculated value**.

---

## Q3. Does `useCallback()` stop all re-renders?

### ❌ Answer

No.

It only helps keep a function reference stable when dependencies haven't changed.

---

## Q4. What usually happens when a dependency changes?

### ✅ Answer

React creates a new callback reference for the changed dependencies.

---

## Q5. Why can a new function reference matter?

### ✅ Answer

Because a function passed as a prop can appear changed to a child, which can matter for memoized components.

---

## Q6. What Hook is often combined with `useCallback()`?

### ✅ Answer

```jsx
React.memo
```

---

## Q7. Is this correct?

```jsx
<button onClick={handleClick}>
```

### ✅ Answer

Yes.

It passes the function reference.

---

## Q8. Is this usually correct?

```jsx
<button onClick={handleClick()}>
```

### ❌ Answer

No.

It calls the function during rendering instead of giving React the event handler.

---

## Q9. Does `useCallback()` make a function's internal code faster?

### ❌ Answer

No.

It mainly preserves the function reference between renders when dependencies don't change.

---

## Q10. Should every function use `useCallback()`?

### ❌ Answer

No.

Use it when there is a meaningful performance or reference-stability reason.

---

# 📚 4️⃣3️⃣ Quick Revision

| Concept | Meaning |
|---|---|
| 🎯 `useCallback()` | Memoizes a function reference |
| 🔗 Function reference | Reference to a function object |
| 📦 Dependency | Value used by callback |
| 🔄 Dependency changes | New callback reference may be created |
| ♻️ Dependency unchanged | Previous callback reference can be reused |
| 🧩 `React.memo` | Can skip child rendering when props are unchanged |
| ⚡ Performance | Main reason to consider these optimizations |
| 🟢 `useState` | Stores state |
| 🔵 `useMemo` | Memoizes a value |
| 🟣 `useCallback` | Memoizes a function |
| 🟠 `useEffect` | Handles side effects |

---

# 🧠 4️⃣4️⃣ Final Memory Trick

Remember these four Hooks:

```text
useState
   ↓
📦 STATE

useMemo
   ↓
💰 VALUE

useCallback
   ↓
🔗 FUNCTION

useEffect
   ↓
⚡ SIDE EFFECT
```

And the most important `useCallback()` flow:

```text
Parent
   ↓
creates callback
   ↓
useCallback()
   ↓
stable function reference
   ↓
Child receives function
   ↓
React.memo()
   ↓
Potentially avoid unnecessary child render
```

### ⭐ One simple sentence

> **`useCallback()` helps remember a function reference between renders when its dependencies have not changed.**

---

# ✅ Chapter 17 Checklist

Before moving forward, make sure you can explain:

- [ ] What is a function reference?
- [ ] Why functions can have different references between renders
- [ ] What is `useCallback()`?
- [ ] `useCallback()` syntax
- [ ] Dependency array
- [ ] What happens when dependencies change
- [ ] What happens when dependencies don't change
- [ ] Parent → Child callback
- [ ] `useCallback()` + `React.memo()`
- [ ] `useMemo()` vs `useCallback()`
- [ ] Function reference vs function call
- [ ] Common dependency mistakes
- [ ] When NOT to use `useCallback()`
- [ ] How callbacks are useful in CRUD applications

---

# 🚀 NEXT CHAPTER

## ⚛️ CHAPTER 18 — `React.memo`

Next we will learn:

```text
⚛️ React.memo
      ↓
Component Re-rendering
      ↓
Props Comparison
      ↓
Performance Optimization
      ↓
React.memo + useCallback
      ↓
Common Mistakes
```

Chapter 18 in your source specifically covers **component re-rendering, `React.memo`, props comparison, performance optimization, and common mistakes**.

Type **`NEXT`** when you are ready for **Chapter 18 — `React.memo`**.