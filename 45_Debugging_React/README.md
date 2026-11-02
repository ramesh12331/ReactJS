# 🐞 CHAPTER 45 — DEBUGGING REACT

## 🎯 Chapter Goal

In this chapter, you will learn how to find and fix React problems instead of randomly changing code.

We will learn:

- 🛠️ Browser DevTools
- 🖥️ Console
- 🌐 Network tab
- ⚛️ React DevTools
- 🔄 Debugging state
- 📦 Debugging props
- 🌐 API debugging
- ⚠️ Common React errors
- ⬜ White screen
- 🔐 CORS
- ❌ 404 errors
- 💥 500 errors
- 🔄 Infinite rendering

---

# 1. 🧠 What Is Debugging?

**Debugging** means finding and fixing problems in your code.

For example:

```jsx
function App() {
    const name = "Ramesh";

    return <h1>Hello {username}</h1>;
}
```

We created:

```text
name
```

But we used:

```text
username
```

JavaScript will report an error.

Debugging means:

```text
Problem
   ↓
Find the error
   ↓
Understand the reason
   ↓
Fix the code
   ↓
Test again
```

---

# 2. 🤔 Why Is Debugging Important?

When building real React applications, errors are normal.

For example:

```text
Button doesn't work
API doesn't return data
Page is blank
Form doesn't submit
State doesn't update
Component doesn't render
Images don't load
Routing doesn't work
```

A good developer doesn't panic.

They follow a process.

```text
❌ Don't randomly change code

✅ Find the problem
      ↓
   Read error
      ↓
   Find source
      ↓
   Understand
      ↓
   Fix
```

---

# 3. 🧰 Main Debugging Tools

Your most important tools are:

```text
🖥️ Browser DevTools
       │
       ├── Console
       ├── Network
       ├── Elements
       └── Sources

⚛️ React DevTools
       │
       ├── Components
       └── Profiler
```

---

# 4. 🖥️ Browser DevTools

In Chrome:

```text
Right Click
    ↓
Inspect
```

Or press:

```text
F12
```

or:

```text
Ctrl + Shift + I
```

You will see:

```text
Elements
Console
Sources
Network
Application
...
```

For React debugging, the most important are:

```text
Console
Network
Elements
Sources
```

---

# 5. 🖥️ Console

The Console displays JavaScript errors, warnings and messages.

Example:

```javascript
console.log("Hello");
```

Output:

```text
Hello
```

---

# 6. 🔍 Using `console.log()`

Suppose:

```jsx
function App() {

    const name = "Ramesh";

    console.log(name);

    return <h1>Hello {name}</h1>;
}
```

Console:

```text
Ramesh
```

This is one of the simplest debugging techniques.

---

# 7. 🧠 Debug State

Suppose:

```jsx
import {
    useState
} from "react";


function Counter() {

    const [count, setCount] =
        useState(0);


    function increment() {

        setCount(count + 1);
    }


    console.log(
        "count:",
        count
    );


    return (
        <div>

            <h1>{count}</h1>

            <button
                onClick={increment}
            >
                +
            </button>

        </div>
    );
}
```

Initially:

```text
count: 0
```

Click:

```text
+
```

Then:

```text
count: 1
```

Again:

```text
count: 2
```

---

# 8. 🔄 Important Idea — State Update Causes Render

Remember:

```text
setCount()
   ↓
State changes
   ↓
Component renders again
   ↓
New UI
```

Therefore:

```javascript
console.log(count);
```

inside the component can show different values across renders.

---

# 9. 📦 Debug Props

Suppose:

```jsx
function User({ name, age }) {

    console.log(
        "name:",
        name
    );

    console.log(
        "age:",
        age
    );


    return (
        <div>

            <h2>{name}</h2>

            <p>{age}</p>

        </div>
    );
}
```

Parent:

```jsx
function App() {

    return (
        <User
            name="Ramesh"
            age={25}
        />
    );
}
```

Console:

```text
name: Ramesh
age: 25
```

This helps us verify whether the parent is sending the expected props.

---

# 10. 🧪 Debugging Objects

For objects:

```javascript
const user = {
    name: "Ramesh",
    age: 25,
    city: "Hyderabad"
};

console.log(user);
```

Better:

```javascript
console.log(
    "User:",
    user
);
```

You can expand the object inside the browser console.

---

# 11. 🌐 Network Tab

The **Network** tab is extremely important when working with APIs.

Suppose React calls:

```text
GET /students
```

Open:

```text
DevTools
   ↓
Network
```

You may see:

```text
students
products
users
login
```

Click the request.

You can inspect:

```text
Headers
Payload
Preview
Response
Status
Timing
```

---

# 12. 🔎 Network Debugging

Suppose you have:

```javascript
fetch(
    "http://localhost:8000/students"
);
```

But no data appears.

Don't immediately change React code.

First check:

```text
Network
   ↓
students
   ↓
Status
```

Maybe you find:

```text
404
```

Now you know the request reached a server, but the requested resource/path wasn't found.

---

# 13. 📊 Common HTTP Status Codes

Remember these:

| Status | Meaning |
|---|---|
| 200 | Success |
| 201 | Created |
| 400 | Bad Request |
| 401 | Unauthorized |
| 403 | Forbidden |
| 404 | Not Found |
| 500 | Server Error |

---

# 14. ❌ 404 Error

Suppose React calls:

```text
GET /student
```

But FastAPI provides:

```text
GET /students
```

Difference:

```text
/student
/students
```

The API request may return:

```text
404 Not Found
```

### Debugging process

```text
React URL
   ↓
Network tab
   ↓
Request URL
   ↓
Compare backend route
```

---

# 15. 💥 500 Error

A `500` error usually means something went wrong on the server.

Example:

```text
React
  ↓
GET /students
  ↓
FastAPI
  ↓
Database error
  ↓
500
```

Important:

**A 500 error is usually a backend/server problem, not simply a React rendering problem.**

Check:

```text
FastAPI terminal
Backend logs
Database
API request
```

---

# 16. 🔐 CORS Error

This is very common in React + FastAPI projects.

For example:

```text
React
http://localhost:5173

FastAPI
http://localhost:8000
```

The browser may block a request if the backend's CORS configuration doesn't allow the frontend origin.

You may see a message related to:

```text
Access-Control-Allow-Origin
```

### Debugging flow

```text
React
   ↓
fetch()
   ↓
Browser
   ↓
CORS check
   ↓
FastAPI
```

If CORS configuration is incorrect:

```text
❌ Browser blocks request
```

---

# 17. 🧠 How to Debug CORS

Check the backend.

For FastAPI:

```python
from fastapi.middleware.cors import (
    CORSMiddleware
)


app.add_middleware(
    CORSMiddleware,

    allow_origins=[
        "http://localhost:5173"
    ],

    allow_credentials=True,

    allow_methods=["*"],

    allow_headers=["*"]
)
```

Then restart the backend if necessary.

---

# 18. ⬜ White Screen

One of the most frightening beginner problems:

```text
Browser
   ↓
⬜ Completely blank page
```

Don't panic.

Usually start with the Console.

```text
Blank page
    ↓
Open Console
    ↓
Read first error
    ↓
Find file + line
    ↓
Fix
```

---

# 19. 🐛 Example White Screen Error

Suppose:

```jsx
function App() {

    return (
        <div>

            <User />

        </div>
    );
}
```

But `User` was never imported.

You may get an error.

Check:

```text
Console
```

It might point you toward the component/file causing the problem.

---

# 20. 🔍 Read Errors Carefully

Suppose you see:

```text
ReferenceError:
User is not defined
```

Don't just search:

```text
"React is broken"
```

Read the important part:

```text
User is not defined
```

Then ask:

```text
Did I define User?
Did I import User?
Did I spell User correctly?
```

---

# 21. 🧠 Error Reading Method

When you see an error, identify:

```text
1. Error type
2. Error message
3. File
4. Line number
5. Code causing it
```

For example:

```text
ReferenceError
     ↓
User is not defined
     ↓
App.jsx
     ↓
Line 15
```

This is much better than guessing.

---

# 22. ⚠️ Common React Errors

### Error 1

```text
X is not defined
```

Possible reason:

```text
Variable doesn't exist
Import missing
Typo
```

---

### Error 2

```text
Cannot read properties of undefined
```

Example:

```jsx
user.name
```

but:

```javascript
user === undefined
```

---

### Error 3

```text
Objects are not valid as a React child
```

You may be trying to render an object directly.

Bad:

```jsx
<p>{user}</p>
```

Better:

```jsx
<p>{user.name}</p>
```

---

# 23. 🔄 Debugging `useEffect`

Suppose:

```jsx
useEffect(() => {

    console.log(
        "Effect executed"
    );

}, []);
```

This helps us know whether the effect is running.

If you expect it to run but don't see the log, investigate:

```text
Component rendered?
   ↓
Effect mounted?
   ↓
Dependency array?
   ↓
Condition?
```

---

# 24. 🔄 Infinite Rendering

A very common mistake:

```jsx
function App() {

    const [count, setCount] =
        useState(0);


    setCount(count + 1);


    return <h1>{count}</h1>;
}
```

Why is this dangerous?

```text
Render
 ↓
setCount()
 ↓
State changes
 ↓
Render
 ↓
setCount()
 ↓
State changes
 ↓
Render
 ↓
...
```

This creates an endless update loop.

---

# 25. ❌ Another Infinite Loop

Be careful with:

```jsx
useEffect(() => {

    setCount(count + 1);

}, [count]);
```

Flow:

```text
count changes
   ↓
effect runs
   ↓
setCount()
   ↓
count changes
   ↓
effect runs
   ↓
...
```

This can create an infinite update cycle.

---

# 26. 🧠 Debugging Infinite Rendering

Ask:

```text
Where is state changing?

↓

What causes that state update?

↓

Does that update cause another render?

↓

Does the render trigger another update?
```

Remember:

```text
Render
 ↓
State update
 ↓
Render
```

If there is no stopping condition, investigate immediately.

---

# 27. ⚛️ React DevTools

React DevTools is a browser extension/tool used to inspect React applications.

It provides:

```text
Components
Profiler
```

The Components panel is especially useful for understanding:

```text
Component tree
Props
State
```

---

# 28. 🌳 Component Tree

Suppose your application contains:

```text
App
├── Header
├── Navbar
├── ProductList
│   ├── ProductCard
│   ├── ProductCard
│   └── ProductCard
└── Footer
```

React DevTools can help you inspect this component structure.

---

# 29. 📦 Debug Props With React DevTools

Suppose:

```jsx
<ProductCard
    name="Laptop"
    price={50000}
/>
```

In React DevTools, you can inspect the component and see its props.

Conceptually:

```text
ProductCard
 ├── name → Laptop
 └── price → 50000
```

This is often easier than adding many `console.log()` statements.

---

# 30. 🔄 Debug State With React DevTools

Suppose:

```jsx
const [count, setCount] =
    useState(0);
```

React DevTools can help you inspect the component's state while the application is running.

This helps answer:

```text
Is state correct?
Did state update?
Which component owns the state?
```

---

# 31. 🧪 Debugging API Data

Suppose:

```jsx
useEffect(() => {

    fetch(
        "http://localhost:8000/products"
    )
        .then(response =>
            response.json()
        )
        .then(data =>
            setProducts(data)
        );

}, []);
```

If products aren't displayed, check in this order:

```text
1️⃣ Is component rendering?
       ↓
2️⃣ Is useEffect running?
       ↓
3️⃣ Is request sent?
       ↓
4️⃣ What is HTTP status?
       ↓
5️⃣ What is response?
       ↓
6️⃣ Is response JSON correct?
       ↓
7️⃣ Is setProducts() called?
       ↓
8️⃣ Is map() using correct property?
```

This is a professional debugging approach.

---

# 32. 🧠 Example API Debugging

Suppose backend returns:

```json
[
    {
        "id": 1,
        "name": "Laptop"
    }
]
```

But React uses:

```jsx
{products.map(
    product => (
        <p>
            {product.title}
        </p>
    )
)}
```

Problem:

Backend:

```text
name
```

React expects:

```text
title
```

Result:

```text
undefined
```

The API itself may be working perfectly.

The problem is a **data-shape mismatch**.

---

# 33. 🔎 Debug With Logs

You can temporarily use:

```jsx
console.log(
    "API data:",
    products
);
```

Then inspect the actual response.

Don't guess the response structure.

**Look at the data.**

---

# 34. 🌐 Network Debugging Checklist

When an API doesn't work:

```text
☐ Network tab open?
☐ Request sent?
☐ Correct URL?
☐ Correct HTTP method?
☐ Correct status code?
☐ Request headers correct?
☐ Request body correct?
☐ Response correct?
☐ CORS problem?
☐ Backend running?
```

---

# 35. 🐍 React + FastAPI Debugging

For your full-stack projects, use:

```text
                React
                  ↓
          Browser Console
                  ↓
            Network Tab
                  ↓
               FastAPI
                  ↓
          FastAPI Terminal
                  ↓
             PostgreSQL
```

If the request fails, determine **which layer failed**.

---

# 36. 🧩 Debugging Layers

### Layer 1 — UI

```text
Does the component render?
```

### Layer 2 — State

```text
Is state correct?
```

### Layer 3 — Event

```text
Did the click/change/submit event execute?
```

### Layer 4 — API

```text
Was the request sent?
```

### Layer 5 — Backend

```text
Did FastAPI receive it?
```

### Layer 6 — Database

```text
Did the database operation succeed?
```

This prevents random debugging.

---

# 37. 🧠 Professional Debugging Formula

Use this:

```text
UI
 ↓
State
 ↓
Event
 ↓
API
 ↓
Backend
 ↓
Database
```

Find the **first layer where things become wrong**.

That is usually where you should start fixing.

---

# 38. 🛠️ Use Breakpoints

The browser Sources panel allows you to pause JavaScript execution.

For example:

```javascript
function calculateTotal() {

    const price = 100;

    const quantity = 3;

    const total =
        price * quantity;

    return total;
}
```

You can place a breakpoint and inspect:

```text
price
quantity
total
```

This is more powerful than using many logs.

---

# 39. 🧪 `debugger`

JavaScript provides:

```javascript
debugger;
```

Example:

```jsx
function handleClick() {

    debugger;

    console.log("Button clicked");
}
```

When DevTools is open, execution can pause at that point.

Then you can inspect variables.

---

# 40. ⚠️ Don't Leave Debug Code Everywhere

During development:

```javascript
console.log(data);
```

is useful.

But before production, clean unnecessary debugging statements.

Especially remove:

```text
Passwords
Tokens
API keys
Private information
Sensitive data
```

Never log secrets.

---

# 41. 🧭 Debugging Strategy

When something doesn't work:

### Step 1

Reproduce the problem.

```text
What exactly is broken?
```

### Step 2

Read the Console.

```text
Is there a JavaScript error?
```

### Step 3

Check the Network tab.

```text
Did the API request work?
```

### Step 4

Check React DevTools.

```text
Are props/state correct?
```

### Step 5

Check backend.

```text
Did FastAPI receive the request?
```

### Step 6

Check database.

```text
Did the query succeed?
```

### Step 7

Fix the root cause.

---

# 42. 🧪 Practical Example

Imagine:

```text
Product page
```

shows:

```text
No products
```

Don't immediately rewrite the component.

Check:

```text
Product component
      ↓
products state
      ↓
useEffect
      ↓
fetch()
      ↓
Network
      ↓
API response
```

Suppose Network shows:

```text
GET /products
500
```

Now React is probably not the root problem.

Check:

```text
FastAPI terminal
```

Maybe the database query failed.

---

# 43. 🧠 Important Debugging Mindset

Don't say:

> "React is not working."

Instead say:

> "The request returned 500."

or:

> "The state contains an empty array."

or:

> "The component received `undefined` for the `user` prop."

Be specific.

---

# 44. ❌ COMMON MISTAKES

## Mistake 1 — Ignoring Console

Bad approach:

```text
Blank page
 ↓
Change random code
 ↓
Try again
```

Better:

```text
Blank page
 ↓
Console
 ↓
Read error
 ↓
Find source
```

---

## Mistake 2 — Only Checking React

Sometimes the problem is:

```text
FastAPI
Database
CORS
Network
```

not React.

---

## Mistake 3 — Assuming API Worked

Always inspect:

```text
Network → Status → Response
```

---

## Mistake 4 — Ignoring the Error Line

If the browser says:

```text
App.jsx:25
```

look at line 25 first.

---

## Mistake 5 — Logging Too Much

Don't put hundreds of logs everywhere.

Use focused logs:

```javascript
console.log(
    "Products API response:",
    data
);
```

---

# 45. ✅ BEST PRACTICES

### 1. Read the error first

Don't guess.

### 2. Reproduce consistently

Know exactly how to trigger the problem.

### 3. Debug one layer at a time

```text
UI
→ State
→ Event
→ API
→ Backend
→ Database
```

### 4. Use meaningful logs

```javascript
console.log(
    "User:",
    user
);
```

is better than:

```javascript
console.log("x");
```

### 5. Use Network for API issues

Don't debug API requests only through React code.

### 6. Use React DevTools for components

Inspect:

```text
Props
State
Component tree
```

### 7. Fix root causes

Don't hide errors with random conditions.

---

# 46. 💼 INTERVIEW QUESTIONS

## Beginner

### Q1. What is debugging?

Debugging is the process of finding and fixing problems in software.

### Q2. What is `console.log()` used for?

To inspect values and understand what the code is doing.

### Q3. What is Browser DevTools?

A set of browser tools used to inspect and debug web applications.

### Q4. What is the Network tab?

It allows developers to inspect HTTP requests and responses.

---

# 47. 💼 Intermediate Questions

### Q5. How do you debug an API request in React?

Check:

```text
Network
 ↓
URL
 ↓
Method
 ↓
Status
 ↓
Request
 ↓
Response
```

### Q6. What causes a 404?

Usually the requested resource or route cannot be found.

### Q7. What does a 500 error indicate?

A server-side error occurred.

### Q8. How do you debug state?

Use:

```text
console.log()
```

and React DevTools.

---

# 48. 💼 Advanced Questions

### Q9. How would you debug a React + FastAPI application?

I would isolate the layers:

```text
React UI
 ↓
State/Event
 ↓
Network request
 ↓
FastAPI
 ↓
Database
```

Then identify the first layer where the expected behavior fails.

### Q10. How would you investigate an infinite render loop?

I would look for state updates occurring during rendering or effects whose dependency changes because of the update they themselves trigger.

---

# 49. 📝 PRACTICE QUESTIONS

## Practice 1

Create:

```jsx
const [count, setCount] =
    useState(0);
```

Log count whenever the component renders.

---

## Practice 2

Create a `User` component and debug its props.

---

## Practice 3

Fetch products from your FastAPI backend.

Inspect:

```text
Network
```

and identify:

```text
URL
Status
Response
```

---

## Practice 4

Create an intentional 404 request.

Find the error in Network.

---

## Practice 5

Create a button that throws an error.

Use the Console to identify it.

---

## Practice 6

Create an intentional state update loop.

Then identify why it happens.

---

# 50. 🧩 MINI CHALLENGE

Build:

# 🛒 Debuggable Product Dashboard

Requirements:

```text
Product Dashboard
       │
       ├── Product List
       ├── Loading State
       ├── Error State
       └── Retry Button
```

API:

```text
GET /products
```

When something goes wrong, your application should display:

```text
Loading...
```

or:

```text
❌ Failed to load products
```

or:

```text
🔄 Retry
```

Use:

```text
React
+
FastAPI
```

Then practice debugging:

```text
404
500
CORS
empty response
wrong property name
```

---

# 51. 🔥 REAL-WORLD DEBUGGING EXAMPLE

Suppose your React page shows:

```text
❌ Failed to load products
```

Your debugging process should be:

```text
STEP 1
Open Console
      ↓
No JavaScript error
      ↓

STEP 2
Open Network
      ↓
GET /products
      ↓

STEP 3
Status = 500
      ↓

STEP 4
Open FastAPI terminal
      ↓
Database error
      ↓

STEP 5
Fix backend/database problem
      ↓

STEP 6
Refresh React
      ↓
Products displayed
```

This is real developer debugging.

---

# 52. 🧠 DEBUGGING CHEAT SHEET

```text
⬜ Blank page
   ↓
Console

❌ API not working
   ↓
Network

404
   ↓
Check URL / route

500
   ↓
Check backend

CORS
   ↓
Check backend CORS configuration

Wrong UI data
   ↓
Check state + API response

Wrong component data
   ↓
Check props

Infinite rendering
   ↓
Check state updates + effects

Slow application
   ↓
React DevTools / Profiler
```

---

# 53. 📝 10-QUESTION QUIZ

### Q1. What is debugging?

**Answer:** Finding and fixing problems in code.

---

### Q2. Which browser tool shows JavaScript errors?

**Answer:** Console.

---

### Q3. Which DevTools tab is used for API requests?

**Answer:** Network.

---

### Q4. What does HTTP 404 usually mean?

**Answer:** The requested resource/route was not found.

---

### Q5. What does HTTP 500 usually mean?

**Answer:** A server-side error.

---

### Q6. What can React DevTools help inspect?

**Answer:** React components, props and state.

---

### Q7. What can cause an infinite rendering loop?

**Answer:** Repeated state updates that continually trigger renders, such as an unconditional state update during render or an effect that repeatedly updates one of its own dependencies.

---

### Q8. Where should you look when a React + FastAPI request returns 500?

**Answer:** Check the Network request and then inspect the FastAPI/backend logs.

---

### Q9. What is `debugger;` used for?

**Answer:** It can pause JavaScript execution when DevTools is open so you can inspect the program state.

---

### Q10. Complete the debugging flow:

```text
React
 ↓
?
 ↓
?
 ↓
FastAPI
 ↓
?
```

**Answer:**

```text
React
 ↓
Browser Network
 ↓
HTTP Request
 ↓
FastAPI
 ↓
Database
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
🐞 DEBUGGING

Console
   ↓
JavaScript errors

Network
   ↓
API errors

React DevTools
   ↓
Props + State

FastAPI Terminal
   ↓
Backend errors

PostgreSQL
   ↓
Database errors
```

And:

```text
404 → Where is the route?
500 → What broke on the server?
CORS → Is the browser allowed to call the API?
White screen → Check Console first
Wrong data → Check State / Props / Response
Infinite render → Check State updates
```

---

# ✅ CHAPTER 45 COMPLETE

You learned:

- 🐞 Debugging
- 🖥️ Browser DevTools
- 🖥️ Console
- 🌐 Network tab
- ⚛️ React DevTools
- 🔄 State debugging
- 📦 Props debugging
- 🌐 API debugging
- ⬜ White-screen debugging
- 🔐 CORS debugging
- ❌ 404 debugging
- 💥 500 debugging
- 🔄 Infinite-render debugging
- 🐍 React + FastAPI debugging
- 🧩 Breakpoints
- `debugger`
- 💼 Interview questions
- 📝 Practice
- 🧩 Mini challenge

## 🧠 One rule to remember

> **Don't guess where the bug is. Find the first place where the data or behavior becomes wrong.**

# 🚀 NEXT CHAPTER — CHAPTER 46

## 🧪 React Testing

We will learn:

- Why testing?
- Unit testing
- Component testing
- Integration testing
- Vitest
- React Testing Library
- User interactions
- Mocking API calls
- Practical tests
- Interview questions
- Testing practice project

Type **NEXT** to continue.