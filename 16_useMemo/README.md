# ⚡ ReactJS — Chapter 16
# `useMemo()`

## 🎯 Chapter Goal

By the end of this chapter, you will understand:

- 🧠 What is memoization?
- ⚡ What is `useMemo()`?
- 💰 What is an expensive calculation?
- 📝 `useMemo()` syntax
- 📦 Dependency array
- 🔄 When React recalculates a value
- ✅ When to use `useMemo()`
- ❌ When NOT to use `useMemo()`
- 🔍 Practical examples
- ⚠️ Common mistakes
- 🎤 Interview questions
- 🧪 Practice
- 🏆 Mini challenge

---

# 1️⃣ First: Why Do We Need `useMemo()`?

Before learning `useMemo()`, you need to understand one important React concept:

> **A component can render again when its state or props change.**

Suppose we have:

```jsx
function App() {
    // component logic

    return <h1>Hello</h1>;
}
```

If the component renders again, JavaScript inside the component can run again.

For example:

```jsx
function App() {

    const result = calculateSomething();

    return <h1>{result}</h1>;
}
```

Every render can call:

```text
calculateSomething()
```

again.

For a very simple calculation, this is completely fine.

But what if the calculation is expensive?

That is where memoization can help.

---

# 2️⃣ What Is Memoization? 🧠

**Memoization** means:

> Save a calculation result and reuse it when the input has not changed.

Simple example:

```text
Calculation:
10 × 20

Result:
200
```

If we ask again:

```text
10 × 20
```

we already know:

```text
200
```

Instead of calculating unnecessarily again, we can reuse the previous result.

Think:

```text
Input
  ↓
Calculation
  ↓
Result
  ↓
Save result
```

Next time:

```text
Same input?
    ↓
   YES
    ↓
Use saved result
```

---

# 3️⃣ Real-World Analogy 📚

Imagine you calculate your monthly salary:

```text
Basic Salary = ₹40,000
Bonus = ₹5,000
Other = ₹5,000

Total = ₹50,000
```

You write the answer in a notebook.

Tomorrow someone asks:

> "What was the total?"

You don't calculate everything again.

You look at your notebook:

```text
₹50,000
```

That is similar to memoization.

```text
Calculate once
      ↓
Remember result
      ↓
Reuse when inputs are unchanged
```

---

# 4️⃣ What Is `useMemo()`?

`useMemo()` is a React Hook that lets you **memoize a calculated value**.

Basic idea:

```text
useMemo()
   ↓
Calculate value
   ↓
Remember value
   ↓
Reuse when dependencies don't change
```

The important word is:

# 🧠 VALUE

`useMemo()` memoizes a **value**, not a function.

Later, in Chapter 17, you will learn `useCallback()`, which is used for memoizing a function reference.

---

# 5️⃣ Basic Syntax

```jsx
const result = useMemo(() => {
    return calculation();
}, [dependencies]);
```

Let's break it down.

```jsx
useMemo(
    () => {
        // calculation
    },
    [dependencies]
);
```

There are two important parts:

```text
1️⃣ Calculation
2️⃣ Dependency array
```

---

# 6️⃣ First Simple Example

```jsx
import { useMemo } from "react";

function App() {

    const number = 10;

    const result = useMemo(() => {
        return number * 2;
    }, [number]);

    return (
        <h1>{result}</h1>
    );
}

export default App;
```

Output:

```text
20
```

---

# 7️⃣ Understand the Code

### Step 1

```jsx
import { useMemo } from "react";
```

We import the Hook.

---

### Step 2

```jsx
const number = 10;
```

Our input is:

```text
10
```

---

### Step 3

```jsx
const result = useMemo(() => {
```

We tell React:

> Calculate this value and remember it according to the dependencies.

---

### Step 4

```jsx
return number * 2;
```

Calculation:

```text
10 × 2
   ↓
20
```

---

### Step 5

```jsx
[number]
```

This is the dependency array.

It tells React that the calculation depends on:

```text
number
```

---

# 8️⃣ What Is a Dependency? 📦

A dependency is a value that the calculation uses.

Example:

```jsx
const number = 10;

const result = useMemo(() => {
    return number * 2;
}, [number]);
```

The calculation uses:

```text
number
```

Therefore:

```jsx
[number]
```

is the dependency.

Think:

```text
number
   ↓
calculation
   ↓
result
```

---

# 9️⃣ What Happens When the Dependency Does NOT Change?

Suppose:

```text
number = 10
```

First render:

```text
10 × 2
 ↓
20
```

React remembers:

```text
20
```

If the component renders again but:

```text
number = 10
```

is still the same, React can reuse the memoized result.

Conceptually:

```text
First render
    ↓
Calculate
    ↓
20
    ↓
Remember

Second render
    ↓
number still 10
    ↓
Reuse 20
```

---

# 🔟 What Happens When the Dependency Changes?

Suppose:

```text
number = 10
```

Result:

```text
20
```

Later:

```text
number = 20
```

Now the dependency changed.

So React needs to calculate again:

```text
20 × 2
 ↓
40
```

Flow:

```text
number = 10
     ↓
    20

number = 20
     ↓
    40
```

---

# 1️⃣1️⃣ Important Mental Model 🧠

Remember:

```text
Dependency unchanged
        ↓
Reuse previous value
```

But:

```text
Dependency changed
        ↓
Calculate again
```

This is the basic idea of `useMemo()`.

---

# 1️⃣2️⃣ Why Do We Need Expensive Calculations?

Let's imagine:

```jsx
function calculateTotal(products) {

    // Imagine this takes a lot of work

    return products.reduce(
        (total, product) =>
            total + product.price,
        0
    );
}
```

For 5 products:

```text
5 products
 ↓
Very easy
```

For 5,000 products:

```text
5,000 products
 ↓
More work
```

For a very complex calculation:

```text
Large data
    +
Complex calculation
    ↓
More processing
```

If the calculation doesn't need to run again, memoization may help avoid unnecessary work.

---

# 1️⃣3️⃣ Practical Example — Product Total 💰

```jsx
import { useMemo } from "react";

function App() {

    const products = [
        { id: 1, name: "Laptop", price: 50000 },
        { id: 2, name: "Mouse", price: 1000 },
        { id: 3, name: "Keyboard", price: 2000 }
    ];

    const total = useMemo(() => {

        console.log("Calculating total...");

        return products.reduce(
            (sum, product) => sum + product.price,
            0
        );

    }, [products]);

    return (
        <div>

            <h1>Total: ₹{total}</h1>

        </div>
    );
}

export default App;
```

Total:

```text
50000
+ 1000
+ 2000
------
53000
```

Output:

```text
Total: ₹53000
```

---

# 1️⃣4️⃣ Dry Run 🧠

Products:

```text
Laptop    ₹50,000
Mouse     ₹1,000
Keyboard  ₹2,000
```

Calculation:

```text
50,000 + 1,000 + 2,000
        ↓
      53,000
```

So:

```text
total = 53000
```

React displays:

```text
Total: ₹53000
```

---

# 1️⃣5️⃣ But There Is an Important Problem ⚠️

Look carefully at:

```jsx
const products = [
    { id: 1, name: "Laptop", price: 50000 },
    { id: 2, name: "Mouse", price: 1000 },
    { id: 3, name: "Keyboard", price: 2000 }
];
```

This array is created inside the component.

On every render, JavaScript creates a new array.

So:

```jsx
[products]
```

may not behave like a stable dependency.

This is an important concept.

Don't blindly add `useMemo()`.

The dependency itself needs to be considered carefully.

---

# 1️⃣6️⃣ Better Example With State

Let's make the example more realistic.

```jsx
import { useMemo, useState } from "react";

function App() {

    const [search, setSearch] = useState("");

    const products = [
        { id: 1, name: "Laptop", price: 50000 },
        { id: 2, name: "Mouse", price: 1000 },
        { id: 3, name: "Keyboard", price: 2000 }
    ];

    const filteredProducts = useMemo(() => {

        console.log("Filtering products...");

        return products.filter(product =>
            product.name
                .toLowerCase()
                .includes(search.toLowerCase())
        );

    }, [search]);

    return (
        <div>

            <input
                value={search}
                onChange={(e) => setSearch(e.target.value)}
                placeholder="Search product"
            />

            {filteredProducts.map(product => (
                <p key={product.id}>
                    {product.name}
                </p>
            ))}

        </div>
    );
}

export default App;
```

---

# 1️⃣7️⃣ What Is Happening Here?

User types:

```text
Lap
```

The dependency:

```jsx
[search]
```

changes.

Therefore React recalculates:

```text
filteredProducts
```

When `search` stays the same, the memoized value can be reused.

---

# 1️⃣8️⃣ Another Important Example — Sorting

Imagine:

```jsx
const sortedProducts = useMemo(() => {

    return [...products].sort(
        (a, b) => a.price - b.price
    );

}, [products]);
```

This calculates products sorted by price.

Example:

```text
Laptop     ₹50,000
Mouse      ₹1,000
Keyboard   ₹2,000
```

After sorting:

```text
Mouse      ₹1,000
Keyboard   ₹2,000
Laptop     ₹50,000
```

If the `products` dependency has not changed, the memoized result can be reused.

---

# 1️⃣9️⃣ `useMemo()` Is About Performance ⚡

The main reason to use `useMemo()` is:

```text
Performance optimization
```

Especially when:

```text
Calculation
   ↓
Expensive
   ↓
Repeated unnecessarily
```

You can potentially reduce unnecessary calculations.

---

# 2️⃣0️⃣ What Is an Expensive Calculation?

An expensive calculation is not necessarily one mathematical operation.

It means a calculation that takes meaningful processing time.

Examples can include:

```text
🔢 Large data processing
🔍 Filtering huge datasets
📊 Complex calculations
📈 Large sorting operations
🧮 Heavy transformations
```

Example:

```text
10 records
   ↓
Very cheap

1,000,000 records
   ↓
Potentially expensive
```

---

# 2️⃣1️⃣ Don't Use `useMemo()` Everywhere ❌

This is extremely important.

You should NOT think:

> "I learned `useMemo()`, so I should use it everywhere."

Wrong approach.

For example:

```jsx
const total = useMemo(() => {
    return 10 + 20;
}, []);
```

This is unnecessary.

Just write:

```jsx
const total = 10 + 20;
```

The calculation is extremely cheap.

---

# 2️⃣2️⃣ When NOT to Use `useMemo()` ❌

Avoid unnecessary memoization when:

### 1. Calculation is very cheap

```jsx
const result = age + 1;
```

No need.

---

### 2. Code becomes harder to understand

If adding `useMemo()` makes simple code confusing, don't use it without a good reason.

---

### 3. There is no performance problem

Optimization should solve a real problem.

Don't optimize code simply because you can.

---

# 2️⃣3️⃣ Simple Rule ⭐

Ask yourself:

```text
❓ Is this calculation expensive?

        ↓

     NO → Don't use useMemo

        ↓

     YES

        ↓

❓ Does it run unnecessarily?

        ↓

     YES → useMemo may help
```

This is a good beginner decision rule.

---

# 2️⃣4️⃣ `useMemo()` Does NOT Stop Component Rendering

This is a very important point.

Suppose:

```jsx
const result = useMemo(() => {
    return expensiveCalculation(data);
}, [data]);
```

`useMemo()` memoizes:

```text
result
```

It does NOT mean:

```text
"the entire component will never render again"
```

The component can still render.

`useMemo()` helps avoid recalculating a particular value when its dependencies haven't changed.

---

# 2️⃣5️⃣ `useMemo()` vs `useState()`

These are completely different.

### `useState()`

Stores changing state:

```jsx
const [count, setCount] = useState(0);
```

### `useMemo()`

Caches a calculated value:

```jsx
const total = useMemo(() => {
    return calculateTotal(products);
}, [products]);
```

Remember:

```text
useState
   ↓
Store state

useMemo
   ↓
Memoize calculation result
```

---

# 2️⃣6️⃣ `useMemo()` vs `useEffect()`

Another important interview question.

### `useMemo()`

Used to calculate/memoize a value.

```jsx
const total = useMemo(() => {
    return calculateTotal(products);
}, [products]);
```

### `useEffect()`

Used for side effects.

Examples:

```text
🌐 API calls
⏱️ Timers
🎧 Event listeners
🌍 Browser interactions
```

Simple memory trick:

```text
useMemo
   ↓
Calculate a value

useEffect
   ↓
Perform a side effect
```

---

# 2️⃣7️⃣ Example Comparison

### `useMemo`

```jsx
const total = useMemo(() => {
    return products.reduce(
        (sum, product) => sum + product.price,
        0
    );
}, [products]);
```

Purpose:

```text
Calculate total
```

---

### `useEffect`

```jsx
useEffect(() => {

    console.log("Component changed");

}, [products]);
```

Purpose:

```text
Perform an effect
```

These Hooks solve different problems.

---

# 2️⃣8️⃣ Dependency Array

Example:

```jsx
useMemo(() => {
    return calculateTotal(products);
}, [products]);
```

Dependency:

```text
products
```

The calculation depends on:

```text
products
```

So we list it.

---

# 2️⃣9️⃣ Multiple Dependencies

Sometimes a calculation uses more than one value.

Example:

```jsx
const total = useMemo(() => {

    return price * quantity * discount;

}, [price, quantity, discount]);
```

Dependencies:

```text
price
quantity
discount
```

If any of them changes:

```text
Recalculate
```

---

# 3️⃣0️⃣ Dependency Example

Suppose:

```text
price = 100
quantity = 2
discount = 10
```

Calculation:

```text
100 × 2 - 10
= 190
```

If:

```text
quantity = 3
```

changed:

```text
100 × 3 - 10
= 290
```

The dependency changed, so recalculation is needed.

---

# 3️⃣1️⃣ Empty Dependency Array

You may see:

```jsx
const value = useMemo(() => {
    return expensiveCalculation();
}, []);
```

The dependency list is empty.

This means the memoized calculation has no reactive dependencies listed.

But don't use `[]` just to "make it run once."

The dependency list should correctly describe the values used by the calculation.

This is an important best practice.

---

# 3️⃣2️⃣ Common Mistake — Missing Dependency ❌

Suppose:

```jsx
const total = useMemo(() => {
    return price * quantity;
}, [price]);
```

But the calculation uses:

```text
price
quantity
```

Then `quantity` is missing.

Better:

```jsx
const total = useMemo(() => {
    return price * quantity;
}, [price, quantity]);
```

The dependency list should match the reactive values used by the calculation.

---

# 3️⃣3️⃣ Common Mistake — Thinking `useMemo()` Makes Everything Faster

Not necessarily.

`useMemo()` itself has overhead.

For a tiny calculation:

```jsx
const result = 10 + 20;
```

adding memoization can be unnecessary.

The goal is not:

```text
Maximum useMemo
```

The goal is:

```text
Useful optimization
```

---

# 3️⃣4️⃣ Common Mistake — Using `useMemo()` Instead of `useState`

Wrong thinking:

```text
"I need a value that changes, so I'll use useMemo."
```

No.

If it is component state:

```jsx
useState()
```

If it is a derived calculation that you want to memoize:

```jsx
useMemo()
```

---

# 3️⃣5️⃣ Derived Value 🧠

A derived value is calculated from other data.

Example:

```text
price = ₹100
quantity = 3

total = ₹300
```

`total` is derived from:

```text
price
+
quantity
```

You could write:

```jsx
const total = price * quantity;
```

If the calculation is expensive, you might consider:

```jsx
const total = useMemo(() => {
    return expensiveCalculation(price, quantity);
}, [price, quantity]);
```

---

# 3️⃣6️⃣ Real-World Data Table Example 📊

Imagine an admin dashboard with:

```text
10,000 employees
```

You want to filter employees:

```text
Department = IT
Salary > ₹50,000
Experience > 5 years
```

That filtering can involve a lot of data.

Conceptually:

```jsx
const filteredEmployees = useMemo(() => {

    return employees.filter(employee =>
        employee.department === "IT" &&
        employee.salary > 50000 &&
        employee.experience > 5
    );

}, [employees]);
```

Now the filtered result can be reused while the dependency remains unchanged.

---

# 3️⃣7️⃣ `useMemo()` Flow

```text
             Component Render
                    ↓
              useMemo()
                    ↓
           Check dependencies
              ↙          ↘
         unchanged      changed
             ↓             ↓
        reuse value     calculate
             ↓             ↓
             └──────┬──────┘
                    ↓
                  result
                    ↓
                    UI
```

---

# 3️⃣8️⃣ Important Difference

### `useMemo`

```text
Memoize a VALUE
```

### `useCallback`

```text
Memoize a FUNCTION reference
```

You will learn `useCallback()` in the next chapter.

Remember:

```text
useMemo
   ↓
value

useCallback
   ↓
function
```

---

# 3️⃣9️⃣ Best Practices ✅

### ✅ Use it for meaningful calculations

```text
Large filtering
Large sorting
Complex calculations
```

### ✅ Keep dependencies correct

```jsx
[products, search]
```

if the calculation uses both.

### ✅ Measure before optimizing

If the application is already fast, don't add unnecessary complexity.

### ✅ Keep the code readable

Performance optimization should not make simple code unnecessarily difficult.

### ✅ Learn `useMemo` together with rendering concepts

Understanding re-renders makes `useMemo()` much easier.

---

# 🧠 4️⃣0️⃣ Simple Real-World Example

Imagine an e-commerce website.

You have:

```text
Products
 ↓
Search
 ↓
Filter
 ↓
Sort
 ↓
Displayed Products
```

A large product list may require processing.

You could calculate:

```jsx
const visibleProducts = useMemo(() => {

    return products
        .filter(product =>
            product.name
                .toLowerCase()
                .includes(search.toLowerCase())
        )
        .sort((a, b) => a.price - b.price);

}, [products, search]);
```

Dependencies:

```text
products
search
```

If neither changes:

```text
Reuse memoized result
```

If `search` changes:

```text
Calculate again
```

---

# 🎤 4️⃣1️⃣ Interview Questions

## Q1. What is `useMemo()`?

`useMemo()` is a React Hook used to memoize a calculated value.

---

## Q2. Why do we use `useMemo()`?

It can help avoid unnecessary recalculation of expensive values when the relevant dependencies have not changed.

---

## Q3. What does `useMemo()` return?

It returns the memoized calculation result.

---

## Q4. What is memoization?

Memoization means storing a calculation result and reusing it when the inputs have not changed.

---

## Q5. Does `useMemo()` stop component re-renders?

No.

It memoizes a value. It does not prevent the entire component from rendering.

---

## Q6. What is the dependency array?

It contains the reactive values that the memoized calculation depends on.

---

## Q7. What happens when a dependency changes?

The calculation is performed again and a new memoized value is produced.

---

## Q8. Should every calculation use `useMemo()`?

No.

Simple calculations generally don't need it.

---

## Q9. What is the difference between `useMemo()` and `useCallback()`?

```text
useMemo
→ memoizes a value

useCallback
→ memoizes a function reference
```

---

## Q10. What is the difference between `useMemo()` and `useEffect()`?

```text
useMemo
→ memoize a calculated value

useEffect
→ perform side effects
```

---

# 🧪 4️⃣2️⃣ Practice Tasks

## Task 1 — Calculate Total

Create:

```text
price = 100
quantity = 5
```

Calculate:

```text
total = 500
```

Use `useMemo()`.

---

## Task 2 — Filter Products

Create:

```text
Laptop
Mouse
Keyboard
Monitor
```

Use `useMemo()` to filter products based on a search term.

---

## Task 3 — Sort Products

Create products with different prices.

Use `useMemo()` to sort them from:

```text
Lowest → Highest
```

---

## Task 4 — Employee Filtering

Create:

```text
name
department
salary
experience
```

Use `useMemo()` to find employees where:

```text
department = IT
salary > 50000
```

---

# 🏆 4️⃣3️⃣ Mini Challenge

Build a small:

# 🛒 Product Search + Filter

Products:

```text
Laptop       ₹50000
Mouse        ₹1000
Keyboard     ₹2000
Monitor      ₹15000
Phone        ₹30000
```

Requirements:

```text
🔎 Search product
💰 Show price
📊 Sort by price
```

Use:

```text
useState
+
useMemo
```

Architecture:

```text
             App
              ↓
        products data
              ↓
          useState
              ↓
            search
              ↓
          useMemo
              ↓
       filtered products
              ↓
              UI
```

### 🎯 Goal

When the search value changes:

```text
search
   ↓
useMemo recalculates
   ↓
filtered products
   ↓
UI
```

---

# 📝 4️⃣4️⃣ 10-Question Quiz

## Q1. What does `useMemo()` memoize?

### ✅ Answer

A calculated **value**.

---

## Q2. What is memoization?

### ✅ Answer

Saving a calculation result so it can be reused when the relevant inputs have not changed.

---

## Q3. Is `useMemo()` mainly a state Hook?

### ❌ Answer

No.

It is a performance optimization Hook for memoizing calculated values.

---

## Q4. What does the dependency array contain?

### ✅ Answer

The values that the calculation depends on.

---

## Q5. If a dependency changes, what happens?

### ✅ Answer

The memoized calculation runs again.

---

## Q6. Does `useMemo()` prevent all component renders?

### ❌ Answer

No.

It memoizes a value; it does not prevent the component itself from rendering.

---

## Q7. Should you use `useMemo()` for `10 + 20`?

### ❌ Answer

Usually no.

That calculation is too simple to need memoization.

---

## Q8. What does `useMemo()` return?

### ✅ Answer

The memoized result of the calculation.

---

## Q9. Which Hook memoizes a function reference?

### ✅ Answer

```jsx
useCallback()
```

---

## Q10. What is the main reason to use `useMemo()`?

### ✅ Answer

To avoid unnecessary recalculation of expensive values when their dependencies have not changed.

---

# 📚 4️⃣5️⃣ Quick Revision

| Concept | Meaning |
|---|---|
| 🧠 Memoization | Remember a calculation result |
| ⚡ `useMemo()` | Memoizes a value |
| 💰 Expensive calculation | Calculation that takes meaningful processing |
| 📦 Dependency | Value used by the calculation |
| 🔄 Dependency changes | Calculation runs again |
| ♻️ Dependency unchanged | Memoized value can be reused |
| 🟢 `useState` | Stores state |
| 🔵 `useMemo` | Memoizes calculated value |
| 🟣 `useCallback` | Memoizes function reference |
| 🟠 `useEffect` | Handles side effects |

---

# 🧠 4️⃣6️⃣ Final Memory Trick

Remember:

> **`useMemo()` = Remember a calculated VALUE.**

```text
Input
  ↓
Expensive Calculation
  ↓
Result
  ↓
useMemo remembers it
  ↓
Same dependencies?
  ↓
YES → Reuse
NO  → Calculate again
```

### ⭐ One-line memory formula

```text
useMemo = expensive value + dependencies + reuse
```

And remember:

```text
useState
   ↓
STATE

useMemo
   ↓
VALUE

useCallback
   ↓
FUNCTION

useEffect
   ↓
SIDE EFFECT
```

---

# ✅ Chapter 16 Checklist

Before moving to the next chapter, make sure you can explain:

- [ ] What is memoization?
- [ ] What is `useMemo()`?
- [ ] Why expensive calculations matter
- [ ] `useMemo()` syntax
- [ ] Dependency array
- [ ] What happens when a dependency changes
- [ ] What happens when dependencies stay unchanged
- [ ] When to use `useMemo()`
- [ ] When NOT to use `useMemo()`
- [ ] `useMemo()` vs `useState()`
- [ ] `useMemo()` vs `useEffect()`
- [ ] `useMemo()` vs `useCallback()`
- [ ] Common dependency mistakes
- [ ] Why state should not be confused with memoized values

---

# 🚀 NEXT CHAPTER

## 🎯 CHAPTER 17 — `useCallback()`

Next we will learn:

```text
🎯 useCallback()
      ↓
Function References
      ↓
Re-render Problem
      ↓
Dependency Array
      ↓
useCallback + React.memo
      ↓
Practical Examples
```

These are exactly the Chapter 17 topics in your React course plan.

Type **`NEXT`** when you are ready for **Chapter 17 — `useCallback()`**.