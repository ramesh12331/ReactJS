# 🧩 REACTJS — CHAPTER 5
# ⚛️ COMPONENTS

---

# 🎯 Chapter Goal

After completing this chapter, you should understand:

- 🧩 What is a component?
- 🤔 Why do we need components?
- ⚛️ Functional components
- 📝 Component naming
- 🏗️ Component structure
- ♻️ Reusable components
- 🧱 Component composition
- 👨‍👦 Parent and child components
- 🌳 Component hierarchy
- 🏢 Real-world component architecture

---

# 1️⃣ What Is a Component?

## 🧠 Simple Definition

A **component** is a small, reusable piece of a React user interface.

For example, a website can have:

```text
🌐 Website
│
├── 🧭 Navbar
├── 🏠 Home
├── 📦 Products
├── 🛒 Cart
└── 👣 Footer
```

Each part can be a separate React component.

So instead of creating one huge file, we divide the application into smaller pieces.

---

# 2️⃣ Real-World Analogy 🏠

Think about a house.

A house contains:

```text
🏠 House
│
├── 🚪 Door
├── 🪟 Windows
├── 🛋️ Living Room
├── 🍳 Kitchen
└── 🛏️ Bedroom
```

Each part has a specific purpose.

Similarly, a React application can contain:

```text
⚛️ React App
│
├── 🧭 Navbar
├── 🏠 Home
├── 📦 ProductList
├── 🛒 Cart
└── 👣 Footer
```

Each component handles one part of the UI.

### 🧠 Memory Trick

> **Component = Small building block of a React UI.**

---

# 3️⃣ Why Do We Need Components?

Imagine you build an e-commerce application.

Without components, you might put everything into:

```text
App.jsx
```

and eventually have:

```text
App.jsx
    ↓
1000+ lines
```

That becomes difficult to:

- understand
- maintain
- debug
- reuse
- test
- modify

Instead:

```text
App
│
├── Navbar
├── Sidebar
├── ProductList
│   ├── ProductCard
│   ├── ProductCard
│   └── ProductCard
├── Cart
└── Footer
```

Now the application is easier to manage.

Your course describes components as a way to divide a large UI into smaller, reusable and maintainable pieces.

---

# 4️⃣ First Component

Let's create a simple component.

```jsx id="x7t3pd"
function Welcome() {
    return (
        <h1>Welcome to React</h1>
    );
}

export default Welcome;
```

This is a React component.

The component name is:

```text
Welcome
```

---

# 5️⃣ Understanding the Component

Look at:

```jsx id="2w8l5e"
function Welcome() {
```

We created a JavaScript function:

```text
Welcome
```

Then:

```jsx id="z9q6l2"
return (
    <h1>Welcome to React</h1>
);
```

The function returns JSX.

Therefore:

```text
JavaScript Function
       +
JSX
       ↓
React Component
```

---

# 6️⃣ Functional Component ⭐

A **functional component** is a JavaScript function that returns UI.

Example:

```jsx id="g8h5q3"
function Header() {
    return (
        <header>
            <h1>My Website</h1>
        </header>
    );
}

export default Header;
```

Here:

```text
Header
```

is a functional component.

---

# 7️⃣ Another Example

```jsx id="j6k2sm"
function Footer() {
    return (
        <footer>
            <p>Copyright 2026</p>
        </footer>
    );
}

export default Footer;
```

We now have two components:

```text
Header
Footer
```

---

# 8️⃣ Component Naming Rules ⚠️

React component names should normally start with a **capital letter**.

### ✅ Correct

```jsx id="9tq7as"
function Header() {
    return <h1>Header</h1>;
}
```

```jsx id="8q4m1b"
function ProductCard() {
    return <div>Product</div>;
}
```

```jsx id="p2w9k6"
function UserProfile() {
    return <div>User</div>;
}
```

### ❌ Avoid

```jsx id="e2z7kd"
function header() {
    return <h1>Header</h1>;
}
```

```jsx id="u8m5xq"
function productCard() {
    return <div>Product</div>;
}
```

### 🧠 Why?

When React sees:

```jsx id="8g4xv2"
<Header />
```

it understands `Header` as a component.

But lowercase names are normally interpreted as HTML/custom element-style tags.

So remember:

> **React Component → Start with Capital Letter.**

---

# 9️⃣ Component File Naming

A common convention is:

```text
Header.jsx
Footer.jsx
Navbar.jsx
ProductCard.jsx
UserProfile.jsx
```

Notice the capital letters.

This style is called **PascalCase**.

Examples:

```text
UserProfile
ProductCard
ShoppingCart
LoginForm
```

---

# 🔟 Creating Our First Real Component

Suppose our application needs a header.

Create:

```text
src/components/Header.jsx
```

Code:

```jsx id="2j7x6v"
function Header() {
    return (
        <header>
            <h1>My Website</h1>
        </header>
    );
}

export default Header;
```

Now we have:

```text
src/
│
├── components/
│   └── Header.jsx
│
└── App.jsx
```

---

# 1️⃣1️⃣ Importing a Component

Now open:

```text
src/App.jsx
```

Import the component:

```jsx id="3r6w8k"
import Header from "./components/Header";
```

Then use it:

```jsx id="7k2m4s"
function App() {
    return (
        <div>
            <Header />

            <h2>Home Page</h2>
        </div>
    );
}

export default App;
```

---

# 🔄 1️⃣2️⃣ What Happens?

Let's follow the flow.

```text
App.jsx
   │
   │ import
   ↓
Header.jsx
   │
   │ returns
   ↓
<header>
    <h1>My Website</h1>
</header>
```

Browser:

```text
My Website
Home Page
```

---

# 1️⃣3️⃣ What Does `<Header />` Mean?

This:

```jsx id="f5r9va"
<Header />
```

means:

> "Render the `Header` component here."

It is similar to calling the component in the UI structure.

You can think:

```text
<Header />
    ↓
Run/render Header component
    ↓
Get its JSX
    ↓
Display it
```

---

# 1️⃣4️⃣ Component Structure

A simple component normally looks like:

```jsx id="0e6w5x"
function ComponentName() {

    return (
        // JSX
    );
}

export default ComponentName;
```

Example:

```jsx id="n8j4qa"
function Welcome() {

    return (
        <div>
            <h1>Welcome</h1>
            <p>Learn React</p>
        </div>
    );
}

export default Welcome;
```

---

# 1️⃣5️⃣ Component Parts

Let's divide it.

```jsx id="2p3x8r"
function Welcome() {
```

### Part 1

Component declaration.

```text
Welcome
```

is the component name.

---

Then:

```jsx id="8q5x2k"
return (
```

### Part 2

The component returns UI.

---

Then:

```jsx id="w6j1rm"
<h1>Welcome</h1>
<p>Learn React</p>
```

### Part 3

JSX describes the UI.

---

Finally:

```jsx id="a7d2fs"
export default Welcome;
```

### Part 4

The component is exported so another file can import it.

---

# 1️⃣6️⃣ Reusable Components ♻️

This is one of the most important benefits of components.

Suppose we create:

```jsx id="h4x7mz"
function Button() {
    return (
        <button>Click Me</button>
    );
}

export default Button;
```

Now we can use it multiple times:

```jsx id="r7c3ka"
function App() {
    return (
        <div>
            <Button />
            <Button />
            <Button />
        </div>
    );
}

export default App;
```

The same component is reused three times.

---

# 🧠 Think About It

Instead of writing:

```jsx id="9j3k5d"
<button>Click Me</button>
<button>Click Me</button>
<button>Click Me</button>
```

we can create:

```text
Button component
```

and reuse:

```jsx id="k5y8tr"
<Button />
<Button />
<Button />
```

This becomes much more useful when components become larger.

---

# 1️⃣7️⃣ Real Example — Product Card

Imagine an online shopping website.

A product card might contain:

```text
┌──────────────────────┐
│      📱 Image        │
│                      │
│      iPhone          │
│      ₹70,000         │
│                      │
│   [ Add to Cart ]    │
└──────────────────────┘
```

Instead of writing this UI repeatedly, create:

```text
ProductCard
```

Then:

```jsx id="c5m7xa"
<ProductCard />
<ProductCard />
<ProductCard />
```

Later, each card can receive different product information using **props**.

⚠️ Props are the next major topic in Chapter 6. We will not jump there yet.

---

# 1️⃣8️⃣ Component Composition 🧱

Component composition means:

> **Building a larger UI by combining smaller components.**

For example:

```text
App
│
├── Header
├── MainContent
│   ├── ProductList
│   │   ├── ProductCard
│   │   ├── ProductCard
│   │   └── ProductCard
│   │
│   └── Sidebar
│
└── Footer
```

This is component composition.

---

# 1️⃣9️⃣ Simple Composition Example

Create:

```jsx id="j8w4q1"
function Header() {
    return <h1>My Website</h1>;
}

function Footer() {
    return <p>Copyright 2026</p>;
}

function App() {
    return (
        <div>
            <Header />

            <main>
                <h2>Home Page</h2>
            </main>

            <Footer />
        </div>
    );
}

export default App;
```

Here:

```text
App
│
├── Header
├── Main
└── Footer
```

The `App` component is composed of smaller pieces.

---

# 2️⃣0️⃣ Parent and Child Components 👨‍👦

This is another important concept.

Suppose:

```text
App
 │
 ├── Header
 └── Footer
```

`App` is the:

```text
Parent
```

`Header` and `Footer` are:

```text
Children
```

So:

```text
App
│
├── Header
└── Footer
```

means:

```text
App = Parent
Header = Child
Footer = Child
```

---

# 2️⃣1️⃣ Parent → Child Example

```jsx id="r5t2yn"
function Header() {
    return (
        <h1>My Website</h1>
    );
}

function App() {
    return (
        <div>
            <Header />
        </div>
    );
}

export default App;
```

Here:

```text
App
 ↓
Header
```

`App` renders `Header`.

Therefore:

```text
App = Parent
Header = Child
```

---

# 2️⃣2️⃣ Component Hierarchy 🌳

A component hierarchy shows how components are connected.

Example:

```text
                 App
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
      Header              Footer
        │
        ↓
      Navbar
        │
   ┌────┴────┐
   ↓         ↓
 Logo      Menu
```

This is called a **component hierarchy**.

---

# 2️⃣3️⃣ Another Real-World Example

Imagine an e-commerce application.

```text
App
│
├── Navbar
│   ├── Logo
│   ├── SearchBar
│   └── CartIcon
│
├── Home
│   ├── Hero
│   ├── CategoryList
│   └── ProductList
│       ├── ProductCard
│       ├── ProductCard
│       └── ProductCard
│
└── Footer
```

This is how a large application can be divided.

---

# 2️⃣4️⃣ Why This Structure Is Useful

Suppose the `Navbar` has a problem.

Instead of searching through a huge `App.jsx`, you can open:

```text
components/Navbar.jsx
```

and work there.

Similarly:

```text
ProductCard.jsx
```

contains product-card UI.

This makes the project easier to:

- 🧠 Understand
- 🔧 Maintain
- 🐞 Debug
- ♻️ Reuse
- 🧪 Test
- 👥 Work on with a team

---

# 2️⃣5️⃣ A Small Project Structure

For now, create:

```text
src/
│
├── components/
│   ├── Header.jsx
│   ├── Footer.jsx
│   └── Button.jsx
│
├── App.jsx
├── main.jsx
└── index.css
```

This is a simple beginning.

---

# 2️⃣6️⃣ Complete Example

### `Header.jsx`

```jsx id="q7r3vm"
function Header() {
    return (
        <header>
            <h1>My Website</h1>
        </header>
    );
}

export default Header;
```

---

### `Footer.jsx`

```jsx id="p2m8xs"
function Footer() {
    return (
        <footer>
            <p>Copyright 2026</p>
        </footer>
    );
}

export default Footer;
```

---

### `Button.jsx`

```jsx id="a6k1wd"
function Button() {
    return (
        <button>
            Click Me
        </button>
    );
}

export default Button;
```

---

### `App.jsx`

```jsx id="n4v8qz"
import Header from "./components/Header";
import Footer from "./components/Footer";
import Button from "./components/Button";

function App() {
    return (
        <div>
            <Header />

            <main>
                <h2>Home Page</h2>

                <Button />
            </main>

            <Footer />
        </div>
    );
}

export default App;
```

---

# 🔍 2️⃣7️⃣ Dry Run

Let's understand what happens.

### Step 1

React starts with:

```text
App
```

### Step 2

`App` imports:

```text
Header
Footer
Button
```

### Step 3

App renders:

```jsx id="sg2y5p"
<Header />
```

React displays Header.

### Step 4

App renders:

```jsx id="t7x9qa"
<Button />
```

React displays Button.

### Step 5

App renders:

```jsx id="v5j3sm"
<Footer />
```

React displays Footer.

Final UI:

```text
My Website

Home Page

[ Click Me ]

Copyright 2026
```

---

# 2️⃣8️⃣ Component Flow

Remember this:

```text
App
 │
 ├── Header
 │
 ├── Main
 │    └── Button
 │
 └── Footer
```

The UI is built by **combining components**.

---

# 2️⃣9️⃣ Component vs HTML Element

This is very important.

HTML element:

```jsx
<h1>Hello</h1>
```

React component:

```jsx
<Header />
```

Notice the capital letter:

```text
<h1>       → HTML element

<Header />  → React component
```

### 🧠 Memory Trick

> **Lowercase → normally HTML element**  
> **Capitalized → React component**

---

# 3️⃣0️⃣ Component vs Function

A React functional component is a JavaScript function that returns UI.

For example:

```jsx id="y4r6mn"
function Welcome() {
    return <h1>Welcome</h1>;
}
```

This is both:

```text
JavaScript function
        +
React component
```

because it returns UI and is used as a component.

---

# 3️⃣1️⃣ Common Beginner Mistakes ❌

## Mistake 1 — Lowercase component name

❌

```jsx id="7t5h3c"
function header() {
    return <h1>Header</h1>;
}
```

Prefer:

```jsx id="0r8k6p"
function Header() {
    return <h1>Header</h1>;
}
```

---

## Mistake 2 — Forgetting export

You create:

```jsx id="z7v4qb"
function Header() {
    return <h1>Header</h1>;
}
```

and then try to import it elsewhere.

If it is meant to be a default import, add:

```jsx id="6y1k3r"
export default Header;
```

---

## Mistake 3 — Wrong import path

If your structure is:

```text
src/
├── App.jsx
└── components/
    └── Header.jsx
```

from `App.jsx`, the import is:

```jsx id="x9q2vm"
import Header from "./components/Header";
```

---

## Mistake 4 — Trying to make everything one component

Don't create:

```text
App.jsx
   ↓
1000 lines
```

Break the UI into logical components.

---

## Mistake 5 — Creating too many tiny components

Components are useful, but don't create a separate component for every tiny piece without a reason.

For example, you don't necessarily need:

```text
LetterH.jsx
LetterE.jsx
LetterL.jsx
```

😄

Use components where they make the UI easier to understand, reuse, or maintain.

---

# 3️⃣2️⃣ Best Practices ⭐

### ✅ 1. Use PascalCase

```text
Navbar.jsx
ProductCard.jsx
UserProfile.jsx
LoginForm.jsx
```

---

### ✅ 2. Give components meaningful names

Good:

```text
ProductCard
UserProfile
OrderList
```

Avoid unclear names like:

```text
Box1
Test
ABC
X
```

---

### ✅ 3. Keep components focused

A component should have a clear purpose.

For example:

```text
Navbar → navigation
ProductCard → product display
Footer → footer
```

---

### ✅ 4. Reuse components

If you need the same UI multiple times:

```jsx id="m2k9az"
<ProductCard />
<ProductCard />
<ProductCard />
```

create one component and reuse it.

---

### ✅ 5. Organize components

As the project grows:

```text
src/
│
├── components/
├── pages/
├── services/
└── hooks/
```

We will learn these structures later.

---

# 🏢 3️⃣3️⃣ Real-World Component Architecture

Imagine a real shopping application:

```text
src/
│
├── components/
│   │
│   ├── Navbar.jsx
│   ├── Footer.jsx
│   ├── Button.jsx
│   ├── ProductCard.jsx
│   └── SearchBar.jsx
│
├── pages/
│   │
│   ├── Home.jsx
│   ├── Products.jsx
│   ├── ProductDetails.jsx
│   └── Cart.jsx
│
├── App.jsx
└── main.jsx
```

Think:

```text
App
│
├── Navbar
│
├── Pages
│   ├── Home
│   ├── Products
│   └── Cart
│
└── Footer
```

This is the beginning of real-world React architecture.

---

# 🧠 3️⃣4️⃣ Important Concept

Don't think:

> "React is just writing HTML."

Instead think:

> **React is building UI by combining components.**

For example:

```text
Application
      ↓
   Components
      ↓
   Components
      ↓
      UI
```

---

# 🎤 3️⃣5️⃣ Interview Questions

## Q1. What is a React component?

### Answer:

A React component is a reusable piece of UI.

It can contain JSX and logic needed to display part of an application.

---

## Q2. What is a functional component?

### Answer:

A functional component is a JavaScript function that returns React UI.

Example:

```jsx id="j4k7xs"
function Welcome() {
    return <h1>Welcome</h1>;
}
```

---

## Q3. Why should component names start with capital letters?

### Answer:

Capitalized names allow React to distinguish user-defined components from normal lowercase HTML elements.

---

## Q4. What is component composition?

### Answer:

Component composition means building a larger UI by combining smaller components.

Example:

```text
App
├── Header
├── Main
└── Footer
```

---

## Q5. What is a reusable component?

### Answer:

A reusable component is a component that can be used in multiple places in an application.

Example:

```jsx id="w8r2kp"
<Button />
<Button />
<Button />
```

---

## Q6. What is a parent component?

### Answer:

A parent component is a component that renders another component.

Example:

```jsx id="f9c4xm"
function App() {
    return <Header />;
}
```

Here:

```text
App → Parent
Header → Child
```

---

## Q7. What is a child component?

### Answer:

A child component is a component rendered by another component.

---

## Q8. What is component hierarchy?

### Answer:

Component hierarchy describes how components are arranged and related to each other.

Example:

```text
App
├── Header
├── Main
│   └── ProductCard
└── Footer
```

---

## Q9. Why are components useful?

### Answer:

Components help divide large applications into smaller, reusable, maintainable pieces.

---

## Q10. Is every JavaScript function a React component?

### Answer:

No.

A React functional component is a function used as a component and returns React UI.

---

# 📝 3️⃣6️⃣ Practice Questions

Try answering these without looking above.

### Practice 1

What is a React component?

### Practice 2

Why do we need components?

### Practice 3

Why should component names start with capital letters?

### Practice 4

What is a functional component?

### Practice 5

What is component composition?

### Practice 6

In this structure:

```text
App
│
├── Header
└── Footer
```

Which component is the parent?

### Practice 7

What is component hierarchy?

### Practice 8

Why are reusable components useful?

---

# 💪 3️⃣7️⃣ Mini Challenge

Create a simple **Student Profile** component.

Create:

```text
src/components/Student.jsx
```

Start with:

```jsx
function Student() {

    // Your code here

}

export default Student;
```

It should display:

```text
Student Profile

Name: Ramesh
Course: React.js
Experience: 3 Years
```

Then use it inside:

```text
App.jsx
```

### 🤔 Think First

Ask yourself:

```text
1. What component should I create?
        ↓
Student

2. What should it return?
        ↓
JSX

3. Where should I create the file?
        ↓
src/components/Student.jsx

4. How will App use it?
        ↓
Import + <Student />
```

⚠️ **Don't use props yet.**

We will learn props in **Chapter 6**.

---

# 🧪 3️⃣8️⃣ 10-Question Quiz

## Q1. What is a component?

A. Database  
B. Reusable UI piece  
C. CSS file  
D. npm package

### ✅ Answer: B

A component is a reusable piece of UI.

---

## Q2. Which is a correctly named component?

A.

```text
productcard
```

B.

```text
ProductCard
```

C.

```text
product-card
```

### ✅ Answer: B

```text
ProductCard
```

---

## Q3. What does this represent?

```jsx
<Header />
```

### ✅ Answer

It renders the `Header` React component.

---

## Q4. Which is a functional component?

A.

```jsx
function Header() {
    return <h1>Header</h1>;
}
```

B.

```jsx
const name = "Ramesh";
```

### ✅ Answer: A

It is a function that returns UI.

---

## Q5. What is component reuse?

### ✅ Answer

Using the same component in multiple places.

Example:

```jsx
<Button />
<Button />
<Button />
```

---

## Q6. In this structure:

```text
App
└── Header
```

Who is the parent?

### ✅ Answer

```text
App
```

---

## Q7. Who is the child?

```text
App
└── Header
```

### ✅ Answer

```text
Header
```

---

## Q8. What is component composition?

### ✅ Answer

Combining smaller components to build a larger UI.

---

## Q9. What does this represent?

```text
App
├── Navbar
├── ProductList
└── Footer
```

### ✅ Answer

A component hierarchy.

---

## Q10. Why should components be reusable?

### ✅ Answer

Reusability reduces duplicate code and makes applications easier to maintain and manage.

---

# 📚 3️⃣9️⃣ Chapter Summary

| Concept | Meaning |
|---|---|
| 🧩 Component | Reusable UI piece |
| ⚛️ Functional Component | Function that returns UI |
| 📝 PascalCase | Common naming style for components |
| ♻️ Reusable Component | Component used in multiple places |
| 🧱 Composition | Combining components |
| 👨 Parent | Component that renders another component |
| 👶 Child | Component rendered by another component |
| 🌳 Hierarchy | Component relationship/tree |
| 🏢 Architecture | Organization of components in a real application |

---

# 🧠 4️⃣0️⃣ Final Memory Trick

Remember this:

```text
React
  ↓
Components
  ↓
Small UI pieces
  ↓
Combine them
  ↓
Large Application
```

Example:

```text
                 App
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
    Navbar      Main       Footer
                  │
                  ↓
             ProductList
                  │
          ┌───────┼───────┐
          ↓       ↓       ↓
       Product Product Product
        Card    Card    Card
```

### ⭐ Most important idea

> **A React application is built by combining small, reusable components.**

---

# ✅ Chapter 5 Checklist

Before moving to the next chapter, make sure you can explain:

- [ ] What is a component?
- [ ] Why do we need components?
- [ ] What is a functional component?
- [ ] How do we name components?
- [ ] How do we create a component?
- [ ] How do we import a component?
- [ ] How do we render a component?
- [ ] What is component reuse?
- [ ] What is component composition?
- [ ] What is a parent component?
- [ ] What is a child component?
- [ ] What is component hierarchy?
- [ ] How components are organized in a real project?

---

# 🚀 NEXT CHAPTER

## 📦 CHAPTER 6 — PROPS

This is a **very important React concept**.

We will learn:

```text
What are Props?
      ↓
Why Props?
      ↓
Passing Props
      ↓
Receiving Props
      ↓
Multiple Props
      ↓
Props Destructuring
      ↓
Default Values
      ↓
Objects
      ↓
Arrays
      ↓
Functions
      ↓
Props vs Variables
      ↓
Props are Read-Only
      ↓
Real-world Examples
```

Your uploaded syllabus places all of these topics in Chapter 6.

🛑 **STOP HERE.**

Type **`NEXT`** when you are ready for **Chapter 6 — Props**.