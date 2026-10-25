# 🚀 CHAPTER 21 — REACT ROUTER

Welcome to **Chapter 21**! 🎯

In previous chapters, we learned:

- Components
- Props
- Events
- State
- Hooks
- Context
- State Management

Now we will learn how to create **multiple pages/screens inside a React application**.

---

# 📚 What We Will Learn

1. What is Routing?
2. Why do we need React Router?
3. SPA and Routing
4. Installing React Router
5. BrowserRouter
6. Routes
7. Route
8. Link
9. NavLink
10. Navigation
11. Route structure
12. Practical project
13. Dry run
14. Common mistakes
15. Best practices
16. Interview questions
17. Practice tasks
18. Mini challenge
19. Quiz
20. Final revision

---

# 1️⃣ What is Routing?

### Simple Definition

**Routing means displaying different components based on the URL.**

For example:

```text
http://localhost:5173/
```

shows:

```text
Home
```

And:

```text
http://localhost:5173/about
```

shows:

```text
About
```

And:

```text
http://localhost:5173/contact
```

shows:

```text
Contact
```

So:

```text
URL
 ↓
React Router
 ↓
Matching Route
 ↓
Component
```

---

# 2️⃣ Real-Life Analogy 🏠

Think about a house.

A house can have:

```text
🏠 Home
 ├── 🛋️ Living Room
 ├── 🍳 Kitchen
 ├── 🛏️ Bedroom
 └── 🚿 Bathroom
```

Each room has a different purpose.

Similarly, a website can have:

```text
🌐 Website
 ├── 🏠 Home
 ├── 👤 About
 ├── 🛍️ Products
 └── 📞 Contact
```

The URL tells React which "room" to display.

---

# 3️⃣ Why Do We Need React Router?

Imagine a website with:

```text
Home
About
Products
Contact
Login
Register
```

Without routing, we would have to manually decide which component to display.

React Router provides a proper routing system.

It allows us to create:

```text
/
/about
/products
/contact
/login
/register
```

---

# 4️⃣ Traditional Website vs React SPA

## Traditional Website

Suppose you click:

```text
About
```

The browser may request another HTML page from the server.

```text
Browser
   ↓
Server
   ↓
about.html
   ↓
Browser reload
```

---

# 5️⃣ React SPA

React commonly works as a **Single Page Application (SPA)**.

The application loads once and React changes the displayed UI according to the route.

```text
Browser
   ↓
React Application
   ↓
React Router
   ↓
Component
```

For example:

```text
/
 ↓
Home

/about
 ↓
About

/products
 ↓
Products
```

The page does not need to be rebuilt as a separate traditional HTML page for each route.

---

# 6️⃣ Install React Router

Inside your React project, install the router package used by your project setup.

A common installation is:

```bash
npm install react-router-dom
```

After installation, you can import routing components.

Example:

```jsx
import {
    BrowserRouter,
    Routes,
    Route,
    Link,
    NavLink
} from "react-router-dom";
```

---

# 7️⃣ Important React Router Components

Remember these names:

| Component | Purpose |
|---|---|
| `BrowserRouter` | Enables routing |
| `Routes` | Holds route definitions |
| `Route` | Defines a URL and component |
| `Link` | Navigates between routes |
| `NavLink` | Navigation link with active state |

🧠 Memory:

```text
BrowserRouter → Routing ON
Routes        → Route container
Route         → URL → Component
Link          → Navigate
NavLink       → Navigate + Active Style
```

---

# 8️⃣ BrowserRouter

`BrowserRouter` provides routing functionality to your application.

Example:

```jsx
import { BrowserRouter } from "react-router-dom";

function App() {
    return (
        <BrowserRouter>
            <h1>My Application</h1>
        </BrowserRouter>
    );
}

export default App;
```

Think of it as:

```text
BrowserRouter
     ↓
"React, routing is enabled inside here."
```

---

# 9️⃣ Routes

`Routes` contains your route definitions.

Example:

```jsx
<Routes>
    {/* routes go here */}
</Routes>
```

---

# 🔟 Route

A `Route` connects a URL path with a component.

Example:

```jsx
<Route path="/" element={<Home />} />
```

This means:

```text
URL:
/

↓

Component:
<Home />
```

Another:

```jsx
<Route path="/about" element={<About />} />
```

Means:

```text
/about
   ↓
<About />
```

---

# 1️⃣1️⃣ First Routing Example

Let's create three components.

## Home.jsx

```jsx
function Home() {
    return <h1>Home Page</h1>;
}

export default Home;
```

## About.jsx

```jsx
function About() {
    return <h1>About Page</h1>;
}

export default About;
```

## Contact.jsx

```jsx
function Contact() {
    return <h1>Contact Page</h1>;
}

export default Contact;
```

Now configure routing.

## App.jsx

```jsx
import {
    BrowserRouter,
    Routes,
    Route
} from "react-router-dom";

import Home from "./Home";
import About from "./About";
import Contact from "./Contact";

function App() {
    return (
        <BrowserRouter>

            <Routes>

                <Route
                    path="/"
                    element={<Home />}
                />

                <Route
                    path="/about"
                    element={<About />}
                />

                <Route
                    path="/contact"
                    element={<Contact />}
                />

            </Routes>

        </BrowserRouter>
    );
}

export default App;
```

---

# 1️⃣2️⃣ Understand the Code

This:

```jsx
<BrowserRouter>
```

enables routing.

Then:

```jsx
<Routes>
```

contains the routes.

Then:

```jsx
<Route
    path="/"
    element={<Home />}
/>
```

means:

```text
If URL = /
        ↓
Show Home
```

This:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

means:

```text
If URL = /about
        ↓
Show About
```

---

# 1️⃣3️⃣ Routing Diagram

```text
                 Browser URL
                      │
                      ↓
              React Router
                      │
          ┌───────────┼───────────┐
          ↓           ↓           ↓
          /         /about     /contact
          ↓           ↓           ↓
        Home        About       Contact
```

---

# 1️⃣4️⃣ What is Link?

We don't normally want users to manually type URLs.

We need navigation links.

React Router provides:

```jsx
<Link>
```

Example:

```jsx
import { Link } from "react-router-dom";

function Navbar() {
    return (
        <nav>

            <Link to="/">Home</Link>

            <Link to="/about">About</Link>

            <Link to="/contact">Contact</Link>

        </nav>
    );
}

export default Navbar;
```

---

# 1️⃣5️⃣ Why Use Link?

You might think:

```html
<a href="/about">About</a>
```

Why not use that?

React applications generally use:

```jsx
<Link to="/about">
    About
</Link>
```

for client-side navigation.

### Important

```jsx
<Link to="/about">
```

uses React Router navigation.

---

# 1️⃣6️⃣ Complete Example with Navbar

Let's combine everything.

```jsx
import {
    BrowserRouter,
    Routes,
    Route,
    Link
} from "react-router-dom";

function Home() {
    return <h1>Home Page 🏠</h1>;
}

function About() {
    return <h1>About Page 👤</h1>;
}

function Contact() {
    return <h1>Contact Page 📞</h1>;
}

function App() {
    return (
        <BrowserRouter>

            <nav>

                <Link to="/">
                    Home
                </Link>

                {" | "}

                <Link to="/about">
                    About
                </Link>

                {" | "}

                <Link to="/contact">
                    Contact
                </Link>

            </nav>

            <Routes>

                <Route
                    path="/"
                    element={<Home />}
                />

                <Route
                    path="/about"
                    element={<About />}
                />

                <Route
                    path="/contact"
                    element={<Contact />}
                />

            </Routes>

        </BrowserRouter>
    );
}

export default App;
```

---

# 1️⃣7️⃣ Dry Run 🧠

Suppose you click:

```text
About
```

The code is:

```jsx
<Link to="/about">
    About
</Link>
```

### Step 1

User clicks:

```text
About
```

### Step 2

React Router navigates to:

```text
/about
```

### Step 3

React checks:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

### Step 4

Path matches.

### Step 5

React renders:

```jsx
<About />
```

Output:

```text
About Page 👤
```

---

# 1️⃣8️⃣ What is NavLink?

`NavLink` is similar to `Link`.

But it provides information about whether the link is currently active.

Example:

```jsx
import { NavLink } from "react-router-dom";

<NavLink to="/">
    Home
</NavLink>
```

For navigation menus, `NavLink` is very useful.

---

# 1️⃣9️⃣ Link vs NavLink

| Link | NavLink |
|---|---|
| Navigation | Navigation |
| Simple link | Navigation with active state |
| No built-in active styling information | Can detect active route |
| Good for normal links | Good for navbar/menu |

Example:

```jsx
<Link to="/about">
    About
</Link>
```

vs

```jsx
<NavLink to="/about">
    About
</NavLink>
```

---

# 2️⃣0️⃣ NavLink Active Styling

Example:

```jsx
<NavLink
    to="/about"
    className={({ isActive }) =>
        isActive ? "active" : ""
    }
>
    About
</NavLink>
```

Here:

```jsx
isActive
```

tells us whether the current URL matches the link.

Concept:

```text
Current URL = /about

About NavLink
     ↓
isActive = true
```

Then:

```text
active
```

class can be applied.

---

# 2️⃣1️⃣ Practical Navbar

```jsx
import { NavLink } from "react-router-dom";

function Navbar() {
    return (
        <nav>

            <NavLink
                to="/"
                className={({ isActive }) =>
                    isActive ? "active" : ""
                }
            >
                Home
            </NavLink>

            <NavLink
                to="/about"
                className={({ isActive }) =>
                    isActive ? "active" : ""
                }
            >
                About
            </NavLink>

            <NavLink
                to="/contact"
                className={({ isActive }) =>
                    isActive ? "active" : ""
                }
            >
                Contact
            </NavLink>

        </nav>
    );
}

export default Navbar;
```

---

# 2️⃣2️⃣ Project Structure

A simple project can look like:

```text
src/
│
├── components/
│   └── Navbar.jsx
│
├── pages/
│   ├── Home.jsx
│   ├── About.jsx
│   └── Contact.jsx
│
├── App.jsx
├── main.jsx
└── index.css
```

This is cleaner than putting every component into `App.jsx`.

---

# 2️⃣3️⃣ Practical Project

Let's build:

```text
React Website
│
├── 🏠 Home
├── 👤 About
├── 🛍️ Products
└── 📞 Contact
```

### Home.jsx

```jsx
function Home() {
    return (
        <div>
            <h1>Home</h1>
            <p>Welcome to our website.</p>
        </div>
    );
}

export default Home;
```

### About.jsx

```jsx
function About() {
    return (
        <div>
            <h1>About</h1>
            <p>This is our about page.</p>
        </div>
    );
}

export default About;
```

### Products.jsx

```jsx
function Products() {
    return (
        <div>
            <h1>Products</h1>
            <p>Our products are displayed here.</p>
        </div>
    );
}

export default Products;
```

### Contact.jsx

```jsx
function Contact() {
    return (
        <div>
            <h1>Contact</h1>
            <p>Contact us here.</p>
        </div>
    );
}

export default Contact;
```

---

# 2️⃣4️⃣ App.jsx

```jsx
import {
    BrowserRouter,
    Routes,
    Route,
    NavLink
} from "react-router-dom";

import Home from "./pages/Home";
import About from "./pages/About";
import Products from "./pages/Products";
import Contact from "./pages/Contact";

function App() {
    return (
        <BrowserRouter>

            <nav>

                <NavLink to="/">
                    Home
                </NavLink>

                {" | "}

                <NavLink to="/about">
                    About
                </NavLink>

                {" | "}

                <NavLink to="/products">
                    Products
                </NavLink>

                {" | "}

                <NavLink to="/contact">
                    Contact
                </NavLink>

            </nav>

            <Routes>

                <Route
                    path="/"
                    element={<Home />}
                />

                <Route
                    path="/about"
                    element={<About />}
                />

                <Route
                    path="/products"
                    element={<Products />}
                />

                <Route
                    path="/contact"
                    element={<Contact />}
                />

            </Routes>

        </BrowserRouter>
    );
}

export default App;
```

---

# 2️⃣5️⃣ URL Testing

Run:

```bash
npm run dev
```

Then test:

### Home

```text
http://localhost:5173/
```

Output:

```text
Home
Welcome to our website.
```

### About

```text
http://localhost:5173/about
```

### Products

```text
http://localhost:5173/products
```

### Contact

```text
http://localhost:5173/contact
```

---

# 2️⃣6️⃣ Important Concept — Route Mapping

Remember this table:

| URL | Component |
|---|---|
| `/` | Home |
| `/about` | About |
| `/products` | Products |
| `/contact` | Contact |

Think:

```text
path
 ↓
component
```

Example:

```jsx
<Route
    path="/products"
    element={<Products />}
/>
```

means:

```text
/products
    ↓
Products
```

---

# 2️⃣7️⃣ Common Mistakes ❌

## Mistake 1 — Forgetting BrowserRouter

Wrong:

```jsx
<Routes>
    <Route path="/" element={<Home />} />
</Routes>
```

If routing isn't properly provided, routing components won't work as expected.

Better:

```jsx
<BrowserRouter>

    <Routes>
        <Route path="/" element={<Home />} />
    </Routes>

</BrowserRouter>
```

---

## Mistake 2 — Wrong `to`

Wrong:

```jsx
<Link to="about">
```

Prefer:

```jsx
<Link to="/about">
```

---

## Mistake 3 — Wrong Route Path

```jsx
<Link to="/about">
    About
</Link>
```

but:

```jsx
<Route
    path="/contact"
    element={<About />}
/>
```

Now the URLs don't represent the intended mapping.

Correct:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

---

# 2️⃣8️⃣ Mistake — Calling Component Instead of Rendering

Wrong:

```jsx
<Route
    path="/about"
    element={About()}
/>
```

Correct:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

The `element` receives a React element.

---

# 2️⃣9️⃣ Best Practices ✅

### 1. Keep pages separate

```text
pages/
├── Home.jsx
├── About.jsx
└── Contact.jsx
```

### 2. Keep navigation separate

```text
components/
└── Navbar.jsx
```

### 3. Use `Link`/`NavLink`

For React Router navigation, use the router's navigation components.

### 4. Keep routes readable

```jsx
<Routes>

    <Route path="/" element={<Home />} />

    <Route path="/about" element={<About />} />

    <Route path="/products" element={<Products />} />

</Routes>
```

### 5. Don't put everything in one giant component

Separate:

```text
Navbar
Pages
Reusable Components
Routes
```

---

# 3️⃣0️⃣ Interview Questions 🎯

### Q1. What is React Router?

**Answer:**

React Router is a routing library used to handle navigation and different URLs in React applications.

---

### Q2. Why do we use React Router?

**Answer:**

We use React Router to display different components for different URLs in a React application.

---

### Q3. What is BrowserRouter?

**Answer:**

`BrowserRouter` enables routing functionality using the browser's URL.

---

### Q4. What is Routes?

**Answer:**

`Routes` is a container that holds the application's route definitions.

---

### Q5. What is Route?

**Answer:**

`Route` maps a URL path to a React component.

Example:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

---

### Q6. What is Link?

**Answer:**

`Link` is used to navigate between routes without using a normal page link.

---

### Q7. Link vs NavLink?

**Answer:**

Both are used for navigation. `NavLink` also provides information about whether the current route is active, which is useful for navigation menus.

---

### Q8. What is SPA?

**Answer:**

SPA means **Single Page Application**. The application can update its UI according to navigation without requiring a traditional full-page load for every route.

---

### Q9. Can React have multiple pages?

**Answer:**

Yes. React Router can provide multiple URL-based views/pages within a single React application.

---

### Q10. What is route mapping?

**Answer:**

Route mapping means connecting a URL path with the component that should be displayed.

Example:

```text
/about → About
```

---

# 3️⃣1️⃣ Practice Tasks 🧪

Create a React application with these pages:

```text
🏠 Home
👤 About
📚 Courses
💼 Services
📞 Contact
```

Create these routes:

```text
/
 /about
 /courses
 /services
 /contact
```

Create a navbar using:

```jsx
NavLink
```

### Your goal

When you click:

```text
Courses
```

the URL should become:

```text
/courses
```

and the Courses component should appear.

---

# 3️⃣2️⃣ Mini Challenge 🚀

Create a **Student Website**.

### Pages

```text
Home
Students
Courses
About
Contact
```

### Routes

```text
/
 /students
 /courses
 /about
 /contact
```

### Navbar

Use:

```jsx
NavLink
```

### Requirement

The active page should receive an:

```text
active
```

class.

---

# 3️⃣3️⃣ Quiz 🧠

### Q1. What is routing?

**Answer:** Routing connects URLs with the UI/components that should be displayed.

---

### Q2. Which component enables browser-based routing?

**Answer:** `BrowserRouter`

---

### Q3. Which component contains routes?

**Answer:** `Routes`

---

### Q4. Which component defines an individual route?

**Answer:** `Route`

---

### Q5. How do you navigate to `/about` using Link?

**Answer:**

```jsx
<Link to="/about">
    About
</Link>
```

---

### Q6. What is the main difference between Link and NavLink?

**Answer:** `NavLink` can determine whether its route is currently active.

---

### Q7. What component should be displayed for `/products`?

```jsx
<Route
    path="/products"
    element={<Products />}
/>
```

**Answer:** `Products`

---

### Q8. What does SPA mean?

**Answer:** Single Page Application.

---

### Q9. Is this correct?

```jsx
<Route
    path="/about"
    element={About()}
/>
```

**Answer:** ❌ No.

Use:

```jsx
<Route
    path="/about"
    element={<About />}
/>
```

---

### Q10. What is the basic routing flow?

**Answer:**

```text
URL
 ↓
React Router
 ↓
Route
 ↓
Component
 ↓
UI
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
BrowserRouter
     ↓
    Routes
     ↓
    Route
     ↓
Component
```

And:

```text
Link     → Navigate
NavLink  → Navigate + Active State
```

The most important pattern is:

```jsx
<BrowserRouter>

    <Routes>

        <Route
            path="/"
            element={<Home />}
        />

        <Route
            path="/about"
            element={<About />}
        />

    </Routes>

</BrowserRouter>
```

### 🏆 Chapter 21 in one sentence:

> **React Router connects URLs to React components and allows users to navigate between different views of a React application.**

---

# ✅ CHAPTER 21 CHECKLIST

Before moving forward, make sure you understand:

- [ ] What routing means
- [ ] Why React Router is used
- [ ] SPA concept
- [ ] BrowserRouter
- [ ] Routes
- [ ] Route
- [ ] Link
- [ ] NavLink
- [ ] `path`
- [ ] `element`
- [ ] Basic navigation
- [ ] Route mapping
- [ ] Basic project structure
- [ ] Common routing mistakes

---

# 🚀 NEXT

## CHAPTER 22 — REACT ROUTER ADVANCED

We will learn:

- 🔥 Dynamic Routes
- 🔥 Route Parameters
- 🔥 `useParams`
- 🔥 Nested Routes
- 🔥 `Outlet`
- 🔥 `useNavigate`
- 🔥 Programmatic Navigation
- 🔥 404 / Not Found Route
- 🔥 Route-based practical project
- 🔥 Interview questions
- 🔥 Practice
- 🔥 Quiz

Type **NEXT** when you are ready.