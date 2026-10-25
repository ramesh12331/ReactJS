
# 🎨 CHAPTER 29 — CSS WITH REACT

## 🎯 Goal of This Chapter

Until now, we focused mainly on:

```text
React
 ↓
Components
 ↓
Props
 ↓
State
 ↓
Events
 ↓
API
```

But a real application also needs good UI design.

For example:

```text
Without CSS

Hello Ramesh
Click Me
```

With CSS:

```text
┌──────────────────────────────┐
│       Welcome Ramesh 👋      │
│                              │
│       [  Click Me  ]         │
└──────────────────────────────┘
```

So in this chapter we learn how to use CSS with React.

---

# 📚 CHAPTER TOPICS

We will learn:

1. CSS in React
2. CSS files
3. Importing CSS
4. Class names
5. Inline CSS
6. CSS Modules
7. Dynamic styles
8. Conditional classes
9. Responsive design
10. Practical component styling
11. Common mistakes
12. Best practices
13. Interview questions
14. Practice tasks
15. Mini challenge
16. Quiz
17. Final revision

---

# 1️⃣ HOW DOES CSS WORK IN REACT?

React does not replace CSS.

You can still use normal CSS.

```text
HTML/CSS
   +
JavaScript
   +
React
   ↓
Complete UI
```

React creates the UI.

CSS controls the appearance.

---

# 2️⃣ REAL-WORLD ANALOGY 🏠

Think about building a house.

```text
React
 ↓
Structure of the house

CSS
 ↓
Paint + design + layout
```

For example:

```text
React
→ Button exists

CSS
→ Button color
→ Button size
→ Button spacing
→ Button shape
→ Hover effect
```

---

# 3️⃣ METHOD 1 — CSS FILES 📄

This is one of the simplest ways to style React components.

Suppose:

```text
src/
├── App.jsx
└── App.css
```

### App.jsx

```jsx
function App() {

    return (
        <div className="container">

            <h1>
                Hello React
            </h1>

            <button>
                Click Me
            </button>

        </div>
    );
}

export default App;
```

### App.css

```css
.container {
    padding: 20px;
}

h1 {
    color: blue;
}

button {
    background-color: black;
    color: white;
    padding: 10px 20px;
}
```

---

# 4️⃣ IMPORT THE CSS FILE

In `App.jsx`:

```jsx
import "./App.css";
```

Complete:

```jsx
import "./App.css";

function App() {

    return (
        <div className="container">

            <h1>
                Hello React
            </h1>

            <button>
                Click Me
            </button>

        </div>
    );
}

export default App;
```

---

# 5️⃣ WHY `className`?

In normal HTML:

```html
<div class="container">
```

In JSX:

```jsx
<div className="container">
```

React JSX uses:

```text
className
```

instead of:

```text
class
```

### Remember:

```text
HTML
class

React JSX
className
```

---

# 6️⃣ DRY RUN 🧠

React sees:

```jsx
<div className="container">
```

CSS contains:

```css
.container {
    padding: 20px;
}
```

Browser applies the CSS.

Flow:

```text
App.jsx
   ↓
className="container"
   ↓
App.css
   ↓
.container
   ↓
Browser styling
```

---

# 7️⃣ CSS CLASS EXAMPLE

CSS:

```css
.title {
    color: purple;
    font-size: 32px;
}
```

React:

```jsx
<h1 className="title">
    Welcome
</h1>
```

Output:

```text
Welcome
```

with the CSS styling applied.

---

# 8️⃣ MULTIPLE CLASSES

You can use multiple classes.

```jsx
<button className="btn primary">
    Save
</button>
```

CSS:

```css
.btn {
    padding: 10px 20px;
}

.primary {
    background-color: blue;
    color: white;
}
```

The element receives both classes:

```text
btn
+
primary
```

---

# 9️⃣ METHOD 2 — INLINE CSS

React also supports inline styles.

Example:

```jsx
function App() {

    return (
        <h1
            style={{
                color: "blue",
                fontSize: "32px"
            }}
        >
            Hello React
        </h1>
    );
}

export default App;
```

---

# 🔟 IMPORTANT INLINE CSS RULE

Normal CSS:

```css
font-size: 30px;
```

React inline style:

```jsx
fontSize: "30px"
```

Why?

Because React uses JavaScript object property names.

So:

```text
font-size
```

becomes:

```text
fontSize
```

---

# 1️⃣1️⃣ INLINE STYLE OBJECT

You can create a style object.

```jsx
function App() {

    const titleStyle = {
        color: "blue",
        fontSize: "32px",
        textAlign: "center"
    };

    return (
        <h1 style={titleStyle}>
            Hello React
        </h1>
    );
}

export default App;
```

This is easier to read than putting a large object directly inside JSX.

---

# 1️⃣2️⃣ WHY USE INLINE CSS?

Inline CSS is useful when:

```text
Style is very small
       OR
Style depends directly on JavaScript data
```

Example:

```jsx
const color = "green";

<p style={{ color }}>
    Success
</p>
```

---

# 1️⃣3️⃣ INLINE CSS vs CSS FILE

### CSS file

```css
.title {
    color: blue;
}
```

```jsx
<h1 className="title">
    Hello
</h1>
```

### Inline CSS

```jsx
<h1
    style={{
        color: "blue"
    }}
>
    Hello
</h1>
```

For larger applications, separate CSS files or other organized styling approaches are usually easier to maintain.

---

# 1️⃣4️⃣ METHOD 3 — CSS MODULES 🧩

CSS Modules allow CSS to be locally scoped to a component.

Example:

```text
src/
├── Button.jsx
└── Button.module.css
```

---

# 1️⃣5️⃣ CREATE `Button.module.css`

```css
.button {
    background-color: blue;
    color: white;
    padding: 10px 20px;
    border-radius: 6px;
}
```

---

# 1️⃣6️⃣ USE CSS MODULE IN REACT

`Button.jsx`:

```jsx
import styles from "./Button.module.css";

function Button() {

    return (
        <button className={styles.button}>
            Click Me
        </button>
    );
}

export default Button;
```

Notice:

```jsx
styles.button
```

instead of:

```text
"button"
```

---

# 1️⃣7️⃣ WHY CSS MODULES?

Imagine:

```text
Header.css

.button
```

and:

```text
Footer.css

.button
```

Both could have a class called:

```text
.button
```

With normal global CSS, those selectors can conflict.

CSS Modules scope the styles to the component/module.

Conceptually:

```text
Button.jsx
   ↓
Button.module.css
   ↓
Scoped class
   ↓
Button component
```

---

# 1️⃣8️⃣ CSS MODULE DRY RUN

You write:

```css
.button {
    background: blue;
}
```

Then:

```jsx
className={styles.button}
```

The build system generates a scoped class name behind the scenes.

Conceptually:

```text
button
 ↓
scoped generated class
 ↓
browser
```

You don't need to manually create a unique class name.

---

# 1️⃣9️⃣ CSS FILE vs CSS MODULE

| CSS File | CSS Module |
|---|---|
| Usually global CSS | Locally scoped |
| `className="button"` | `className={styles.button}` |
| Easy for global styles | Useful for component-specific styles |
| Possible class conflicts | Helps avoid naming conflicts |

---

# 2️⃣0️⃣ METHOD 4 — DYNAMIC STYLES 🔄

Sometimes styling depends on data.

Example:

```text
Product stock

10 → Green
5  → Orange
0  → Red
```

We can dynamically calculate a style.

---

# 2️⃣1️⃣ DYNAMIC INLINE STYLE

```jsx
function Product({ stock }) {

    const color =
        stock > 0
            ? "green"
            : "red";

    return (
        <p style={{ color }}>
            Stock: {stock}
        </p>
    );
}

export default Product;
```

If:

```text
stock = 10
```

then:

```text
green
```

If:

```text
stock = 0
```

then:

```text
red
```

---

# 2️⃣2️⃣ DYNAMIC STYLE DRY RUN

Suppose:

```jsx
<Product stock={10} />
```

Component receives:

```text
stock = 10
```

Condition:

```javascript
stock > 0
```

Result:

```text
true
```

Therefore:

```text
color = green
```

Output:

```text
Stock: 10
```

with green text.

---

# 2️⃣3️⃣ CONDITIONAL CLASSES 🔀

Instead of changing inline styles, we can change CSS classes.

Example:

```css
.success {
    color: green;
}

.error {
    color: red;
}
```

React:

```jsx
function Message({ success }) {

    return (
        <p
            className={
                success
                    ? "success"
                    : "error"
            }
        >
            {success
                ? "Payment Successful"
                : "Payment Failed"}
        </p>
    );
}
```

---

# 2️⃣4️⃣ CONDITIONAL CLASS FLOW

```text
success = true
      ↓
"success"
      ↓
green text
```

Or:

```text
success = false
      ↓
"error"
      ↓
red text
```

---

# 2️⃣5️⃣ THREE COMMON WAYS

### Option 1 — Normal CSS

```jsx
<p className="title">
    Hello
</p>
```

### Option 2 — Inline CSS

```jsx
<p
    style={{
        color: "blue"
    }}
>
    Hello
</p>
```

### Option 3 — CSS Module

```jsx
<p className={styles.title}>
    Hello
</p>
```

---

# 2️⃣6️⃣ CONDITIONAL MULTIPLE CLASSES

Suppose:

```css
button {
    padding: 10px;
}

active {
    background: green;
}

disabled {
    background: gray;
}
```

Better CSS:

```css
.button {
    padding: 10px;
}

.active {
    background-color: green;
}

.disabled {
    background-color: gray;
}
```

React:

```jsx
<button
    className={`button ${
        isActive
            ? "active"
            : "disabled"
    }`}
>
    Submit
</button>
```

This is a JavaScript template literal.

---

# 2️⃣7️⃣ EASIER CONDITIONAL CLASS

You can also use:

```jsx
<button
    className={
        isActive
            ? "button active"
            : "button disabled"
    }
>
    Submit
</button>
```

For beginners, this version is often easier to understand.

---

# 2️⃣8️⃣ RESPONSIVE DESIGN 📱💻

Responsive design means:

> The UI adjusts to different screen sizes.

For example:

```text
Desktop
┌──────────────────────────────┐
│ Card │ Card │ Card │ Card    │
└──────────────────────────────┘
```

Mobile:

```text
┌──────────────┐
│     Card     │
├──────────────┤
│     Card     │
├──────────────┤
│     Card     │
└──────────────┘
```

---

# 2️⃣9️⃣ MEDIA QUERY

Normal CSS:

```css
.container {
    display: flex;
    gap: 20px;
}
```

Mobile:

```css
@media (max-width: 768px) {

    .container {
        flex-direction: column;
    }
}
```

---

# 3️⃣0️⃣ RESPONSIVE REACT EXAMPLE

React:

```jsx
function Products() {

    return (
        <div className="products">

            <div className="card">
                Product 1
            </div>

            <div className="card">
                Product 2
            </div>

            <div className="card">
                Product 3
            </div>

        </div>
    );
}

export default Products;
```

CSS:

```css
.products {
    display: grid;
    grid-template-columns:
        repeat(3, 1fr);
    gap: 20px;
}

.card {
    padding: 20px;
    border: 1px solid #ddd;
}
```

Mobile:

```css
@media (max-width: 768px) {

    .products {
        grid-template-columns: 1fr;
    }
}
```

---

# 3️⃣1️⃣ RESPONSIVE FLOW

```text
Browser width
      ↓
CSS Media Query
      ↓
Which rule matches?
      ↓
Desktop?
      ↓
3 columns

Mobile?
      ↓
1 column
```

React does not need to detect the screen size just to achieve ordinary responsive CSS layouts.

CSS handles most responsive layout requirements.

---

# 3️⃣2️⃣ REAL-WORLD PRODUCT CARD

Let's combine React + CSS.

### ProductCard.jsx

```jsx
import "./ProductCard.css";

function ProductCard({
    name,
    price,
    available
}) {

    return (
        <div className="product-card">

            <h2>
                {name}
            </h2>

            <p>
                Price: ₹{price}
            </p>

            <p
                className={
                    available
                        ? "available"
                        : "out-of-stock"
                }
            >
                {available
                    ? "Available"
                    : "Out of Stock"}
            </p>

            <button
                disabled={!available}
            >
                Buy Now
            </button>

        </div>
    );
}

export default ProductCard;
```

---

# 3️⃣3️⃣ PRODUCT CARD CSS

```css
.product-card {
    width: 250px;
    padding: 20px;
    border: 1px solid #ddd;
    border-radius: 10px;
}

.available {
    color: green;
}

.out-of-stock {
    color: red;
}

.product-card button {
    padding: 10px 20px;
    cursor: pointer;
}
```

---

# 3️⃣4️⃣ USING THE COMPONENT

```jsx
import ProductCard
    from "./ProductCard";

function App() {

    return (
        <div>

            <ProductCard
                name="Laptop"
                price={55000}
                available={true}
            />

            <ProductCard
                name="Phone"
                price={25000}
                available={false}
            />

        </div>
    );
}

export default App;
```

---

# 3️⃣5️⃣ WHAT HAPPENS?

First component:

```text
Laptop
₹55000
Available
Buy Now
```

Second:

```text
Phone
₹25000
Out of Stock
Buy Now disabled
```

The same component is reused.

Only the data changes.

---

# 3️⃣6️⃣ CSS + PROPS

This is an important React pattern.

```text
Props
 ↓
Data
 ↓
Condition
 ↓
CSS class
 ↓
UI
```

Example:

```jsx
className={
    available
        ? "available"
        : "out-of-stock"
}
```

---

# 3️⃣7️⃣ CSS + STATE

Styles can also depend on state.

```jsx
import { useState } from "react";

function Toggle() {

    const [active, setActive] =
        useState(false);

    return (
        <button
            className={
                active
                    ? "active"
                    : "inactive"
            }
            onClick={() =>
                setActive(!active)
            }
        >
            {active
                ? "ON"
                : "OFF"}
        </button>
    );
}

export default Toggle;
```

CSS:

```css
.active {
    background-color: green;
    color: white;
}

.inactive {
    background-color: gray;
    color: white;
}
```

---

# 3️⃣8️⃣ DRY RUN

Initial state:

```text
active = false
```

Class:

```text
inactive
```

Button:

```text
OFF
```

User clicks.

```text
setActive(true)
```

Now:

```text
active = true
```

Class:

```text
active
```

Button:

```text
ON
```

Flow:

```text
Click
 ↓
State changes
 ↓
React re-renders
 ↓
Class changes
 ↓
CSS changes appearance
```

---

# 3️⃣9️⃣ COMMON MISTAKE ❌

## Mistake 1 — Using `class`

Wrong:

```jsx
<div class="container">
```

Correct:

```jsx
<div className="container">
```

---

# 4️⃣0️⃣ COMMON MISTAKE ❌

## Mistake 2 — Wrong inline style syntax

Wrong:

```jsx
<div
    style={{
        background-color: "red"
    }}
>
```

Correct:

```jsx
<div
    style={{
        backgroundColor: "red"
    }}
>
```

---

# 4️⃣1️⃣ COMMON MISTAKE ❌

## Mistake 3 — Forgetting CSS import

You created:

```text
Button.css
```

but forgot:

```jsx
import "./Button.css";
```

Then the styles may not be applied.

---

# 4️⃣2️⃣ COMMON MISTAKE ❌

## Mistake 4 — CSS Module used incorrectly

Wrong:

```jsx
import styles
    from "./Button.module.css";

<button className="button">
```

Correct:

```jsx
<button className={styles.button}>
```

---

# 4️⃣3️⃣ COMMON MISTAKE ❌

## Mistake 5 — Confusing JavaScript and CSS

Wrong idea:

```jsx
style="color: red"
```

React inline style uses a JavaScript object:

```jsx
style={{
    color: "red"
}}
```

---

# 4️⃣4️⃣ COMMON MISTAKE ❌

## Mistake 6 — Making everything inline

Avoid putting large amounts of styling directly into JSX.

Instead of:

```jsx
<div
    style={{
        padding: "20px",
        margin: "10px",
        border: "1px solid gray",
        borderRadius: "10px"
    }}
>
```

For reusable styling, a CSS file or CSS Module can be cleaner.

---

# 4️⃣5️⃣ BEST PRACTICES ⭐

### ✅ 1. Use meaningful class names

Good:

```text
product-card
login-form
navbar
profile-card
```

Avoid:

```text
box1
abc
x123
```

---

### ✅ 2. Keep component styles organized

For example:

```text
components/
│
├── Button.jsx
├── Button.css
│
├── Card.jsx
├── Card.css
```

---

### ✅ 3. Use CSS Modules when local scoping is useful

```text
Button.module.css
```

---

### ✅ 4. Use responsive design

Always consider:

```text
Desktop
Tablet
Mobile
```

---

### ✅ 5. Don't duplicate styles unnecessarily

Create reusable classes/components.

---

### ✅ 6. Keep JavaScript logic separate from styling when possible

Use JavaScript for:

```text
Data
State
Logic
Conditions
```

Use CSS for:

```text
Colors
Spacing
Layout
Typography
Responsive design
Animations
```

---

# 4️⃣6️⃣ WHICH METHOD SHOULD YOU USE?

For a beginner, remember:

| Situation | Good Choice |
|---|---|
| Global styles | CSS file |
| Component-specific styles | CSS Module |
| Very small dynamic style | Inline style |
| Dynamic appearance | Conditional classes / dynamic styles |
| Responsive layout | CSS media queries |
| Large application | Organized styling strategy |

---

# 4️⃣7️⃣ INTERVIEW QUESTIONS 🎤

## Q1. How do you apply CSS in React?

**Answer:**

We can use CSS files, inline styles, CSS Modules, and conditional/dynamic styling approaches.

---

## Q2. Why does React use `className`?

**Answer:**

JSX uses `className` for the CSS class attribute.

```jsx
<div className="container">
```

---

## Q3. How do you write inline CSS in React?

**Answer:**

Using a JavaScript object:

```jsx
<div
    style={{
        color: "red"
    }}
>
```

---

## Q4. What are CSS Modules?

**Answer:**

CSS Modules provide locally scoped CSS class names for components/modules, helping reduce class-name conflicts.

---

## Q5. What is dynamic styling?

**Answer:**

Dynamic styling means changing styles based on JavaScript data, props, or state.

---

## Q6. What is conditional class rendering?

**Answer:**

Choosing a CSS class based on a condition.

Example:

```jsx
className={
    isActive
        ? "active"
        : "inactive"
}
```

---

## Q7. What is responsive design?

**Answer:**

Responsive design means creating UI that adapts to different screen sizes and devices.

---

## Q8. How do you make a React UI responsive?

**Answer:**

Commonly by using CSS techniques such as media queries, flexible layouts, Flexbox, Grid, and responsive sizing.

---

## Q9. CSS file vs CSS Module?

**Answer:**

A normal CSS file generally creates global selectors, while CSS Modules provide locally scoped class names.

---

## Q10. Should all CSS be inline in React?

**Answer:**

No. Inline styles are useful for certain cases, especially small dynamic styles. Larger reusable styling is often better organized using CSS files or CSS Modules.

---

# 4️⃣8️⃣ PRACTICE TASKS 📝

## Task 1 — Student Card

Create:

```text
StudentCard.jsx
StudentCard.css
```

Display:

```text
Name
Course
Experience
City
```

Style the card.

---

## Task 2 — Button Component

Create:

```text
Button.jsx
Button.css
```

Create:

```text
Primary
Secondary
Danger
```

buttons.

---

## Task 3 — Dynamic Status

Create:

```text
Active
Inactive
Pending
```

Use different CSS classes based on status.

---

## Task 4 — Responsive Product Grid

Create:

```text
Product 1
Product 2
Product 3
Product 4
```

Desktop:

```text
4 columns
```

Mobile:

```text
1 column
```

---

## Task 5 — CSS Module

Create:

```text
Card.jsx
Card.module.css
```

Style the card using:

```jsx
styles.card
```

---

# 4️⃣9️⃣ MINI CHALLENGE 🚀

## 🛒 BUILD A RESPONSIVE PRODUCT UI

Create:

```text
ProductCard.jsx
ProductCard.css
App.jsx
```

Each product should have:

```text
Product image
Product name
Price
Category
Stock status
Buy button
```

Example:

```text
┌─────────────────────┐
│      📱 IMAGE       │
│                     │
│      iPhone         │
│      ₹70,000        │
│      Electronics    │
│                     │
│      Available      │
│                     │
│    [ Buy Now ]      │
└─────────────────────┘
```

### Requirements

```text
✅ CSS file
✅ Props
✅ Conditional class
✅ Dynamic stock status
✅ Responsive design
✅ Reusable ProductCard
```

---

# 5️⃣0️⃣ QUIZ 🧠

### Q1. What attribute is used instead of `class` in JSX?

**Answer:**

```text
className
```

---

### Q2. How do you write inline CSS in React?

**Answer:**

```jsx
style={{
    color: "red"
}}
```

---

### Q3. What is a CSS Module?

**Answer:**

A CSS file whose class names are scoped to the importing module/component.

---

### Q4. How do you import a normal CSS file?

**Answer:**

```jsx
import "./App.css";
```

---

### Q5. How do you import a CSS Module?

**Answer:**

```jsx
import styles from "./App.module.css";
```

---

### Q6. How do you use a CSS Module class?

**Answer:**

```jsx
className={styles.button}
```

---

### Q7. What is dynamic styling?

**Answer:**

Changing styling based on JavaScript data, props, or state.

---

### Q8. What is conditional class rendering?

**Answer:**

Selecting a CSS class depending on a condition.

---

### Q9. What is responsive design?

**Answer:**

Design that adapts to different screen sizes and devices.

---

### Q10. What CSS feature is commonly used for responsive breakpoints?

**Answer:**

```text
Media queries
```

---

# 🧠 FINAL MEMORY TRICK

Remember the **5 CSS methods/topics** from this chapter:

```text
📄 CSS File
     ↓
className

🎯 Inline CSS
     ↓
style={{ }}

🧩 CSS Module
     ↓
styles.button

🔀 Conditional Class
     ↓
condition ? "active" : "inactive"

📱 Responsive Design
     ↓
Media Queries
```

And remember:

```text
React
 ↓
Component
 ↓
Props / State
 ↓
Condition
 ↓
CSS Class
 ↓
Styled UI
```

---

# ✅ CHAPTER 29 CHECKLIST

Before moving forward, make sure you understand:

- [ ] CSS in React
- [ ] CSS files
- [ ] `className`
- [ ] Importing CSS
- [ ] Multiple classes
- [ ] Inline CSS
- [ ] Inline style object
- [ ] CSS Modules
- [ ] `styles.className`
- [ ] Dynamic styles
- [ ] Conditional classes
- [ ] Props + CSS
- [ ] State + CSS
- [ ] Responsive design
- [ ] Media queries
- [ ] Common CSS mistakes
- [ ] CSS best practices

# 🎉 CHAPTER 29 COMPLETE

You now know how to make your React components **look good, reusable, dynamic, and responsive**.

The next chapter is:

# ⏭️ CHAPTER 30 — TAILWIND CSS WITH REACT 💨

We will learn:

```text
Tailwind Setup
Utility Classes
Responsive Design
Flexbox
Grid
Spacing
Typography
Colors
Borders
Shadows
Hover
Focus
Transitions
Animations
Dark Mode
```

These are the topics listed for Chapter 30 in your React course.

Type **`NEXT`** when you are ready.