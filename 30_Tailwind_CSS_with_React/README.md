# 💨 CHAPTER 30 — TAILWIND CSS WITH REACT

## 🎯 Goal of This Chapter

In Chapter 29, we learned normal CSS.

Now we will learn **Tailwind CSS**.

By the end of this chapter, you should be able to build React UI like:

```text
┌───────────────────────────────────┐
│           My Products             │
│                                   │
│  ┌─────────┐  ┌─────────┐        │
│  │ Laptop  │  │ Phone   │        │
│  │ ₹50,000 │  │ ₹25,000 │        │
│  │ [ Buy ] │  │ [ Buy ] │        │
│  └─────────┘  └─────────┘        │
│                                   │
└───────────────────────────────────┘
```

without writing a large CSS file.

---

# 📚 CHAPTER TOPICS

We will learn:

1. What is Tailwind CSS?
2. Why Tailwind?
3. Tailwind vs normal CSS
4. Tailwind setup with Vite
5. Utility classes
6. Colors
7. Spacing
8. Typography
9. Borders
10. Rounded corners
11. Shadows
12. Width and height
13. Flexbox
14. Grid
15. Responsive design
16. Hover
17. Focus
18. Transitions
19. Animations
20. Dark mode
21. React + Tailwind practical project
22. Common mistakes
23. Best practices
24. Interview questions
25. Practice
26. Mini challenge
27. Quiz
28. Summary

---

# 1️⃣ WHAT IS TAILWIND CSS?

Tailwind CSS is a **utility-first CSS framework**.

Instead of writing:

```css
.button {
    background-color: blue;
    color: white;
    padding: 10px 20px;
    border-radius: 8px;
}
```

we can write utility classes directly in JSX:

```jsx
<button className="bg-blue-600 text-white px-5 py-2 rounded-lg">
    Click Me
</button>
```

So:

```text
Normal CSS
     ↓
Create CSS class
     ↓
Write CSS
     ↓
Use className
```

Tailwind:

```text
Tailwind utility classes
     ↓
Write classes directly
     ↓
React UI
```

---

# 2️⃣ WHY TAILWIND?

Suppose you need a button.

With normal CSS:

```text
Button.jsx
     +
Button.css
```

You write CSS separately.

With Tailwind:

```jsx
<button className="bg-blue-600 text-white px-4 py-2 rounded">
    Save
</button>
```

The styling is visible directly in the component.

---

# 3️⃣ REAL-WORLD ANALOGY 🧱

Imagine building a house.

Normal CSS:

```text
Create a complete room design
       ↓
Give the room a name
       ↓
Apply the design
```

Tailwind:

```text
Give wall
Give color
Give spacing
Give size
Give border
Give shadow
```

You combine small building blocks.

That is why Tailwind uses **utility classes**.

---

# 4️⃣ WHAT IS A UTILITY CLASS?

A utility class usually performs one small styling job.

Examples:

```text
text-center
```

means:

```text
center the text
```

```text
font-bold
```

means:

```text
bold text
```

```text
bg-blue-600
```

means:

```text
blue background
```

```text
p-4
```

means:

```text
padding
```

```text
rounded-lg
```

means:

```text
rounded corners
```

---

# 5️⃣ TAILWIND SETUP WITH VITE 🚀

Because you are learning React with Vite, we will use the current Vite integration.

From your existing React project:

```bash
npm install tailwindcss @tailwindcss/vite
```

The official Tailwind documentation currently recommends this Vite-plugin approach.

---

# 6️⃣ CONFIGURE `vite.config.js`

Your Vite configuration will contain the Tailwind plugin.

Example:

```javascript
import { defineConfig } from "vite";
import react from "@vitejs/plugin-react";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
    plugins: [
        react(),
        tailwindcss()
    ]
});
```

### Important

You may already have:

```javascript
react()
```

because your project is a React Vite project.

We add:

```javascript
tailwindcss()
```

---

# 7️⃣ IMPORT TAILWIND

Open your main CSS file.

For example:

```text
src/index.css
```

Add:

```css
@import "tailwindcss";
```

This is the current Tailwind v4 approach.

---

# 8️⃣ RUN THE PROJECT

```bash
npm run dev
```

Then test Tailwind:

```jsx
function App() {

    return (
        <h1 className="text-3xl font-bold underline">
            Hello Tailwind
        </h1>
    );
}

export default App;
```

If Tailwind is working, you should see:

```text
Hello Tailwind
```

with:

```text
Large text
Bold text
Underline
```

---

# 9️⃣ IMPORTANT VERSION NOTE ⚠️

You may find tutorials showing:

```bash
npx tailwindcss init -p
```

and:

```css
@tailwind base;
@tailwind components;
@tailwind utilities;
```

Those are associated with older Tailwind workflows, particularly v3.

Current Tailwind v4 uses:

```bash
npm install tailwindcss @tailwindcss/vite
```

and:

```css
@import "tailwindcss";
```

The official upgrade guide documents this change.

### 🧠 Beginner rule

Don't mix:

```text
Tailwind v3 setup
```

with:

```text
Tailwind v4 setup
```

Choose one version and follow its documentation.

For our current course, we will learn using the modern approach.

---

# 1️⃣0️⃣ BASIC TAILWIND EXAMPLE

```jsx
function App() {

    return (
        <div className="p-5">

            <h1 className="text-3xl font-bold">
                Hello React
            </h1>

            <button className="bg-blue-600 text-white px-5 py-2 rounded-lg">
                Click Me
            </button>

        </div>
    );
}

export default App;
```

---

# 1️⃣1️⃣ BREAK THE CODE DOWN

Look at:

```text
p-5
```

Padding.

```text
text-3xl
```

Large text.

```text
font-bold
```

Bold font.

```text
bg-blue-600
```

Blue background.

```text
text-white
```

White text.

```text
px-5
```

Horizontal padding.

```text
py-2
```

Vertical padding.

```text
rounded-lg
```

Rounded corners.

---

# 1️⃣2️⃣ TAILWIND MINDSET 🧠

Don't memorize everything.

Think:

```text
What do I need?
       ↓
Color?
       ↓
bg-blue-600

Text?
       ↓
text-white

Padding?
       ↓
p-4

Rounded?
       ↓
rounded-lg

Shadow?
       ↓
shadow-lg
```

You learn the pattern.

---

# 1️⃣3️⃣ COLORS 🎨

Examples:

```text
bg-red-500
bg-blue-500
bg-green-500
bg-yellow-500
bg-purple-500
bg-gray-500
```

Text:

```text
text-red-500
text-blue-500
text-green-500
text-gray-700
```

---

# 1️⃣4️⃣ COLOR SCALE

You may see:

```text
blue-50
blue-100
blue-200
blue-300
blue-400
blue-500
blue-600
blue-700
blue-800
blue-900
```

Conceptually:

```text
50
 ↓
Very light

500
 ↓
Middle

900
 ↓
Very dark
```

Example:

```jsx
<div className="bg-blue-100">
    Light Blue
</div>

<div className="bg-blue-600">
    Strong Blue
</div>

<div className="bg-blue-900 text-white">
    Dark Blue
</div>
```

---

# 1️⃣5️⃣ TEXT COLORS

```jsx
<p className="text-red-500">
    Error
</p>

<p className="text-green-500">
    Success
</p>

<p className="text-gray-600">
    Information
</p>
```

---

# 1️⃣6️⃣ SPACING 📏

Tailwind provides utility classes for spacing.

### Padding

```text
p-1
p-2
p-3
p-4
p-5
p-6
```

### Margin

```text
m-1
m-2
m-4
m-6
```

---

# 1️⃣7️⃣ PADDING DIRECTIONS

```text
p-4
```

All sides.

```text
px-4
```

Left + right.

```text
py-4
```

Top + bottom.

```text
pt-4
```

Top.

```text
pb-4
```

Bottom.

```text
pl-4
```

Left.

```text
pr-4
```

Right.

---

# 1️⃣8️⃣ MARGIN DIRECTIONS

```text
m-4
```

All sides.

```text
mx-auto
```

Horizontal auto margin.

Very commonly used for centered containers.

Example:

```jsx
<div className="max-w-4xl mx-auto">
    Content
</div>
```

---

# 1️⃣9️⃣ TYPOGRAPHY ✍️

Font size:

```text
text-sm
text-base
text-lg
text-xl
text-2xl
text-3xl
text-4xl
```

Example:

```jsx
<h1 className="text-4xl">
    Main Heading
</h1>

<p className="text-lg">
    Description
</p>
```

---

# 2️⃣0️⃣ FONT WEIGHT

```text
font-normal
font-medium
font-semibold
font-bold
font-extrabold
```

Example:

```jsx
<h1 className="font-bold">
    React Course
</h1>
```

---

# 2️⃣1️⃣ TEXT ALIGNMENT

```text
text-left
text-center
text-right
```

Example:

```jsx
<h1 className="text-center">
    Welcome
</h1>
```

---

# 2️⃣2️⃣ BORDERS

Examples:

```text
border
border-2
border-4
```

Color:

```text
border-gray-300
border-blue-500
border-red-500
```

Example:

```jsx
<div className="border border-gray-300">
    Card
</div>
```

---

# 2️⃣3️⃣ BORDER RADIUS

```text
rounded
rounded-sm
rounded-md
rounded-lg
rounded-xl
rounded-full
```

Example:

```jsx
<button className="rounded-lg">
    Save
</button>
```

For a circular element:

```jsx
<div className="rounded-full">
    Avatar
</div>
```

---

# 2️⃣4️⃣ SHADOWS

Tailwind provides:

```text
shadow-sm
shadow
shadow-md
shadow-lg
shadow-xl
shadow-2xl
```

Example:

```jsx
<div className="shadow-lg">
    Product Card
</div>
```

---

# 2️⃣5️⃣ WIDTH AND HEIGHT

Width:

```text
w-full
w-1/2
w-64
```

Height:

```text
h-10
h-20
h-64
h-screen
```

Example:

```jsx
<div className="w-full h-40">
    Banner
</div>
```

---

# 2️⃣6️⃣ FLEXBOX 🧩

Flexbox is extremely important.

Tailwind:

```text
flex
```

Example:

```jsx
<div className="flex">
    <div>One</div>
    <div>Two</div>
</div>
```

Conceptually:

```text
Before:

One
Two

After flex:

One   Two
```

---

# 2️⃣7️⃣ FLEX DIRECTION

Horizontal:

```text
flex-row
```

Vertical:

```text
flex-col
```

Example:

```jsx
<div className="flex flex-col">
    <p>Name</p>
    <p>Email</p>
</div>
```

---

# 2️⃣8️⃣ JUSTIFY CONTENT

```text
justify-start
justify-center
justify-end
justify-between
justify-around
justify-evenly
```

Example:

```jsx
<div className="flex justify-between">
    <span>Logo</span>
    <span>Menu</span>
</div>
```

Result:

```text
Logo                         Menu
```

---

# 2️⃣9️⃣ ALIGN ITEMS

```text
items-start
items-center
items-end
```

Very common:

```jsx
<div className="flex items-center">
    ...
</div>
```

Meaning:

```text
Flex
 +
Vertical alignment center
```

---

# 3️⃣0️⃣ GAP

Instead of manually adding margins between flex/grid items:

```text
gap-2
gap-4
gap-6
gap-8
```

Example:

```jsx
<div className="flex gap-4">
    <button>Save</button>
    <button>Cancel</button>
</div>
```

---

# 3️⃣1️⃣ FLEXBOX PRACTICAL EXAMPLE

Navbar:

```jsx
function Navbar() {

    return (
        <nav className="flex items-center justify-between p-4">

            <h1 className="text-xl font-bold">
                MyApp
            </h1>

            <div className="flex gap-4">
                <a href="#">Home</a>
                <a href="#">About</a>
                <a href="#">Contact</a>
            </div>

        </nav>
    );
}

export default Navbar;
```

---

# 3️⃣2️⃣ FLEXBOX DRY RUN

Classes:

```text
flex
```

→ puts children in flex layout.

```text
items-center
```

→ aligns items on the cross axis.

```text
justify-between
```

→ pushes first and last content apart.

```text
p-4
```

→ adds padding.

```text
gap-4
```

→ adds space between menu items.

---

# 3️⃣3️⃣ GRID 📊

Grid is useful for:

```text
Cards
Products
Gallery
Dashboard
Tables/layout sections
```

Example:

```jsx
<div className="grid grid-cols-3 gap-6">

    <div>Card 1</div>
    <div>Card 2</div>
    <div>Card 3</div>

</div>
```

Output:

```text
┌────────┐ ┌────────┐ ┌────────┐
│ Card 1 │ │ Card 2 │ │ Card 3 │
└────────┘ └────────┘ └────────┘
```

---

# 3️⃣4️⃣ GRID COLUMNS

```text
grid-cols-1
grid-cols-2
grid-cols-3
grid-cols-4
```

Example:

```jsx
<div className="grid grid-cols-4 gap-4">
```

means:

```text
4 columns
```

---

# 3️⃣5️⃣ RESPONSIVE DESIGN 📱

This is one of Tailwind's biggest advantages.

Tailwind provides responsive prefixes such as:

```text
sm:
md:
lg:
xl:
2xl:
```

Example:

```jsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4">
```

Meaning:

```text
Mobile
↓
1 column

Medium screen
↓
2 columns

Large screen
↓
4 columns
```

---

# 3️⃣6️⃣ RESPONSIVE FLOW

```text
Browser
   ↓
Screen width
   ↓
Tailwind breakpoint
   ↓
Matching class
   ↓
Responsive UI
```

---

# 3️⃣7️⃣ RESPONSIVE CARD EXAMPLE

```jsx
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">

    <div>Product 1</div>
    <div>Product 2</div>
    <div>Product 3</div>

</div>
```

Desktop:

```text
┌────────┐ ┌────────┐ ┌────────┐
│ P1     │ │ P2     │ │ P3     │
└────────┘ └────────┘ └────────┘
```

Mobile:

```text
┌──────────────┐
│ P1           │
└──────────────┘

┌──────────────┐
│ P2           │
└──────────────┘

┌──────────────┐
│ P3           │
└──────────────┘
```

---

# 3️⃣8️⃣ HOVER 🖱️

Tailwind supports state variants.

Example:

```jsx
<button className="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
    Save
</button>
```

Normal:

```text
blue-600
```

Hover:

```text
blue-700
```

---

# 3️⃣9️⃣ HOVER SCALE

```jsx
<div className="hover:scale-105">
    Product
</div>
```

When the mouse moves over it, it scales.

For a smooth effect, combine it with:

```text
transition
```

---

# 4️⃣0️⃣ FOCUS 🎯

Focus happens when an input receives keyboard/mouse focus.

Example:

```jsx
<input
    className="border border-gray-300 focus:border-blue-500 focus:outline-none"
    placeholder="Enter name"
/>
```

Normal:

```text
gray border
```

Focused:

```text
blue border
```

---

# 4️⃣1️⃣ TRANSITIONS

Without transition:

```text
Blue
 ↓
Immediately
 ↓
Dark blue
```

With:

```text
transition
```

the visual change can happen smoothly.

Example:

```jsx
<button className="bg-blue-600 hover:bg-blue-700 transition">
    Hover Me
</button>
```

---

# 4️⃣2️⃣ TRANSITION DURATION

You can control duration.

Example:

```text
duration-150
duration-300
duration-500
```

Example:

```jsx
<button className="bg-blue-600 hover:bg-blue-700 transition duration-300">
    Save
</button>
```

---

# 4️⃣3️⃣ ANIMATIONS 🎬

Tailwind includes animation utilities.

Examples:

```text
animate-spin
animate-pulse
animate-bounce
```

Loading spinner:

```jsx
<div className="animate-spin">
    ⟳
</div>
```

Pulse:

```jsx
<div className="animate-pulse">
    Loading...
</div>
```

---

# 4️⃣4️⃣ DARK MODE 🌙

Tailwind supports dark-mode styling.

Example:

```jsx
<div className="bg-white text-black dark:bg-gray-900 dark:text-white">
    Hello
</div>
```

Normal mode:

```text
White background
Black text
```

Dark mode:

```text
Dark background
White text
```

---

# 4️⃣5️⃣ DARK MODE MINDSET

Normal class:

```text
bg-white
```

Dark version:

```text
dark:bg-gray-900
```

Normal:

```text
text-black
```

Dark:

```text
dark:text-white
```

Think:

```text
normal class
     +
dark:
```

---

# 4️⃣6️⃣ PRACTICAL PROJECT 🛒

Let's create a Product Card.

```jsx
function ProductCard() {

    return (
        <div className="max-w-sm rounded-xl border border-gray-200 bg-white p-5 shadow-lg">

            <div className="h-40 rounded-lg bg-gray-200 flex items-center justify-center">
                Product Image
            </div>

            <h2 className="mt-4 text-xl font-bold">
                Laptop
            </h2>

            <p className="mt-2 text-gray-600">
                Powerful laptop for developers.
            </p>

            <p className="mt-3 text-2xl font-bold text-blue-600">
                ₹50,000
            </p>

            <button className="mt-4 w-full rounded-lg bg-blue-600 px-4 py-2 text-white transition hover:bg-blue-700">
                Buy Now
            </button>

        </div>
    );
}

export default ProductCard;
```

---

# 4️⃣7️⃣ UNDERSTAND THE PRODUCT CARD

This:

```text
max-w-sm
```

limits width.

```text
rounded-xl
```

rounds corners.

```text
border
```

adds border.

```text
p-5
```

adds padding.

```text
shadow-lg
```

adds shadow.

```text
mt-4
```

adds top margin.

```text
text-xl
```

controls text size.

```text
font-bold
```

makes text bold.

```text
w-full
```

makes button full width.

```text
hover:bg-blue-700
```

changes background on hover.

```text
transition
```

makes the visual change smoother.

---

# 4️⃣8️⃣ BUILD A PRODUCT GRID

```jsx
function App() {

    return (
        <div className="min-h-screen bg-gray-100 p-6">

            <h1 className="mb-6 text-center text-3xl font-bold">
                Products
            </h1>

            <div className="grid grid-cols-1 gap-6 sm:grid-cols-2 lg:grid-cols-3">

                <ProductCard />
                <ProductCard />
                <ProductCard />

            </div>

        </div>
    );
}
```

Now we have:

```text
Mobile
   ↓
1 card / row

Tablet
   ↓
2 cards / row

Desktop
   ↓
3 cards / row
```

---

# 4️⃣9️⃣ TAILWIND + PROPS

Tailwind becomes more powerful with React props.

```jsx
function ProductCard({
    name,
    price
}) {

    return (
        <div className="rounded-xl border p-5 shadow">

            <h2 className="text-xl font-bold">
                {name}
            </h2>

            <p className="text-blue-600">
                ₹{price}
            </p>

        </div>
    );
}
```

Use:

```jsx
<ProductCard
    name="Laptop"
    price={50000}
/>

<ProductCard
    name="Phone"
    price={25000}
/>
```

Same UI structure.

Different data.

---

# 5️⃣0️⃣ TAILWIND + CONDITIONAL STYLING

You can also use React conditions.

```jsx
function Status({ active }) {

    return (
        <span
            className={
                active
                    ? "rounded bg-green-100 px-3 py-1 text-green-700"
                    : "rounded bg-red-100 px-3 py-1 text-red-700"
            }
        >
            {active
                ? "Active"
                : "Inactive"}
        </span>
    );
}
```

Flow:

```text
active = true
     ↓
green classes
     ↓
Active
```

or:

```text
active = false
     ↓
red classes
     ↓
Inactive
```

---

# 5️⃣1️⃣ NORMAL CSS vs TAILWIND

### Normal CSS

```jsx
<button className="btn">
    Save
</button>
```

```css
.btn {
    background: blue;
    color: white;
    padding: 10px 20px;
    border-radius: 8px;
}
```

### Tailwind

```jsx
<button className="bg-blue-600 text-white px-5 py-2 rounded-lg">
    Save
</button>
```

---

# 5️⃣2️⃣ WHICH ONE SHOULD YOU USE?

There is no universal rule that Tailwind is always better.

### Normal CSS

Good when:

```text
You prefer traditional CSS
Large custom styles
Existing CSS architecture
```

### Tailwind

Good when:

```text
Rapid UI development
React applications
Reusable utility combinations
Responsive UI
Consistent design system
```

The important skill is understanding CSS itself first.

You learned normal CSS in Chapter 29.

Now Tailwind gives you another way to apply those concepts.

---

# 5️⃣3️⃣ COMMON MISTAKES ❌

## Mistake 1 — Mixing Tailwind versions

For example:

```text
Installing Tailwind v4
```

but following a:

```text
Tailwind v3 tutorial
```

This can cause setup errors.

---

## Mistake 2 — Forgetting `className`

Wrong:

```jsx
<div class="p-4">
```

Correct:

```jsx
<div className="p-4">
```

---

## Mistake 3 — Using normal CSS property names

Wrong:

```jsx
<div className="padding-20">
```

Tailwind doesn't work like ordinary CSS property naming.

You need Tailwind utilities:

```jsx
<div className="p-5">
```

---

## Mistake 4 — Thinking `p-5` means exactly 5 pixels

It does **not** mean:

```text
5px
```

Tailwind's spacing utilities use a predefined spacing scale.

So learn:

```text
p-1
p-2
p-4
p-6
...
```

as Tailwind spacing tokens.

---

## Mistake 5 — Huge class strings

This:

```jsx
className="..."
```

can become very long.

Don't panic.

Break your UI into components.

Example:

```text
Navbar
ProductCard
Button
Modal
Footer
```

---

# 5️⃣4️⃣ BEST PRACTICES ⭐

### ✅ 1. Learn CSS before depending on Tailwind

You already learned CSS in Chapter 29.

That's excellent.

Tailwind becomes much easier when you understand:

```text
Flexbox
Grid
Padding
Margin
Colors
Typography
Responsive design
```

---

### ✅ 2. Use reusable React components

Instead of repeating:

```text
100 utility classes
```

create:

```text
Button
Card
Navbar
Modal
```

---

### ✅ 3. Keep responsive design in mind

Use:

```text
sm:
md:
lg:
xl:
```

when needed.

---

### ✅ 4. Use consistent spacing

Prefer a consistent spacing system instead of random values everywhere.

---

### ✅ 5. Use semantic HTML

Tailwind controls appearance.

It does not replace good HTML structure.

---

# 5️⃣5️⃣ INTERVIEW QUESTIONS 🎤

## Q1. What is Tailwind CSS?

**Answer:**  
Tailwind CSS is a utility-first CSS framework that provides utility classes for styling UI directly in markup/JSX.

---

## Q2. What is a utility class?

**Answer:**  
A small predefined class that performs a specific styling task.

Example:

```text
text-center
```

---

## Q3. How do you use Tailwind in React?

**Answer:**

By placing Tailwind utility classes in JSX:

```jsx
<div className="p-4 bg-blue-500 text-white">
```

---

## Q4. What is responsive design in Tailwind?

**Answer:**  
Tailwind provides responsive variants such as:

```text
sm:
md:
lg:
xl:
2xl:
```

which allow styles to change at different breakpoints.

---

## Q5. What does `flex` do?

**Answer:**  
It applies a flexbox layout.

---

## Q6. What does `grid-cols-3` mean?

**Answer:**  
It defines a three-column grid.

---

## Q7. What does `hover:` mean?

**Answer:**  
It applies a utility when the element is hovered.

Example:

```text
hover:bg-blue-700
```

---

## Q8. What does `focus:` mean?

**Answer:**  
It applies a utility when an element is focused.

---

## Q9. What does `dark:` mean?

**Answer:**  
It applies a utility for dark-mode styling.

Example:

```text
dark:bg-gray-900
```

---

## Q10. Why is Tailwind called utility-first?

**Answer:**  
Because it provides many small utility classes that can be combined to build the final design.

---

# 5️⃣6️⃣ PRACTICE TASKS 📝

## Task 1 — Simple Button

Create:

```text
Save
```

Requirements:

```text
Blue background
White text
Padding
Rounded corners
Hover effect
```

---

## Task 2 — Profile Card

Create:

```text
Name
Role
City
Email
```

Use:

```text
Border
Shadow
Rounded corners
Spacing
Typography
```

---

## Task 3 — Responsive Grid

Create 6 cards.

Desktop:

```text
3 columns
```

Mobile:

```text
1 column
```

---

## Task 4 — Navbar

Create:

```text
Logo
Home
About
Services
Contact
```

Use:

```text
flex
items-center
justify-between
gap
```

---

## Task 5 — Dark Mode

Create a page with:

```text
Light background
Dark background
```

and appropriate text colors.

---

# 5️⃣7️⃣ MINI CHALLENGE 🚀

## 🛒 BUILD A RESPONSIVE PRODUCT PAGE

Create:

```text
ProductPage.jsx
```

Requirements:

### Header

```text
My Store
Home
Products
Contact
```

### Product grid

Create 6 products.

Each card:

```text
Product name
Price
Category
Status
Buy button
```

### Tailwind requirements

Use:

```text
flex
grid
gap
padding
margin
colors
typography
border
rounded
shadow
hover
transition
responsive breakpoints
```

### Responsive requirement

```text
Mobile
→ 1 column

Tablet
→ 2 columns

Desktop
→ 3 columns
```

---

# 5️⃣8️⃣ DRY RUN 🧠

Consider:

```jsx
<div className="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
```

Browser width is mobile.

```text
grid-cols-1
```

wins.

Result:

```text
1 column
```

Browser becomes medium.

```text
md:grid-cols-2
```

applies.

Result:

```text
2 columns
```

Browser becomes large.

```text
lg:grid-cols-3
```

applies.

Result:

```text
3 columns
```

Flow:

```text
Screen size
     ↓
Breakpoint
     ↓
Matching Tailwind utility
     ↓
CSS
     ↓
Responsive UI
```

---

# 5️⃣9️⃣ QUIZ 🧠

### Q1. What does Tailwind CSS provide?

**Answer:**  
Utility classes for styling UI.

---

### Q2. What is the React attribute used for Tailwind classes?

**Answer:**

```jsx
className
```

---

### Q3. What does `p-4` represent?

**Answer:**  
A Tailwind padding utility using the framework's spacing scale.

---

### Q4. What does `flex` do?

**Answer:**  
Enables a flexbox layout.

---

### Q5. What does `grid-cols-3` mean?

**Answer:**  
A grid with three columns.

---

### Q6. What does `gap-4` do?

**Answer:**  
Adds gap/spacing between flex or grid items.

---

### Q7. What does `hover:bg-blue-700` mean?

**Answer:**  
Apply a blue background utility when the element is hovered.

---

### Q8. What does `md:grid-cols-2` mean?

**Answer:**  
At the `md` breakpoint and above, use two grid columns.

---

### Q9. What does `dark:bg-gray-900` mean?

**Answer:**  
Use the gray-900 background in dark mode.

---

### Q10. What is the current Tailwind v4 Vite installation approach?

**Answer:**

```bash
npm install tailwindcss @tailwindcss/vite
```

Then configure the Vite plugin and import:

```css
@import "tailwindcss";
```

This follows the current official Tailwind Vite setup.

---

# 🧠 FINAL MEMORY TRICK

Remember Tailwind in this order:

```text
🎨 COLORS
   ↓
📏 SPACING
   ↓
✍️ TYPOGRAPHY
   ↓
🔲 BORDER
   ↓
🌑 SHADOW
   ↓
🧩 FLEX
   ↓
📊 GRID
   ↓
📱 RESPONSIVE
   ↓
🖱️ HOVER
   ↓
🎯 FOCUS
   ↓
✨ TRANSITION
   ↓
🎬 ANIMATION
   ↓
🌙 DARK MODE
```

And remember the most important Tailwind pattern:

```text
Utility
   +
Utility
   +
Utility
   +
Utility
   ↓
Complete UI
```

For example:

```jsx
<button className="
    bg-blue-600
    text-white
    px-5
    py-2
    rounded-lg
    shadow
    hover:bg-blue-700
    transition
">
    Save
</button>
```

---

# ⚡ QUICK REVISION

```text
Tailwind
→ Utility-first CSS

bg-blue-600
→ Background

text-white
→ Text color

p-4
→ Padding

m-4
→ Margin

flex
→ Flexbox

grid
→ Grid

grid-cols-3
→ 3 columns

gap-4
→ Gap

rounded-lg
→ Rounded corners

shadow-lg
→ Shadow

hover:
→ Hover state

focus:
→ Focus state

sm / md / lg / xl
→ Responsive variants

dark:
→ Dark mode

transition
→ Smooth transition
```

---

# ✅ CHAPTER 30 CHECKLIST

Make sure you understand:

- [ ] What Tailwind CSS is
- [ ] Why Tailwind is useful
- [ ] Utility classes
- [ ] Tailwind + Vite setup
- [ ] Colors
- [ ] Spacing
- [ ] Typography
- [ ] Borders
- [ ] Rounded corners
- [ ] Shadows
- [ ] Width
- [ ] Height
- [ ] Flexbox
- [ ] Grid
- [ ] Gap
- [ ] Responsive design
- [ ] Hover
- [ ] Focus
- [ ] Transitions
- [ ] Animations
- [ ] Dark mode
- [ ] Tailwind + React props
- [ ] Tailwind + React state
- [ ] Conditional Tailwind classes

# 🎉 CHAPTER 30 COMPLETE

You now know the basic Tailwind skills needed to build modern React interfaces.

The next chapter is:

# ⏭️ CHAPTER 31 — COMPONENT LIBRARIES 🧱

We will learn:

```text
DaisyUI
Material UI
Bootstrap
Lucide React
Reusable UI Components
When to use Component Libraries
```

These are the Chapter 31 topics in your React course.

Type **`NEXT`** when you are ready.