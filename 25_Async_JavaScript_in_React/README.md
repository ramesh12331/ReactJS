# ⏳ CHAPTER 25 — ASYNC JAVASCRIPT IN REACT

## 🎯 Goal of This Chapter

In the previous chapter, we learned:

```text
React
 ↓
API
 ↓
Backend
 ↓
Response
 ↓
React State
 ↓
UI
```

But there is an important question:

### ❓ Why doesn't the API response come immediately?

Because API requests take time.

For example:

```text
React
  ↓
Request
  ↓
⏳ Waiting...
  ↓
Backend
  ↓
Database
  ↓
Response
```

JavaScript needs a way to handle this waiting.

That is where **asynchronous JavaScript** comes in.

---

# 📚 CHAPTER TOPICS

We will learn:

1. What is synchronous JavaScript?
2. What is asynchronous JavaScript?
3. Promise
4. Promise states
5. `async`
6. `await`
7. `try`
8. `catch`
9. Error handling
10. Sequential requests
11. Parallel requests
12. `Promise.all()`
13. API loading patterns
14. Async JavaScript in React
15. Practical examples
16. Dry runs
17. Common mistakes
18. Best practices
19. Interview questions
20. Practice tasks
21. Mini challenge
22. Quiz
23. Final revision

---

# 1️⃣ WHAT IS SYNCHRONOUS JAVASCRIPT?

Synchronous means:

> **One task finishes before the next task starts.**

Example:

```javascript
console.log("A");

console.log("B");

console.log("C");
```

Output:

```text
A
B
C
```

Flow:

```text
A
↓
B
↓
C
```

JavaScript executes the statements in order.

---

# 2️⃣ SIMPLE REAL-LIFE ANALOGY 🍵

Imagine you are making tea.

```text
Boil water
   ↓
Wait
   ↓
Add tea powder
   ↓
Wait
   ↓
Add milk
```

You don't normally add the milk before starting the process.

One step follows another.

This is similar to synchronous execution.

---

# 3️⃣ WHAT IS ASYNCHRONOUS JAVASCRIPT?

Asynchronous means:

> JavaScript can start a task that takes time and continue doing other work while waiting for that task to finish.

Example:

```javascript
console.log("Start");

setTimeout(() => {
    console.log("API response");
}, 2000);

console.log("End");
```

Output:

```text
Start
End
API response
```

Why?

Because the timer is asynchronous.

Flow:

```text
Start
 ↓
Start timer
 ↓
Continue JavaScript
 ↓
End
 ↓
⏳ Wait
 ↓
API response
```

---

# 4️⃣ WHY IS THIS IMPORTANT IN REACT?

React applications frequently perform operations that take time:

```text
API requests
Database requests through backend
Timers
File operations
User authentication
Image loading
```

For example:

```text
React
 ↓
GET /products
 ↓
⏳ Waiting
 ↓
Backend
 ↓
Database
 ↓
Response
```

React shouldn't freeze while waiting.

So we use asynchronous JavaScript.

---

# 5️⃣ WHAT IS A PROMISE? 🤝

A **Promise** is an object representing the eventual result of an asynchronous operation.

Simple meaning:

> "I don't have the result right now, but I promise to give you the result later."

Example:

```javascript
const result = fetch("/products");
```

`fetch()` returns a Promise.

It does not immediately give us the final API data.

---

# 6️⃣ REAL-LIFE PROMISE ANALOGY 📦

Imagine you order a product online.

```text
You place order
      ↓
Order processing
      ↓
Shipping
      ↓
Delivery
```

When you place the order, you don't receive the product immediately.

You receive a promise that the result will come later.

Similarly:

```text
fetch()
   ↓
Promise
   ↓
Waiting
   ↓
Response
```

---

# 7️⃣ PROMISE STATES

A Promise has three main states:

```text
             Promise
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
   Pending   Fulfilled  Rejected
```

## 1. Pending

The operation is still running.

```text
⏳ Waiting...
```

---

## 2. Fulfilled

The operation completed successfully.

```text
✅ Success
```

---

## 3. Rejected

The operation failed.

```text
❌ Error
```

---

# 8️⃣ PROMISE EXAMPLE

```javascript
const promise = fetch(
    "https://example.com/products"
);
```

Initially:

```text
Pending
```

If successful:

```text
Fulfilled
```

If it fails:

```text
Rejected
```

---

# 9️⃣ USING `.then()`

Promises can be handled using `.then()`.

Example:

```javascript
fetch("/products")
    .then(response => {
        return response.json();
    })
    .then(data => {
        console.log(data);
    });
```

Flow:

```text
fetch()
 ↓
Promise
 ↓
response
 ↓
response.json()
 ↓
data
```

---

# 🔟 `.catch()` FOR ERRORS

We can handle errors using:

```javascript
.catch()
```

Example:

```javascript
fetch("/products")
    .then(response => {
        return response.json();
    })
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.log(error);
    });
```

Flow:

```text
              fetch()
                 ↓
             Promise
              /    \
             /      \
        Success     Failure
           ↓           ↓
         .then()    .catch()
```

---

# 1️⃣1️⃣ WHY `async/await`?

`.then()` works.

But when there are many asynchronous operations, code can become harder to read.

Example:

```javascript
fetch("/products")
    .then(response => response.json())
    .then(data => {
        return fetch(`/products/${data[0].id}`);
    })
    .then(response => response.json())
    .then(product => {
        console.log(product);
    });
```

It can become difficult for beginners.

`async/await` makes asynchronous code look more like normal step-by-step code.

---

# 1️⃣2️⃣ WHAT IS `async`?

`async` is used before a function.

Example:

```javascript
async function getProducts() {

}
```

An `async` function always returns a Promise.

Example:

```javascript
async function hello() {
    return "Hello";
}
```

Calling:

```javascript
const result = hello();

console.log(result);
```

gives a Promise, not directly the string.

---

# 1️⃣3️⃣ WHAT IS `await`?

`await` tells JavaScript:

> Wait for this Promise to settle before continuing this async function.

Example:

```javascript
const response = await fetch("/products");
```

Think:

```text
fetch()
  ↓
⏳ wait
  ↓
response
```

---

# 1️⃣4️⃣ BASIC `async/await`

```javascript
async function getProducts() {

    const response =
        await fetch("/products");

    const data =
        await response.json();

    console.log(data);
}
```

This is much easier to read.

---

# 1️⃣5️⃣ LINE-BY-LINE

### Line 1

```javascript
async function getProducts() {
```

Creates an asynchronous function.

---

### Line 2

```javascript
const response =
    await fetch("/products");
```

Send the request and wait for the response.

---

### Line 3

```javascript
const data =
    await response.json();
```

Convert the response into JSON data.

---

### Line 4

```javascript
console.log(data);
```

Display the result.

---

# 1️⃣6️⃣ IMPORTANT RULE ⚠️

`await` is normally used inside an `async` function.

Example:

```javascript
async function getData() {

    const response =
        await fetch("/products");

}
```

Don't think of:

```javascript
await fetch(...)
```

as a normal statement that can be placed anywhere.

---

# 1️⃣7️⃣ `try/catch`

When using `async/await`, we commonly use:

```text
try
catch
```

for error handling.

Basic syntax:

```javascript
try {

    // code that may fail

} catch (error) {

    // handle error

}
```

---

# 1️⃣8️⃣ BASIC EXAMPLE

```javascript
async function getProducts() {

    try {

        const response =
            await fetch("/products");

        const data =
            await response.json();

        console.log(data);

    } catch (error) {

        console.log(error);

    }
}
```

Flow:

```text
try
 ↓
API request
 ↓
Success?
 ├── YES → continue
 │
 └── NO → catch
```

---

# 1️⃣9️⃣ WHY `try/catch`?

Suppose:

```text
Backend is down
```

or:

```text
Network disconnected
```

or:

```text
Request failed
```

Without proper handling, your application may not give the user a useful message.

With:

```javascript
catch (error)
```

we can show:

```text
❌ Failed to load products
```

---

# 2️⃣0️⃣ `response.ok` + `try/catch`

Remember something important from Chapter 24.

`fetch()` does not automatically reject for every HTTP error status.

So:

```javascript
async function getProducts() {

    try {

        const response =
            await fetch("/products");

        if (!response.ok) {
            throw new Error(
                "Failed to fetch products"
            );
        }

        const data =
            await response.json();

        return data;

    } catch (error) {

        console.log(error);

    }
}
```

This gives us clearer error handling.

---

# 2️⃣1️⃣ COMPLETE REACT EXAMPLE

```jsx
import {
    useEffect,
    useState
} from "react";

function Products() {

    const [products, setProducts] =
        useState([]);

    const [loading, setLoading] =
        useState(true);

    const [error, setError] =
        useState("");

    async function getProducts() {

        try {

            const response =
                await fetch(
                    "http://localhost:8000/products"
                );

            if (!response.ok) {
                throw new Error(
                    "Failed to fetch products"
                );
            }

            const data =
                await response.json();

            setProducts(data);

        } catch (error) {

            setError(error.message);

        } finally {

            setLoading(false);

        }
    }

    useEffect(() => {

        getProducts();

    }, []);

    if (loading) {
        return <h2>Loading...</h2>;
    }

    if (error) {
        return <h2>{error}</h2>;
    }

    return (
        <div>

            <h1>Products</h1>

            {products.map(product => (
                <p key={product.id}>
                    {product.name}
                </p>
            ))}

        </div>
    );
}

export default Products;
```

---

# 2️⃣2️⃣ UNDERSTAND THE FLOW 🧠

```text
Component renders
      ↓
useEffect()
      ↓
getProducts()
      ↓
async function
      ↓
await fetch()
      ↓
⏳ Wait for response
      ↓
response received
      ↓
await response.json()
      ↓
setProducts(data)
      ↓
React re-renders
      ↓
Products displayed
```

---

# 2️⃣3️⃣ WHAT DOES `finally` DO?

`finally` runs after the `try`/`catch` process whether the operation succeeds or fails.

Example:

```javascript
try {

    // API

} catch (error) {

    // error

} finally {

    setLoading(false);

}
```

Why useful?

Because loading should stop in both cases.

```text
Success
   ↓
loading = false

Error
   ↓
loading = false
```

---

# 2️⃣4️⃣ API LOADING PATTERN

A common React pattern is:

```text
           API Request
                │
                ↓
        loading = true
                │
                ↓
          ┌─────┴─────┐
          ↓           ↓
       Success      Error
          ↓           ↓
       data        error
          │           │
          └─────┬─────┘
                ↓
        loading = false
```

State:

```jsx
const [data, setData] = useState([]);
const [loading, setLoading] = useState(false);
const [error, setError] = useState("");
```

This pattern is extremely useful in real applications.

---

# 2️⃣5️⃣ SEQUENTIAL REQUESTS 🔢

Sequential means:

> Request 2 starts only after Request 1 finishes.

Example:

```text
Request 1
   ↓
Wait
   ↓
Request 2
   ↓
Wait
   ↓
Request 3
```

---

# 2️⃣6️⃣ SEQUENTIAL EXAMPLE

Suppose:

```text
First:
Get user

Second:
Get orders for that user
```

Code:

```javascript
async function loadUserOrders() {

    const userResponse =
        await fetch("/user");

    const user =
        await userResponse.json();

    const ordersResponse =
        await fetch(
            `/orders/${user.id}`
        );

    const orders =
        await ordersResponse.json();

    console.log(user);
    console.log(orders);
}
```

Flow:

```text
GET /user
    ↓
Wait
    ↓
User received
    ↓
GET /orders/1
    ↓
Wait
    ↓
Orders received
```

---

# 2️⃣7️⃣ WHEN DO WE NEED SEQUENTIAL REQUESTS?

When the second request depends on the first result.

Example:

```text
Get user
   ↓
Need user.id
   ↓
Get user's orders
```

The second request cannot start correctly until we know the user ID.

---

# 2️⃣8️⃣ PARALLEL REQUESTS ⚡

Parallel means:

> Start multiple independent requests without waiting for one to finish before starting the others.

Example:

```text
Request A ────────────────→
Request B ───────────→
Request C ─────────────→
```

They can run at the same time.

---

# 2️⃣9️⃣ WHY PARALLEL REQUESTS?

Suppose a dashboard needs:

```text
Users
Products
Orders
```

These requests are independent.

We don't need:

```text
Users
 ↓
Products
 ↓
Orders
```

Instead:

```text
Users ───────→
Products ────→
Orders ──────→
```

This can reduce total waiting time.

---

# 3️⃣0️⃣ `Promise.all()`

JavaScript provides:

```javascript
Promise.all()
```

to wait for multiple Promises together.

Example:

```javascript
const [
    usersResponse,
    productsResponse,
    ordersResponse
] = await Promise.all([
    fetch("/users"),
    fetch("/products"),
    fetch("/orders")
]);
```

The requests are started together.

---

# 3️⃣1️⃣ COMPLETE PARALLEL EXAMPLE

```javascript
async function loadDashboard() {

    try {

        const [
            usersResponse,
            productsResponse,
            ordersResponse
        ] = await Promise.all([

            fetch("/users"),

            fetch("/products"),

            fetch("/orders")

        ]);

        const users =
            await usersResponse.json();

        const products =
            await productsResponse.json();

        const orders =
            await ordersResponse.json();

        console.log(users);
        console.log(products);
        console.log(orders);

    } catch (error) {

        console.log(error);

    }
}
```

---

# 3️⃣2️⃣ SEQUENTIAL vs PARALLEL

## Sequential

```text
Request A
   ↓
Wait
   ↓
Request B
   ↓
Wait
   ↓
Request C
```

Use when:

```text
B depends on A
C depends on B
```

---

## Parallel

```text
Request A ──────→
Request B ──────→
Request C ──────→
```

Use when:

```text
A independent
B independent
C independent
```

---

# 3️⃣3️⃣ EASY REAL-LIFE EXAMPLE 🍕

### Sequential

You need to:

```text
Get customer ID
      ↓
Get customer's orders
```

Second task depends on first.

Use sequential.

---

### Parallel

You need:

```text
Customers
Products
Categories
```

None depends on another.

Use parallel.

---

# 3️⃣4️⃣ PROMISE.ALL DRY RUN

Suppose:

```text
/users      → 1 second
/products   → 2 seconds
/orders     → 3 seconds
```

Sequential approach:

```text
1 + 2 + 3
= 6 seconds approximately
```

Parallel approach:

```text
1 second ──────→
2 seconds ─────→
3 seconds ─────→

approximately 3 seconds
```

The exact timing depends on the environment, but the key idea is that independent requests can overlap.

---

# 3️⃣5️⃣ IMPORTANT `Promise.all()` BEHAVIOR ⚠️

If one Promise rejects:

```javascript
await Promise.all([
    fetch("/users"),
    fetch("/products"),
    fetch("/orders")
]);
```

the `Promise.all()` result rejects.

Therefore:

```javascript
try {

    const results =
        await Promise.all([...]);

} catch (error) {

    console.log(error);

}
```

is a common pattern.

---

# 3️⃣6️⃣ REACT DASHBOARD EXAMPLE

Imagine:

```text
Dashboard
│
├── Total Users
├── Total Products
└── Total Orders
```

We can load them together:

```jsx
useEffect(() => {

    async function loadDashboard() {

        try {

            const [
                usersResponse,
                productsResponse,
                ordersResponse
            ] = await Promise.all([
                fetch("/users"),
                fetch("/products"),
                fetch("/orders")
            ]);

            const users =
                await usersResponse.json();

            const products =
                await productsResponse.json();

            const orders =
                await ordersResponse.json();

            console.log(users);
            console.log(products);
            console.log(orders);

        } catch (error) {

            console.log(error);

        }

    }

    loadDashboard();

}, []);
```

---

# 3️⃣7️⃣ ASYNC FUNCTION IN EVENT HANDLER

Async JavaScript is not only for `useEffect()`.

It can also be used for button actions.

Example:

```jsx
async function handleDelete(id) {

    try {

        const response =
            await fetch(
                `/products/${id}`,
                {
                    method: "DELETE"
                }
            );

        if (!response.ok) {
            throw new Error(
                "Delete failed"
            );
        }

        console.log(
            "Product deleted"
        );

    } catch (error) {

        console.log(error);

    }
}
```

Button:

```jsx
<button
    onClick={() => handleDelete(product.id)}
>
    Delete
</button>
```

---

# 3️⃣8️⃣ ASYNC FORM SUBMISSION

Example:

```jsx
async function handleSubmit(event) {

    event.preventDefault();

    try {

        const response =
            await fetch("/products", {
                method: "POST",

                headers: {
                    "Content-Type":
                        "application/json"
                },

                body: JSON.stringify({
                    name: "Laptop",
                    price: 50000
                })
            });

        if (!response.ok) {
            throw new Error(
                "Failed to create product"
            );
        }

        const data =
            await response.json();

        console.log(data);

    } catch (error) {

        console.log(error);

    }
}
```

---

# 3️⃣9️⃣ VERY IMPORTANT: DON'T MAKE THE EFFECT CALLBACK ASYNC DIRECTLY

Beginners often write:

```jsx
useEffect(async () => {

    const response =
        await fetch("/products");

}, []);
```

Avoid this pattern.

Instead:

```jsx
useEffect(() => {

    async function loadProducts() {

        const response =
            await fetch("/products");

        const data =
            await response.json();

        setProducts(data);
    }

    loadProducts();

}, []);
```

Why?

Because the function passed directly to `useEffect` is expected to either return nothing or a cleanup function, rather than a Promise.

---

# 4️⃣0️⃣ COMMON MISTAKE — FORGETTING `await`

Wrong:

```javascript
const data =
    response.json();
```

`response.json()` returns a Promise.

Correct:

```javascript
const data =
    await response.json();
```

---

# 4️⃣1️⃣ COMMON MISTAKE — FORGETTING `async`

Wrong:

```javascript
function getProducts() {

    const response =
        await fetch("/products");
}
```

Correct:

```javascript
async function getProducts() {

    const response =
        await fetch("/products");
}
```

---

# 4️⃣2️⃣ COMMON MISTAKE — WRONG ASSUMPTION ABOUT `fetch()`

Don't think:

```javascript
const data = fetch("/products");
```

means:

```text
data = actual products
```

Actually:

```text
fetch()
 ↓
Promise
 ↓
Response
 ↓
response.json()
 ↓
Actual data
```

---

# 4️⃣3️⃣ COMMON MISTAKE — NO ERROR HANDLING

Avoid:

```javascript
async function getProducts() {

    const response =
        await fetch("/products");

    const data =
        await response.json();

    setProducts(data);
}
```

Better:

```javascript
async function getProducts() {

    try {

        const response =
            await fetch("/products");

        if (!response.ok) {
            throw new Error(
                "Failed to fetch products"
            );
        }

        const data =
            await response.json();

        setProducts(data);

    } catch (error) {

        setError(error.message);

    }
}
```

---

# 4️⃣4️⃣ COMMON MISTAKE — USING SEQUENTIAL REQUESTS UNNECESSARILY

Suppose:

```text
Users
Products
Orders
```

are independent.

Don't unnecessarily do:

```javascript
const users =
    await fetch("/users");

const products =
    await fetch("/products");

const orders =
    await fetch("/orders");
```

You can often use:

```javascript
await Promise.all([
    fetch("/users"),
    fetch("/products"),
    fetch("/orders")
]);
```

---

# 4️⃣5️⃣ BUT DON'T USE PARALLEL WHEN DATA DEPENDS ON DATA

Example:

```text
Get user
 ↓
Get user.id
 ↓
Get orders/user.id
```

This should remain sequential.

```javascript
const userResponse =
    await fetch("/user");

const user =
    await userResponse.json();

const ordersResponse =
    await fetch(`/orders/${user.id}`);
```

---

# 4️⃣6️⃣ API LOADING PATTERN — PRACTICAL

A good basic structure:

```jsx
const [data, setData] =
    useState([]);

const [loading, setLoading] =
    useState(false);

const [error, setError] =
    useState("");
```

Function:

```jsx
async function loadData() {

    try {

        setLoading(true);
        setError("");

        const response =
            await fetch("/products");

        if (!response.ok) {
            throw new Error(
                "Failed to fetch products"
            );
        }

        const data =
            await response.json();

        setData(data);

    } catch (error) {

        setError(error.message);

    } finally {

        setLoading(false);

    }
}
```

This pattern is worth remembering.

---

# 4️⃣7️⃣ COMPLETE MENTAL MODEL 🧠

When you see:

```javascript
async function loadData() {

    try {

        const response =
            await fetch("/products");

        const data =
            await response.json();

        setData(data);

    } catch (error) {

        setError(error.message);

    } finally {

        setLoading(false);

    }
}
```

Think:

```text
async
 ↓
"This function handles asynchronous work"

await fetch()
 ↓
"Wait for API response"

await response.json()
 ↓
"Wait for JSON conversion"

try
 ↓
"Try the operation"

catch
 ↓
"Handle failure"

finally
 ↓
"Run cleanup"

setData()
 ↓
"Update React state"
```

---

# 4️⃣8️⃣ PROMISE vs ASYNC/AWAIT

### Promise syntax

```javascript
fetch("/products")
    .then(response =>
        response.json()
    )
    .then(data => {
        console.log(data);
    })
    .catch(error => {
        console.log(error);
    });
```

### Async/await syntax

```javascript
try {

    const response =
        await fetch("/products");

    const data =
        await response.json();

    console.log(data);

} catch (error) {

    console.log(error);

}
```

Both work with Promises.

`async/await` is generally easier to read for multi-step asynchronous code.

---

# 4️⃣9️⃣ REAL-WORLD REACT FLOW 🚀

A typical application:

```text
User opens Products page
          ↓
React component renders
          ↓
useEffect()
          ↓
loadProducts()
          ↓
async function
          ↓
await fetch()
          ↓
⏳ API request
          ↓
Response
          ↓
await response.json()
          ↓
setProducts()
          ↓
React re-render
          ↓
Products displayed
```

If something fails:

```text
API
 ↓
Error
 ↓
catch
 ↓
setError()
 ↓
Error message
```

---

# 5️⃣0️⃣ INTERVIEW QUESTIONS 🎤

## Q1. What is asynchronous JavaScript?

**Answer:**

Asynchronous JavaScript allows operations that take time, such as API requests, to be handled without blocking the rest of the JavaScript execution.

---

## Q2. What is a Promise?

**Answer:**

A Promise represents the eventual result of an asynchronous operation.

---

## Q3. What are the states of a Promise?

**Answer:**

```text
Pending
Fulfilled
Rejected
```

---

## Q4. What does `async` do?

**Answer:**

It declares an asynchronous function. An async function returns a Promise.

---

## Q5. What does `await` do?

**Answer:**

It waits for a Promise to settle inside an async function before continuing that function.

---

## Q6. What is `try/catch` used for?

**Answer:**

It is commonly used to handle errors in asynchronous operations.

---

## Q7. What is `Promise.all()`?

**Answer:**

It allows multiple Promises to be awaited together and is useful when the asynchronous operations are independent.

---

## Q8. What is a sequential request?

**Answer:**

A request that starts after a previous request has completed, usually because the second operation depends on the first.

---

## Q9. What is a parallel request?

**Answer:**

Multiple independent asynchronous operations are started without waiting for one to finish before starting the others.

---

## Q10. Why should we not make the `useEffect` callback itself `async`?

**Answer:**

Because `useEffect` expects the callback to return nothing or a cleanup function, while an async function returns a Promise.

---

# 5️⃣1️⃣ PRACTICE TASKS 🧪

## Task 1 — Promise

Create a Promise:

```javascript
const promise = new Promise(
    (resolve, reject) => {

        resolve("Success");

    }
);
```

Handle it using `.then()`.

---

## Task 2 — Async/Await

Create:

```javascript
async function getData() {

}
```

Use:

```javascript
await
```

inside it.

---

## Task 3 — Error Handling

Create an API function using:

```text
try
catch
finally
```

---

## Task 4 — React API

Create:

```text
Products.jsx
```

and implement:

```text
GET
Loading
Error
Success
```

---

## Task 5 — Sequential

Implement:

```text
Get user
 ↓
Get user's orders
```

---

## Task 6 — Parallel

Implement:

```text
Get users
Get products
Get orders
```

using:

```javascript
Promise.all()
```

---

# 5️⃣2️⃣ MINI CHALLENGE 🚀

Build a:

# 📊 DASHBOARD DATA LOADER

Your dashboard needs:

```text
Users
Products
Orders
```

### Requirements

```text
1. Load all three APIs
2. Show Loading...
3. Show Error if something fails
4. Display the three results
5. Use async/await
6. Use try/catch
7. Use Promise.all()
```

Architecture:

```text
Dashboard
   ↓
loadDashboard()
   ↓
Promise.all()
   ├── /users
   ├── /products
   └── /orders
   ↓
setState()
   ↓
Dashboard UI
```

---

# 5️⃣3️⃣ QUIZ 🧠

### Q1. What does asynchronous mean?

**Answer:**  
An operation can continue independently while JavaScript handles other work instead of waiting synchronously for it to finish.

---

### Q2. What does `fetch()` return?

**Answer:**  
A Promise.

---

### Q3. What are the three Promise states?

**Answer:**

```text
Pending
Fulfilled
Rejected
```

---

### Q4. Which keyword declares an asynchronous function?

**Answer:**

```javascript
async
```

---

### Q5. Which keyword waits for a Promise?

**Answer:**

```javascript
await
```

---

### Q6. Which block handles errors?

**Answer:**

```javascript
catch
```

---

### Q7. Which block runs after `try`/`catch` regardless of success or failure?

**Answer:**

```javascript
finally
```

---

### Q8. Which method is useful for independent requests?

**Answer:**

```javascript
Promise.all()
```

---

### Q9. If Request B needs the result of Request A, should they normally be parallel?

**Answer:**  
No. They normally need to be handled sequentially.

---

### Q10. What is the common React API pattern?

**Answer:**

```text
useEffect
   ↓
async function
   ↓
await fetch()
   ↓
await response.json()
   ↓
setState()
   ↓
UI
```

---

# 🧠 FINAL MEMORY TRICK

Remember these **7 words**:

```text
PROMISE
   ↓
ASYNC
   ↓
AWAIT
   ↓
TRY
   ↓
CATCH
   ↓
FINALLY
   ↓
STATE
```

And remember:

```text
                ASYNC API
                    │
                    ↓
                 Promise
                    │
          ┌─────────┴─────────┐
          ↓                   ↓
       Success              Failure
          ↓                   ↓
       await               catch
          ↓                   ↓
      setState()          setError()
          │                   │
          └─────────┬─────────┘
                    ↓
                   UI
```

### ⭐ Most important difference

```text
Sequential:
A → B → C

Parallel:
A ─────→
B ─────→
C ─────→
```

**Sequential** → use when requests depend on each other.

**Parallel** → use when requests are independent.

---

# ✅ CHAPTER 25 CHECKLIST

Before moving ahead, you should understand:

- [ ] Synchronous JavaScript
- [ ] Asynchronous JavaScript
- [ ] Promise
- [ ] Pending
- [ ] Fulfilled
- [ ] Rejected
- [ ] `async`
- [ ] `await`
- [ ] `try`
- [ ] `catch`
- [ ] `finally`
- [ ] Error handling
- [ ] Sequential requests
- [ ] Parallel requests
- [ ] `Promise.all()`
- [ ] React API loading pattern

---

# 🎯 CHAPTER 25 COMPLETE

The next chapter starts **Forms and Real Applications**, beginning with:

# 📝 CHAPTER 26 — ADVANCED FORMS

We will learn:

```text
Form Architecture
       ↓
Validation
       ↓
Client-side Validation
       ↓
Error Messages
       ↓
Required Fields
       ↓
Email Validation
       ↓
Password Validation
       ↓
React Hook Form
       ↓
Controlled vs Uncontrolled Forms
```

**Type `NEXT` to start Chapter 26.**