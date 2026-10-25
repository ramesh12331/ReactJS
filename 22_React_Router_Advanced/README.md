# 🚀 CHAPTER 22 — REACT ROUTER ADVANCED

In Chapter 21, we learned the basic routing concepts.

Now we will move to **advanced React Router**.

---

# 📚 What We Will Learn

1. `createBrowserRouter`
2. `RouterProvider`
3. Layout Routes
4. `Outlet`
5. Nested Routing
6. Route Loaders
7. Route Errors
8. Authentication Routes
9. Role-Based Routes
10. Practical Project
11. Dry Run
12. Common Mistakes
13. Best Practices
14. Interview Questions
15. Practice Tasks
16. Mini Challenge
17. Quiz
18. Final Revision

---

# 1️⃣ What is Advanced Routing?

Basic routing looks like:

```text
URL
 ↓
Route
 ↓
Component
```

Advanced routing allows us to build a more structured application:

```text
Application
     ↓
Router
     ↓
Layout
     ↓
Nested Routes
     ↓
Authentication
     ↓
Role-based Access
```

For example, a real application may have:

```text
🏠 Home

📊 Dashboard
   ├── Overview
   ├── Users
   ├── Products
   └── Reports

⚙️ Settings
   ├── Profile
   └── Security
```

We don't want to repeat the same sidebar and navbar on every page.

That's where **layout routes and nested routes** become useful.

---

# 2️⃣ `createBrowserRouter`

Instead of defining routing entirely with JSX like:

```jsx
<BrowserRouter>
    <Routes>
        <Route />
    </Routes>
</BrowserRouter>
```

React Router also provides a data-router approach using:

```jsx
createBrowserRouter()
```

Example:

```jsx
import {
    createBrowserRouter
} from "react-router-dom";
```

Then:

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />
    }
]);
```

Think:

```text
createBrowserRouter()
        ↓
Creates Router Configuration
```

---

# 3️⃣ Router Configuration

A router can be represented as an array of route objects.

Example:

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />
    },
    {
        path: "/about",
        element: <About />
    }
]);
```

Notice the difference.

### Basic style

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

### Data-router style

```jsx
{
    path: "/about",
    element: <About />
}
```

The second approach represents routes as **JavaScript objects**.

---

# 4️⃣ `RouterProvider`

After creating the router:

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />
    }
]);
```

we provide it to React using:

```jsx
<RouterProvider router={router} />
```

Import:

```jsx
import {
    RouterProvider
} from "react-router-dom";
```

---

# 5️⃣ Complete Basic Example

## main.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import {
    createBrowserRouter,
    RouterProvider
} from "react-router-dom";

import App from "./App";
import About from "./About";

const router = createBrowserRouter([
    {
        path: "/",
        element: <App />
    },
    {
        path: "/about",
        element: <About />
    }
]);

ReactDOM.createRoot(
    document.getElementById("root")
).render(
    <RouterProvider router={router} />
);
```

---

# 6️⃣ Understand the Flow

```text
main.jsx
   ↓
createBrowserRouter()
   ↓
Router configuration
   ↓
RouterProvider
   ↓
URL
   ↓
Matching route
   ↓
Component
```

For:

```text
/about
```

React checks:

```text
/
 /about ← MATCH
```

Then:

```text
About component
```

is displayed.

---

# 7️⃣ What is a Layout Route?

Imagine an admin application.

Every page contains:

```text
--------------------------------
Navbar
--------------------------------
Sidebar | Page Content
--------------------------------
Footer
--------------------------------
```

Pages:

```text
Dashboard
Users
Products
Reports
```

The navbar and sidebar are common.

We don't want:

```text
Dashboard.jsx
 → Navbar
 → Sidebar

Users.jsx
 → Navbar
 → Sidebar

Products.jsx
 → Navbar
 → Sidebar
```

This duplicates code.

Instead:

```text
AdminLayout
    ↓
    ├── Dashboard
    ├── Users
    ├── Products
    └── Reports
```

This is the idea of a **layout route**.

---

# 8️⃣ What is `Outlet`?

`Outlet` is one of the most important concepts in nested routing.

Think of `Outlet` as:

> **A placeholder where the child route will appear.**

Example:

```jsx
import { Outlet } from "react-router-dom";

function Layout() {
    return (
        <div>

            <h1>My Website</h1>

            <Outlet />

        </div>
    );
}

export default Layout;
```

Here:

```jsx
<Outlet />
```

means:

```text
"Put the matching child route here."
```

---

# 9️⃣ Real-Life Analogy for Outlet 🏠

Imagine a house:

```text
🏠 Main House

-------------------------
Header
-------------------------

        [ ROOM ]

-------------------------
Footer
-------------------------
```

The `[ ROOM ]` is the `Outlet`.

Different rooms can appear there.

```text
Outlet
 ↓
Home

or

Outlet
 ↓
Products

or

Outlet
 ↓
Contact
```

---

# 🔟 Layout + Outlet

Create:

```jsx
function Layout() {
    return (
        <div>

            <header>
                <h1>My Website</h1>
            </header>

            <main>
                <Outlet />
            </main>

            <footer>
                <p>Copyright 2026</p>
            </footer>

        </div>
    );
}
```

Now the layout is shared.

---

# 1️⃣1️⃣ Nested Routing

Suppose we have:

```text
/dashboard
```

and inside dashboard:

```text
/dashboard
/dashboard/users
/dashboard/products
/dashboard/reports
```

Structure:

```text
Dashboard
   │
   ├── Overview
   ├── Users
   ├── Products
   └── Reports
```

This is **nested routing**.

---

# 1️⃣2️⃣ Nested Route Configuration

Example:

```jsx
const router = createBrowserRouter([
    {
        path: "/dashboard",
        element: <DashboardLayout />,
        children: [

            {
                index: true,
                element: <DashboardHome />
            },

            {
                path: "users",
                element: <Users />
            },

            {
                path: "products",
                element: <Products />
            },

            {
                path: "reports",
                element: <Reports />
            }

        ]
    }
]);
```

Notice:

```jsx
children: [...]
```

This means:

```text
/dashboard
      ↓
children
```

---

# 1️⃣3️⃣ Important — Child Paths

Inside:

```jsx
path: "/dashboard"
```

we write:

```jsx
path: "users"
```

not:

```jsx
path: "/dashboard/users"
```

Why?

Because it is already a child of:

```text
/dashboard
```

So React Router combines them:

```text
/dashboard
+
users
=
/dashboard/users
```

---

# 1️⃣4️⃣ Dashboard Layout

```jsx
import { Outlet } from "react-router-dom";

function DashboardLayout() {
    return (
        <div>

            <h1>Dashboard</h1>

            <nav>
                Dashboard Menu
            </nav>

            <hr />

            <Outlet />

        </div>
    );
}

export default DashboardLayout;
```

Now the child page appears inside:

```jsx
<Outlet />
```

---

# 1️⃣5️⃣ Complete Nested Example

```jsx
const router = createBrowserRouter([
    {
        path: "/dashboard",
        element: <DashboardLayout />,

        children: [

            {
                index: true,
                element: <DashboardHome />
            },

            {
                path: "users",
                element: <Users />
            },

            {
                path: "products",
                element: <Products />
            }

        ]
    }
]);
```

Flow:

```text
/dashboard
    ↓
DashboardLayout
    ↓
Outlet
    ↓
DashboardHome
```

For:

```text
/dashboard/users
```

Flow:

```text
/dashboard/users
       ↓
DashboardLayout
       ↓
Outlet
       ↓
Users
```

---

# 1️⃣6️⃣ What is `index: true`?

Consider:

```text
/dashboard
```

We want a default child page.

We can write:

```jsx
{
    index: true,
    element: <DashboardHome />
}
```

This means:

```text
/dashboard
    ↓
DashboardHome
```

Think:

```text
index = default child route
```

---

# 1️⃣7️⃣ Route Loaders

A route can also have a loader.

A loader can be used to obtain data needed by a route before rendering it.

Example:

```jsx
const router = createBrowserRouter([
    {
        path: "/products",
        element: <Products />,
        loader: loadProducts
    }
]);
```

Function:

```jsx
async function loadProducts() {

    const response = await fetch(
        "https://example.com/products"
    );

    return response.json();
}
```

Concept:

```text
Request Route
     ↓
Loader
     ↓
Fetch Data
     ↓
Route Component
```

---

# 1️⃣8️⃣ Reading Loader Data

React Router provides:

```jsx
useLoaderData()
```

Example:

```jsx
import { useLoaderData } from "react-router-dom";

function Products() {

    const products = useLoaderData();

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
```

The important idea is:

```text
loader()
   ↓
data
   ↓
useLoaderData()
   ↓
component
```

---

# 1️⃣9️⃣ Route Errors

What happens if something goes wrong?

For example:

```text
Invalid route
API error
Loader error
```

We can define an error element.

Example:

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />,
        errorElement: <ErrorPage />
    }
]);
```

Now:

```text
Routing problem
      ↓
ErrorPage
```

---

# 2️⃣0️⃣ Error Page

```jsx
function ErrorPage() {
    return (
        <div>

            <h1>Oops! 😕</h1>

            <p>
                Something went wrong.
            </p>

        </div>
    );
}

export default ErrorPage;
```

---

# 2️⃣1️⃣ 404 Not Found Page

A common real-world requirement is:

```text
User enters:

/something-that-does-not-exist
```

We want:

```text
404
Page Not Found
```

A catch-all route can be configured with:

```jsx
{
    path: "*",
    element: <NotFound />
}
```

Example:

```jsx
const router = createBrowserRouter([
    {
        path: "/",
        element: <Home />
    },

    {
        path: "/about",
        element: <About />
    },

    {
        path: "*",
        element: <NotFound />
    }
]);
```

---

# 2️⃣2️⃣ Authentication Routes 🔐

Real applications often have:

```text
Login
Register
Dashboard
Profile
Settings
```

Suppose the user is not logged in.

We don't want them to access:

```text
/dashboard
```

So we need a protected route concept.

---

# 2️⃣3️⃣ Authentication Flow

```text
User
 ↓
/dashboard
 ↓
Is user authenticated?
 ├── YES → Dashboard
 │
 └── NO → Login
```

This is called a **protected/authentication route**.

---

# 2️⃣4️⃣ Simple Protected Route

Example:

```jsx
function ProtectedRoute({ children }) {

    const isLoggedIn = true;

    if (!isLoggedIn) {
        return <Navigate to="/login" />;
    }

    return children;
}
```

Import:

```jsx
import {
    Navigate
} from "react-router-dom";
```

Then:

```jsx
<Route
    path="/dashboard"
    element={
        <ProtectedRoute>
            <Dashboard />
        </ProtectedRoute>
    }
/>
```

### Important

The above is a simple learning example.

In a real application, authentication state would normally come from something such as an authentication context, state store, or server/session mechanism.

---

# 2️⃣5️⃣ Role-Based Routes 👨‍💼

Authentication asks:

> "Is the user logged in?"

Authorization asks:

> "What is this user allowed to access?"

Example:

```text
Admin
User
Manager
```

Suppose:

```text
/admin
```

should only be accessible by admins.

Flow:

```text
User
 ↓
Logged in?
 ↓
What role?
 ↓
Admin?
 ├── YES → Admin Dashboard
 └── NO  → Access Denied
```

---

# 2️⃣6️⃣ Simple Role-Based Example

```jsx
function AdminRoute({ children }) {

    const role = "admin";

    if (role !== "admin") {
        return <h1>Access Denied</h1>;
    }

    return children;
}
```

Usage:

```jsx
<Route
    path="/admin"
    element={
        <AdminRoute>
            <AdminDashboard />
        </AdminRoute>
    }
/>
```

---

# 2️⃣7️⃣ Authentication vs Authorization

Very important interview concept:

```text
Authentication
     ↓
Who are you?

Authorization
     ↓
What are you allowed to access?
```

Example:

```text
Login
 ↓
Authentication

Admin permission
 ↓
Authorization
```

---

# 2️⃣8️⃣ Practical Project 🛠️

Let's design an Admin application.

```text
Admin Application
│
├── Login
│
└── Dashboard
     │
     ├── Overview
     ├── Users
     ├── Products
     └── Reports
```

Layout:

```text
DashboardLayout
│
├── Navbar
├── Sidebar
│
└── Outlet
       ↓
   Current Page
```

---

# 2️⃣9️⃣ Project Structure

```text
src/
│
├── layouts/
│   └── DashboardLayout.jsx
│
├── pages/
│   ├── Login.jsx
│   ├── Dashboard.jsx
│   ├── Users.jsx
│   ├── Products.jsx
│   ├── Reports.jsx
│   └── NotFound.jsx
│
├── router/
│   └── router.jsx
│
├── App.jsx
└── main.jsx
```

---

# 3️⃣0️⃣ Router Configuration

```jsx
import {
    createBrowserRouter
} from "react-router-dom";

import DashboardLayout
    from "../layouts/DashboardLayout";

import Login from "../pages/Login";
import Dashboard from "../pages/Dashboard";
import Users from "../pages/Users";
import Products from "../pages/Products";
import Reports from "../pages/Reports";
import NotFound from "../pages/NotFound";

const router = createBrowserRouter([
    {
        path: "/login",
        element: <Login />
    },

    {
        path: "/dashboard",
        element: <DashboardLayout />,

        children: [

            {
                index: true,
                element: <Dashboard />
            },

            {
                path: "users",
                element: <Users />
            },

            {
                path: "products",
                element: <Products />
            },

            {
                path: "reports",
                element: <Reports />
            }

        ]
    },

    {
        path: "*",
        element: <NotFound />
    }
]);

export default router;
```

---

# 3️⃣1️⃣ main.jsx

```jsx
import React from "react";
import ReactDOM from "react-dom/client";

import {
    RouterProvider
} from "react-router-dom";

import router from "./router/router";

ReactDOM.createRoot(
    document.getElementById("root")
).render(
    <RouterProvider router={router} />
);
```

---

# 3️⃣2️⃣ DashboardLayout

```jsx
import {
    Link,
    Outlet
} from "react-router-dom";

function DashboardLayout() {

    return (
        <div>

            <h1>Admin Dashboard</h1>

            <nav>

                <Link to="/dashboard">
                    Overview
                </Link>

                {" | "}

                <Link to="/dashboard/users">
                    Users
                </Link>

                {" | "}

                <Link to="/dashboard/products">
                    Products
                </Link>

                {" | "}

                <Link to="/dashboard/reports">
                    Reports
                </Link>

            </nav>

            <hr />

            <Outlet />

        </div>
    );
}

export default DashboardLayout;
```

---

# 3️⃣3️⃣ Dry Run 🧠

Suppose the user visits:

```text
/dashboard/users
```

### Step 1

Router receives:

```text
/dashboard/users
```

### Step 2

It matches:

```text
/dashboard
```

### Step 3

It finds child:

```text
users
```

### Step 4

It renders:

```text
DashboardLayout
```

### Step 5

Inside:

```jsx
<Outlet />
```

React renders:

```text
Users
```

Final UI:

```text
Admin Dashboard

Overview | Users | Products | Reports

Users Page
```

---

# 3️⃣4️⃣ Routing Mental Model 🧠

Remember:

```text
createBrowserRouter()
          ↓
RouterProvider
          ↓
Route
          ↓
Layout
          ↓
Outlet
          ↓
Nested Route
          ↓
Component
```

---

# 3️⃣5️⃣ Common Mistakes ❌

## Mistake 1 — Forgetting Outlet

You create:

```jsx
children: [
    {
        path: "users",
        element: <Users />
    }
]
```

but your layout doesn't contain:

```jsx
<Outlet />
```

Then the child route has nowhere to appear.

Correct:

```jsx
function Layout() {
    return (
        <div>

            <h1>Layout</h1>

            <Outlet />

        </div>
    );
}
```

---

## Mistake 2 — Incorrect Child Path

Inside:

```jsx
path: "/dashboard"
```

don't unnecessarily write:

```jsx
path: "/dashboard/users"
```

Instead:

```jsx
path: "users"
```

Then React Router creates:

```text
/dashboard/users
```

---

## Mistake 3 — Forgetting RouterProvider

If you create:

```jsx
const router = createBrowserRouter(...)
```

you still need to provide it:

```jsx
<RouterProvider router={router} />
```

---

## Mistake 4 — Confusing Authentication and Authorization

Remember:

```text
Authentication = Who are you?

Authorization = What can you access?
```

---

# 3️⃣6️⃣ Best Practices ✅

### 1. Separate routes

Keep router configuration in a dedicated file:

```text
router/
└── router.jsx
```

### 2. Use layouts for shared UI

```text
Navbar
Sidebar
Footer
```

should not be unnecessarily duplicated.

### 3. Use Outlet for child routes

```jsx
<Outlet />
```

is the key to nested layouts.

### 4. Create a NotFound page

Always think about:

```text
*
```

for unknown routes.

### 5. Keep authentication logic organized

Don't scatter authentication checks across every page.

---

# 3️⃣7️⃣ Interview Questions 🎤

### Q1. What is `createBrowserRouter`?

**Answer:**

`createBrowserRouter` creates a router configuration using route objects.

---

### Q2. What is `RouterProvider`?

**Answer:**

`RouterProvider` provides the configured router to the React application.

Example:

```jsx
<RouterProvider router={router} />
```

---

### Q3. What is a layout route?

**Answer:**

A layout route provides shared UI around child routes, such as a navbar, sidebar, or footer.

---

### Q4. What is `Outlet`?

**Answer:**

`Outlet` is a placeholder where the currently matched child route is rendered.

---

### Q5. What are nested routes?

**Answer:**

Nested routes are routes defined inside another route, usually to represent a parent-child page structure.

---

### Q6. What is `index: true`?

**Answer:**

It defines the default child route of a parent route.

---

### Q7. What is a route loader?

**Answer:**

A route loader is a function associated with a route that can obtain data needed by that route.

---

### Q8. What is `useLoaderData()`?

**Answer:**

It allows a route component to access data returned by its route loader.

---

### Q9. What is a protected route?

**Answer:**

A protected route restricts access to certain pages based on authentication or authorization.

---

### Q10. Authentication vs Authorization?

**Answer:**

```text
Authentication → Who are you?

Authorization → What are you allowed to access?
```

---

# 3️⃣8️⃣ Practice Tasks 🧪

Create this structure:

```text
/dashboard
    ↓
DashboardLayout

/dashboard
/dashboard/users
/dashboard/products
/dashboard/orders
/dashboard/settings
```

Requirements:

- Create `DashboardLayout`
- Add `<Outlet />`
- Create child routes
- Create navigation links
- Create a `NotFound` page
- Use `createBrowserRouter`
- Use `RouterProvider`

---

# 3️⃣9️⃣ Mini Challenge 🚀

Build a small **Admin Panel Router**.

### Routes

```text
/login

/admin
/admin/users
/admin/products
/admin/orders
/admin/reports
```

### Requirements

```text
AdminLayout
     ↓
Outlet
     ↓
Users
Products
Orders
Reports
```

Then add:

```text
/admin/anything
```

and make sure it eventually has an appropriate not-found experience.

---

# 4️⃣0️⃣ Quiz 🧠

### Q1. Which function creates the router?

**Answer:**

```jsx
createBrowserRouter()
```

---

### Q2. Which component provides the router?

**Answer:**

```jsx
RouterProvider
```

---

### Q3. What does `Outlet` do?

**Answer:**

It renders the matching child route inside the parent layout.

---

### Q4. What is this?

```jsx
children: [
    {
        path: "users",
        element: <Users />
    }
]
```

**Answer:**

It defines a nested child route.

---

### Q5. What URL can result from:

```text
Parent: /dashboard
Child: users
```

**Answer:**

```text
/dashboard/users
```

---

### Q6. What does `index: true` mean?

**Answer:**

It represents the default child route.

---

### Q7. What is the purpose of a loader?

**Answer:**

To obtain route-related data before the route component uses it.

---

### Q8. What hook reads loader data?

**Answer:**

```jsx
useLoaderData()
```

---

### Q9. What does authentication answer?

**Answer:**

```text
Who are you?
```

---

### Q10. What does authorization answer?

**Answer:**

```text
What are you allowed to access?
```

---

# 🧠 FINAL MEMORY TRICK

Remember these five:

```text
createBrowserRouter
        ↓
Creates routes

RouterProvider
        ↓
Provides router

Layout
        ↓
Common UI

Outlet
        ↓
Child UI appears here

children
        ↓
Nested routes
```

And remember:

```text
Authentication
      ↓
Who are you?

Authorization
      ↓
What can you access?
```

---

# 📌 CHAPTER 22 QUICK REVISION

| Concept | Meaning |
|---|---|
| `createBrowserRouter` | Creates router configuration |
| `RouterProvider` | Provides router to app |
| Layout | Shared page structure |
| `Outlet` | Placeholder for child route |
| Nested Route | Route inside another route |
| `index` | Default child route |
| Loader | Gets route data |
| `useLoaderData` | Reads loader data |
| Error Route | Handles route errors |
| `*` | Catch-all route |
| Protected Route | Restricts access |
| Authentication | Identifies user |
| Authorization | Controls permissions |

---

# 🎯 CHAPTER 22 COMPLETE

The most important concept from this chapter is:

```text
                 Router
                   ↓
                Layout
                   ↓
                Outlet
                   ↓
             Nested Route
                   ↓
               Component
```

You are now ready for the next major React topic:

# 🌐 CHAPTER 23 — API INTEGRATION

We will learn:

- What is an API?
- REST API
- HTTP methods
- GET
- POST
- PUT
- PATCH
- DELETE
- `fetch()`
- Axios
- Request
- Response
- Headers
- Status codes
- JSON

And we will connect React with real backend APIs.

**Type `NEXT` to start Chapter 23.**