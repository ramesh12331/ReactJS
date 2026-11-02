
# 🛡️ CHAPTER 39 — ERROR HANDLING

According to your React syllabus, this chapter covers:

- 🧱 Error Boundaries
- 🌐 API Errors
- 📝 Form Errors
- 📡 Network Errors
- 🎨 Error UI
- 🔄 Retry Mechanisms
- 🏭 Production Error Handling

The goal is to learn how to build React applications that **don't simply break when something goes wrong**.

---

# 1. 🧠 What Is Error Handling?

An **error** is a problem that happens while our application is running.

For example:

```text
User opens application
       ↓
API request
       ↓
❌ Server error
       ↓
Application must handle it
```

Instead of showing a broken screen, we should show something useful:

```text
❌ Something went wrong.

Please try again.
```

---

# 2. 🤔 Why Do We Need Error Handling?

Imagine an e-commerce application.

```text
🛒 E-Commerce
│
├── Products
├── Cart
├── Orders
├── Login
└── Payment
```

Many things can fail:

```text
🌐 Internet unavailable
📡 API unavailable
🔐 Authentication failed
📝 Invalid form
💾 Database/server error
💥 Component crashes
```

Without error handling:

```text
❌ White screen
❌ Application crashes
❌ User doesn't know what happened
```

With error handling:

```text
⚠️ Something went wrong.

[ Try Again ]
```

---

# 3. 🌍 Real-World Analogy

Think about a lift.

Normally:

```text
Press button
    ↓
Lift comes
    ↓
Enter
    ↓
Reach floor
```

But what if electricity fails?

A good system doesn't simply do nothing.

It gives:

```text
⚠️ Lift temporarily unavailable
Please use the stairs.
```

React applications should work similarly.

---

# 4. 🧩 Types of Errors in React

There are several important categories:

```text
                ERROR HANDLING
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
 Component         API          Form
   Error           Error         Error
       │             │             │
       ↓             ↓             ↓
Error Boundary   try/catch     Validation
       │
       └─────────────┐
                     ↓
               Network Error
                     ↓
                  Error UI
                     ↓
                Retry
```

Let's learn them one by one.

---

# 5. 🧱 Error Boundaries

An **Error Boundary** is a React mechanism for catching errors that happen during rendering and in certain lifecycle operations in a component tree.

Conceptually:

```text
App
│
├── Header
├── Products
│
└── Dashboard
      │
      ❌ Error
      ↓
Error Boundary
      ↓
⚠️ Error UI
```

Instead of allowing the entire UI to fail, an error boundary can display fallback UI.

---

# 6. 🧠 Simple Analogy

Imagine a building:

```text
Building
│
├── Room A
├── Room B
└── Room C
```

If Room C has a problem, we don't necessarily want the whole building to become unusable.

An error boundary acts like a safety barrier around part of the UI.

```text
┌─────────────────────────────┐
│ Error Boundary              │
│                             │
│   Dashboard                 │
│       ❌                    │
│                             │
│   ⚠️ Dashboard failed       │
└─────────────────────────────┘
```

---

# 7. 🧱 Basic Error Boundary

React error boundaries are traditionally implemented using a class component.

Example:

```jsx
import React from "react";

class ErrorBoundary extends React.Component {

    constructor(props) {

        super(props);

        this.state = {
            hasError: false
        };
    }


    static getDerivedStateFromError(error) {

        return {
            hasError: true
        };
    }


    componentDidCatch(error, info) {

        console.error(
            "Error:",
            error
        );

        console.error(
            "Error Info:",
            info
        );
    }


    render() {

        if (this.state.hasError) {

            return (
                <div>

                    <h2>
                        ⚠️ Something went wrong.
                    </h2>

                    <p>
                        Please try again later.
                    </p>

                </div>
            );
        }


        return this.props.children;
    }
}


export default ErrorBoundary;
```

---

# 8. 🔍 Understanding the Error Boundary

### Step 1

```jsx
class ErrorBoundary extends React.Component
```

We create a class component that acts as our boundary.

---

### Step 2

```jsx
this.state = {
    hasError: false
};
```

Initially:

```text
hasError = false
```

So normal UI is displayed.

---

### Step 3

```jsx
static getDerivedStateFromError(error)
```

When a supported rendering error occurs, React allows the boundary to update its state.

We return:

```jsx
{
    hasError: true
}
```

---

### Step 4

```jsx
componentDidCatch(error, info)
```

This can be used for logging error information.

For example:

```text
console
logging service
monitoring system
```

---

### Step 5

```jsx
if (this.state.hasError)
```

If an error happened:

```text
hasError
   ↓
true
```

Then we display fallback UI.

---

# 9. 🧩 Using Error Boundary

Suppose:

```jsx
function Dashboard() {

    throw new Error(
        "Dashboard failed!"
    );

}
```

Wrap it:

```jsx
function App() {

    return (

        <ErrorBoundary>

            <Dashboard />

        </ErrorBoundary>

    );
}
```

Flow:

```text
App
 ↓
ErrorBoundary
 ↓
Dashboard
 ↓
❌ Error
 ↓
ErrorBoundary catches it
 ↓
⚠️ Fallback UI
```

---

# 10. ⚠️ Important Error Boundary Limitation

Error boundaries do **not** replace normal error handling for everything.

For example, you should not expect an error boundary to handle every:

```text
❌ Event handler error
❌ API failure
❌ Form validation problem
❌ Network request failure
```

Those need their own handling strategies.

This is extremely important.

---

# 11. 🌐 API Errors

Modern React applications frequently communicate with APIs.

Example:

```text
React
  ↓
GET /api/products
  ↓
Backend
  ↓
Database
```

But the request can fail.

Possible reasons:

```text
500 Server Error
404 Not Found
401 Unauthorized
403 Forbidden
Network failure
```

We need to handle these cases.

---

# 12. 🧩 API State Pattern

A very common pattern is:

```text
API STATE
   │
   ├── Loading
   ├── Success
   └── Error
```

In React:

```jsx
const [loading, setLoading] =
    useState(false);

const [data, setData] =
    useState([]);

const [error, setError] =
    useState(null);
```

---

# 13. 🧪 API Example

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
        useState(null);


    useEffect(() => {

        fetch(
            "https://example.com/api/products"
        )
            .then((response) => {

                if (!response.ok) {

                    throw new Error(
                        "Failed to fetch products"
                    );
                }

                return response.json();
            })
            .then((data) => {

                setProducts(data);

                setLoading(false);
            })
            .catch((error) => {

                setError(
                    error.message
                );

                setLoading(false);
            });

    }, []);


    if (loading) {

        return (
            <h2>
                ⏳ Loading products...
            </h2>
        );
    }


    if (error) {

        return (
            <div>

                <h2>
                    ❌ {error}
                </h2>

            </div>
        );
    }


    return (

        <div>

            <h1>
                Products
            </h1>

            {products.map((product) => (

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

# 14. 🔍 API Dry Run

Initially:

```text
loading = true
products = []
error = null
```

So:

```text
⏳ Loading products...
```

appears.

---

### If API succeeds:

```text
API
 ↓
200 OK
 ↓
JSON data
 ↓
setProducts(data)
 ↓
setLoading(false)
```

UI:

```text
Products

Laptop
Phone
Keyboard
```

---

### If API fails:

```text
API
 ↓
500
 ↓
throw Error
 ↓
catch()
 ↓
setError()
 ↓
setLoading(false)
```

UI:

```text
❌ Failed to fetch products
```

---

# 15. 🧠 Better API State Model

Instead of thinking about API calls as just:

```text
success / failure
```

think:

```text
┌─────────────┐
│   Initial   │
└──────┬──────┘
       ↓
   Loading
    ↙   ↘
Success  Error
           ↓
         Retry
           ↓
        Loading
```

This is a very useful mental model for real applications.

---

# 16. 📡 Network Errors

A network error is different from a normal HTTP error.

For example:

```text
Internet disconnected
```

The request may fail before receiving a normal HTTP response.

With `fetch()`:

```jsx
fetch(url)
    .then(...)
    .catch((error) => {
        console.error(error);
    });
```

The `catch()` can handle failures such as network-level problems.

---

# 17. ⚠️ HTTP Error vs Network Error

This distinction is important.

### HTTP error

Server responded:

```text
404
500
401
403
```

Example:

```text
Browser
   ↓
Server
   ↓
500
```

### Network error

The request cannot successfully communicate with the server.

Example:

```text
Browser
   ↓
❌ Internet disconnected
```

With `fetch()`, remember:

> A non-2xx HTTP response does not automatically reject the promise.

That's why we often check:

```jsx
if (!response.ok) {
    throw new Error(
        "Request failed"
    );
}
```

---

# 18. 📝 Form Errors

Forms can have validation errors.

Example:

```text
Name
Email
Password
```

User enters:

```text
Name: Ramesh
Email: abc
Password: 123
```

Possible errors:

```text
❌ Invalid email
❌ Password must be at least 8 characters
```

---

# 19. 🧪 Form Validation Example

```jsx
import { useState } from "react";


function Login() {

    const [email, setEmail] =
        useState("");

    const [password, setPassword] =
        useState("");

    const [error, setError] =
        useState("");


    function handleSubmit(event) {

        event.preventDefault();

        setError("");


        if (!email) {

            setError(
                "Email is required"
            );

            return;
        }


        if (!email.includes("@")) {

            setError(
                "Enter a valid email"
            );

            return;
        }


        if (password.length < 8) {

            setError(
                "Password must be at least 8 characters"
            );

            return;
        }


        console.log(
            "Form submitted"
        );
    }


    return (

        <form
            onSubmit={handleSubmit}
        >

            <input
                value={email}
                onChange={(event) =>
                    setEmail(event.target.value)
                }
                placeholder="Email"
            />


            <input
                type="password"
                value={password}
                onChange={(event) =>
                    setPassword(event.target.value)
                }
                placeholder="Password"
            />


            {error && (
                <p>
                    ❌ {error}
                </p>
            )}


            <button type="submit">
                Login
            </button>

        </form>
    );
}


export default Login;
```

---

# 20. 🧠 Form Error Flow

```text
User submits form
       ↓
Validate
       ↓
Is data valid?
   ↙       ↘
 NO         YES
 ↓           ↓
Show error   Submit
```

Example:

```text
Email = "abc"
       ↓
Invalid
       ↓
❌ Enter a valid email
```

---

# 21. 🎨 Error UI

Error handling is not only about JavaScript.

It is also about **user experience**.

Bad:

```text
Error
```

Better:

```text
❌ Unable to load products.

Please check your internet connection
and try again.

[ Try Again ]
```

---

# 22. 🧩 Reusable Error Component

Create:

```jsx
function ErrorMessage({
    message,
    onRetry
}) {

    return (

        <div>

            <h2>
                ❌ Something went wrong
            </h2>

            <p>
                {message}
            </p>

            {onRetry && (
                <button
                    onClick={onRetry}
                >
                    🔄 Try Again
                </button>
            )}

        </div>
    );
}

export default ErrorMessage;
```

Now different pages can reuse it.

---

# 23. 🔄 Retry Mechanism

Sometimes an error is temporary.

Example:

```text
First request
     ↓
❌ Network problem
```

The user clicks:

```text
🔄 Try Again
```

Then:

```text
Retry
 ↓
API request
 ↓
Success
```

---

# 24. 🧪 Retry Example

```jsx
function Products() {

    const [retry, setRetry] =
        useState(0);


    useEffect(() => {

        fetchProducts();

    }, [retry]);


    function handleRetry() {

        setRetry(
            retry + 1
        );
    }

}
```

The idea is:

```text
retry = 0

User clicks Retry

retry = 1

useEffect dependency changes

       ↓

API request runs again
```

---

# 25. 🧠 Better Retry Pattern

You can also create a reusable function:

```jsx
async function fetchProducts() {

    try {

        setLoading(true);
        setError(null);

        const response =
            await fetch(
                "/api/products"
            );


        if (!response.ok) {

            throw new Error(
                "Unable to load products"
            );
        }


        const data =
            await response.json();

        setProducts(data);

    }
    catch (error) {

        setError(
            error.message
        );

    }
    finally {

        setLoading(false);
    }
}
```

Then:

```jsx
<button
    onClick={fetchProducts}
>
    🔄 Retry
</button>
```

---

# 26. 🧠 `try / catch / finally`

This pattern is very important.

```jsx
try {

    // code that may fail

}
catch (error) {

    // handle error

}
finally {

    // code that should run afterward

}
```

For API requests:

```text
try
 ↓
API request

Success
 ↓
data

Failure
 ↓
catch

Finally
 ↓
stop loading
```

---

# 27. 🏭 Production Error Handling

In development:

```text
console.error(error)
```

is useful.

But production applications need more than that.

A professional application may:

```text
Detect error
    ↓
Show safe UI
    ↓
Log technical information
    ↓
Monitor the error
    ↓
Allow user to retry
```

---

# 28. 🔐 Don't Show Sensitive Information

Never show users internal technical information such as:

```text
Database password
API keys
Stack traces
Internal server details
Sensitive configuration
```

Bad:

```text
❌ PostgreSQL connection failed:
password=secret123
host=internal-server
```

Better:

```text
❌ Something went wrong.

Please try again later.
```

Technical details should be handled through appropriate developer logging/monitoring.

---

# 29. 🧠 Error Handling Architecture

A real React application may look like:

```text
                    React App
                       │
       ┌───────────────┼────────────────┐
       ↓               ↓                ↓
 Component Error    API Error       Form Error
       ↓               ↓                ↓
Error Boundary    try/catch       Validation
       ↓               ↓                ↓
       └───────────────┼────────────────┘
                       ↓
                    Error UI
                       ↓
                    Retry
```

---

# 30. 📊 Loading + Success + Error

This pattern will appear again and again in your React career.

```jsx
if (loading) {
    return <Loader />;
}

if (error) {
    return (
        <ErrorMessage
            message={error}
            onRetry={fetchData}
        />
    );
}

return <ProductList products={data} />;
```

Think:

```text
⏳ Loading
    ↓
❓ Result
 ↙      ↘
❌       ✅
Error   Data
```

---

# 31. ⚠️ Common Mistake #1

Forgetting to check HTTP errors with `fetch()`.

Incorrect:

```jsx
fetch("/api/products")
    .then(response =>
        response.json()
    );
```

A `404` or `500` response does not automatically cause `fetch()` to reject.

Better:

```jsx
fetch("/api/products")
    .then(response => {

        if (!response.ok) {

            throw new Error(
                "Request failed"
            );
        }

        return response.json();
    });
```

---

# 32. ⚠️ Common Mistake #2

Showing only:

```text
Something went wrong.
```

This doesn't help the user much.

Better:

```text
❌ Unable to load products.

[ Try Again ]
```

---

# 33. ⚠️ Common Mistake #3

Not stopping the loading state.

Bad:

```jsx
catch (error) {

    setError(
        error.message
    );

}
```

If you forget:

```jsx
setLoading(false);
```

your UI may remain stuck on:

```text
⏳ Loading...
```

Using `finally` is often useful:

```jsx
finally {

    setLoading(false);
}
```

---

# 34. ⚠️ Common Mistake #4

Putting everything inside one giant component.

Bad:

```text
App.jsx
 ├── API
 ├── Validation
 ├── Error handling
 ├── UI
 ├── Forms
 └── Everything
```

Better:

```text
components/
services/
hooks/
pages/
utils/
```

We will study scalable architecture in **Chapter 40**.

---

# 35. ⚠️ Common Mistake #5

Ignoring retry behavior.

Some failures are temporary.

A good UI can provide:

```text
🔄 Retry
```

when retrying makes sense.

---

# 36. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is error handling?

Error handling means detecting errors and responding to them safely instead of allowing the application to fail unexpectedly.

### Q2. What is an Error Boundary?

An Error Boundary is a React mechanism that catches certain errors in a component tree and displays fallback UI.

### Q3. Why do we need error handling?

To prevent poor user experiences such as crashes, blank screens, and unclear failures.

---

# 37. 🟡 Intermediate

### Q4. What is the difference between API error and component error?

```text
Component error
→ Error during component rendering/lifecycle handling

API error
→ Failure while communicating with a backend service
```

They are handled differently.

### Q5. Does `fetch()` reject automatically for HTTP 404 or 500?

No.

You normally check:

```jsx
if (!response.ok) {
    throw new Error("Request failed");
}
```

### Q6. What is the purpose of `finally`?

It allows code to run after the operation completes, whether it succeeds or fails.

A common example is:

```jsx
finally {
    setLoading(false);
}
```

---

# 38. 🔴 Advanced

### Q7. What errors can Error Boundaries handle?

They are designed to catch certain errors during rendering and related React lifecycle processing in their child tree.

### Q8. Can Error Boundaries replace API error handling?

No.

API requests should still be handled explicitly using appropriate async/error handling.

### Q9. Why shouldn't sensitive error details be shown to users?

Because internal details may expose implementation or security-sensitive information.

### Q10. What are the three common API UI states?

```text
Loading
Success
Error
```

---

# 39. 📝 PRACTICE

## Practice 1 — Error Boundary

Create:

```text
ErrorBoundary.jsx
```

Then create a component that intentionally throws an error.

Wrap it with the Error Boundary.

---

## Practice 2 — API Error

Create a product API request.

Handle:

```text
⏳ Loading
✅ Success
❌ Error
```

---

## Practice 3 — Retry

Add:

```text
🔄 Try Again
```

When the API fails.

---

## Practice 4 — Form Validation

Create a registration form:

```text
Name
Email
Password
Confirm Password
```

Display validation errors.

---

# 40. 🧩 MINI CHALLENGE

Build:

# 🛒 Product API Error Handling

Create:

```text
src/
│
├── components/
│   ├── Loader.jsx
│   ├── ErrorMessage.jsx
│   └── ProductList.jsx
│
├── services/
│   └── productService.js
│
├── pages/
│   └── Products.jsx
│
└── App.jsx
```

Requirements:

### 1️⃣ Loading

Display:

```text
⏳ Loading products...
```

### 2️⃣ Success

Display:

```text
Product List
```

### 3️⃣ API Error

Display:

```text
❌ Unable to load products.
```

### 4️⃣ Retry

Add:

```text
🔄 Try Again
```

### 5️⃣ Empty Data

If the API returns no products:

```text
📦 No products found.
```

Your final flow should be:

```text
              API Request
                   │
          ┌────────┼────────┐
          ↓        ↓        ↓
       Loading   Success   Error
          │        │        │
          ↓        ↓        ↓
       Loader    List     Error UI
                            │
                            ↓
                          Retry
                            │
                            ↓
                       API Request
```

---

# 41. 🧠 COMPLETE DRY RUN

Suppose the application starts.

Initial state:

```text
loading = true
data = []
error = null
```

UI:

```text
⏳ Loading products...
```

API request runs.

### Scenario A — Success

```text
200 OK
 ↓
JSON
 ↓
setData(products)
 ↓
setLoading(false)
```

Output:

```text
Products

Laptop
Phone
Keyboard
```

---

### Scenario B — Server Error

```text
500
 ↓
response.ok = false
 ↓
throw Error
 ↓
catch
 ↓
setError(...)
 ↓
setLoading(false)
```

Output:

```text
❌ Unable to load products.

[ 🔄 Try Again ]
```

---

### Scenario C — User retries

```text
Click Retry
     ↓
fetchProducts()
     ↓
Loading
     ↓
API request
```

If successful:

```text
✅ Products displayed
```

---

# 42. 📝 10-QUESTION QUIZ

### Q1. What is an Error Boundary?

**Answer:** A React mechanism for catching certain errors in a component tree and displaying fallback UI.

---

### Q2. Does an Error Boundary handle every possible React error?

**Answer:** No. API errors, form validation errors, and many event-handler errors need their own handling.

---

### Q3. Does `fetch()` automatically reject for HTTP 500?

**Answer:** No.

---

### Q4. How can we detect an unsuccessful HTTP response?

**Answer:**

```jsx
if (!response.ok) {
    throw new Error("Request failed");
}
```

---

### Q5. What are the three common API states?

**Answer:**

```text
Loading
Success
Error
```

---

### Q6. What is the purpose of `catch()`?

**Answer:** To handle errors/rejected promises in the promise chain.

---

### Q7. What is the purpose of `finally()`?

**Answer:** To execute cleanup/finalization logic regardless of success or failure.

---

### Q8. Why do we need retry mechanisms?

**Answer:** Some failures are temporary, such as transient network or server problems.

---

### Q9. Should we show stack traces to normal users?

**Answer:** No. Show safe, user-friendly error information.

---

### Q10. What is the most important API error-handling pattern?

**Answer:**

```text
Loading
   ↓
Success OR Error
   ↓
User-friendly UI
   ↓
Retry when appropriate
```

---

# 43. 🧠 FINAL MEMORY TRICK

Remember these four:

```text
🧱 COMPONENT ERROR
       ↓
Error Boundary


🌐 API ERROR
       ↓
try/catch + response.ok


📝 FORM ERROR
       ↓
Validation


📡 NETWORK ERROR
       ↓
catch + Retry
```

And remember:

```text
⏳ Loading
   ↓
┌───────┴───────┐
↓               ↓
✅ Success      ❌ Error
                  ↓
              🔄 Retry
```

### ⭐ One-Line Memory

> **Detect the error → show useful UI → log safely → retry when appropriate.**

---

# ✅ CHAPTER 39 COMPLETE

You learned:

- 🧱 Error Boundaries
- 🌐 API Errors
- 📝 Form Errors
- 📡 Network Errors
- 🎨 Error UI
- 🔄 Retry mechanisms
- `try/catch/finally`
- `fetch()` + `response.ok`
- Loading/Success/Error states
- Production error-handling concepts
- Common mistakes
- Interview questions
- Practice exercises
- Mini challenge

Your course source specifically lists these seven Chapter 39 topics: Error Boundaries, API errors, form errors, network errors, error UI, retry mechanisms, and production error handling.

---

# 🚀 NEXT CHAPTER

## 🏛️ Chapter 40 — Advanced React Architecture

We will learn:

```text
🏗️ Feature-based structure
🧩 Component-based structure
🌐 Service layer
📡 API layer
🪝 Hooks folder
🌍 Context
🗃️ Store
🛠️ Utilities
📌 Constants
🔐 Environment variables
📁 Scalable folder structure
```

Type **NEXT** to continue.