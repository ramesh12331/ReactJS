# ⚛️ REACTJS — CHAPTER 4
# 🧩 JSX — JavaScript XML

---

## 🎯 Chapter Goal

After this chapter, you should understand:

- 🧩 What JSX is
- 🤔 Why React uses JSX
- ✍️ JSX syntax
- `{}` curly braces
- 🟨 JavaScript inside JSX
- 🏷️ JSX attributes
- 🎨 `className`
- 🖌️ Inline styles
- 📦 Multiple elements
- 🧩 Fragments
- 📏 JSX rules
- ❌ Common JSX mistakes

---

# 1️⃣ What is JSX?

## 🧠 Simple Definition

**JSX is a syntax extension used with JavaScript that allows us to describe UI using HTML-like syntax inside JavaScript.**

For example:

```jsx
function App() {
    return <h1>Hello React</h1>;
}
```

This:

```jsx
<h1>Hello React</h1>
```

looks like HTML.

But inside a React component, it is **JSX**, not ordinary HTML.

Your course specifically introduces JSX as HTML-like UI syntax used inside React components.

---

# 2️⃣ Why Do We Need JSX?

Without JSX, React UI can be written using JavaScript function calls.

For example, conceptually:

```javascript
React.createElement(
    "h1",
    null,
    "Hello React"
);
```

This is harder for a beginner to read.

With JSX:

```jsx
<h1>Hello React</h1>
```

It is much easier to understand.

So JSX gives us a convenient way to describe UI.

### 🧠 Remember

```text
JavaScript
     +
JSX syntax
     ↓
Easy-to-read React UI
```

---

# 3️⃣ Real-World Analogy 🏠

Imagine you are building a house.

Without JSX, you might explain every small building instruction separately.

With JSX, you can describe the structure more naturally:

```text
🏠 House
 ├── 🚪 Door
 ├── 🪟 Window
 └── 🛋️ Room
```

Similarly, JSX lets us describe UI:

```jsx
<div>
    <h1>My Website</h1>
    <p>Welcome!</p>
</div>
```

It is easier to see the UI structure.

---

# 4️⃣ Basic JSX Syntax

The simplest JSX is:

```jsx
<h1>Hello React</h1>
```

Inside a component:

```jsx
function App() {
    return (
        <h1>Hello React</h1>
    );
}

export default App;
```

### Output

```text
Hello React
```

---

# 5️⃣ JSX Looks Like HTML

You already know HTML:

```html
<h1>Hello World</h1>
<p>Welcome to my website</p>
```

In React, we can write:

```jsx
<h1>Hello World</h1>
<p>Welcome to my website</p>
```

But remember:

> ⚠️ JSX looks like HTML, but JSX is used inside JavaScript/React code.

---

# 6️⃣ JSX + JavaScript

This is where JSX becomes powerful.

Suppose we have:

```jsx
const name = "Ramesh";
```

We can display the variable inside JSX.

```jsx
function App() {

    const name = "Ramesh";

    return (
        <h1>Hello {name}</h1>
    );
}

export default App;
```

### Output

```text
Hello Ramesh
```

---

# 7️⃣ Curly Braces `{}` ⭐

This is one of the most important JSX concepts.

We use:

```text
{}
```

to put JavaScript expressions inside JSX.

Example:

```jsx
const name = "Ramesh";

<h1>Hello {name}</h1>
```

Here:

```text
{name}
```

means:

> "Get the value of the JavaScript variable `name`."

---

# 8️⃣ Example — Number

```jsx
function App() {

    const age = 25;

    return (
        <h1>My age is {age}</h1>
    );
}

export default App;
```

### Output

```text
My age is 25
```

---

# 9️⃣ Example — Calculation

You can also use JavaScript expressions.

```jsx
function App() {

    const a = 10;
    const b = 20;

    return (
        <h1>Total = {a + b}</h1>
    );
}

export default App;
```

### Output

```text
Total = 30
```

### 🧠 Dry Run

```text
a = 10
b = 20

a + b
 ↓
10 + 20
 ↓
30
```

Then JSX displays:

```text
Total = 30
```

---

# 🔟 What Can We Put Inside `{}`?

We can use JavaScript **expressions**.

Examples:

```jsx
{name}
```

```jsx
{age}
```

```jsx
{10 + 20}
```

```jsx
{name.toUpperCase()}
```

```jsx
{isLoggedIn ? "Logout" : "Login"}
```

---

# 1️⃣1️⃣ JavaScript String Methods

Example:

```jsx
function App() {

    const name = "ramesh";

    return (
        <h1>{name.toUpperCase()}</h1>
    );
}

export default App;
```

### Output

```text
RAMESH
```

Because:

```text
"ramesh"
    ↓
.toUpperCase()
    ↓
"RAMESH"
```

---

# 1️⃣2️⃣ JSX Expressions vs Statements

This is an important beginner concept.

Inside `{}`, JSX expects an **expression**.

### ✅ Expression

```jsx
{name}
```

```jsx
{10 + 20}
```

```jsx
{user.name}
```

```jsx
{isLoggedIn ? "Welcome" : "Login"}
```

### ❌ Statement

For example, you cannot directly put a normal `if` statement inside JSX like this:

```jsx
<h1>
    {if (age > 18)}
</h1>
```

❌ This is invalid.

Later we will learn different ways to use conditions in React.

---

# 1️⃣3️⃣ JSX Attributes

HTML has attributes:

```html
<img src="photo.jpg">
```

JSX also has attributes:

```jsx
<img src="photo.jpg" />
```

Example:

```jsx
function App() {

    return (
        <img
            src="https://example.com/photo.jpg"
            alt="Example"
        />
    );
}

export default App;
```

Common JSX attributes include:

```text
src
alt
id
className
href
title
```

---

# 1️⃣4️⃣ `className` ⭐

This is a very important JSX rule.

In HTML, we normally write:

```html
<div class="container">
```

In JSX, we write:

```jsx
<div className="container">
```

### ❌ HTML style

```jsx
<div class="container">
```

### ✅ JSX style

```jsx
<div className="container">
```

Why?

Because JSX uses JavaScript syntax conventions, and `class` has a special meaning in JavaScript.

So React uses:

```text
className
```

---

# 1️⃣5️⃣ CSS Example

### `App.jsx`

```jsx
function App() {

    return (
        <h1 className="title">
            Hello React
        </h1>
    );
}

export default App;
```

### `App.css`

```css
.title {
    color: blue;
    font-size: 40px;
}
```

Then the browser displays:

```text
Hello React
```

with the CSS applied.

---

# 1️⃣6️⃣ Dynamic Attributes

We can also put JavaScript values inside attributes.

Example:

```jsx
function App() {

    const imageUrl = "https://example.com/image.jpg";

    return (
        <img
            src={imageUrl}
            alt="Example"
        />
    );
}

export default App;
```

Notice:

```jsx
src={imageUrl}
```

The `{}` tells JSX:

> "Use the JavaScript variable."

---

# 1️⃣7️⃣ String vs JavaScript Value

Compare these carefully.

### Normal string

```jsx
src="photo.jpg"
```

This directly gives the text:

```text
photo.jpg
```

### JavaScript variable

```jsx
src={imageUrl}
```

This means:

```text
Get the value stored in imageUrl
```

---

# 1️⃣8️⃣ Inline Styles

We can write styles directly inside JSX.

Example:

```jsx
function App() {

    return (
        <h1 style={{ color: "blue" }}>
            Hello React
        </h1>
    );
}

export default App;
```

Notice this:

```jsx
style={{ color: "blue" }}
```

There are **two curly braces**.

### First `{}`

The first braces mean:

> JavaScript expression

### Second `{}`

The second braces represent the JavaScript object:

```javascript
{
    color: "blue"
}
```

So:

```jsx
style={{ color: "blue" }}
```

means:

```text
JSX
 ↓
JavaScript expression
 ↓
JavaScript object
 ↓
CSS style
```

---

# 1️⃣9️⃣ Multiple Inline Styles

```jsx
function App() {

    return (
        <h1
            style={{
                color: "blue",
                fontSize: "40px",
                backgroundColor: "lightgray"
            }}
        >
            Hello React
        </h1>
    );
}

export default App;
```

### Important

CSS:

```css
font-size
```

becomes JSX style:

```jsx
fontSize
```

CSS:

```css
background-color
```

becomes:

```jsx
backgroundColor
```

This is called **camelCase**.

---

# 2️⃣0️⃣ Multiple Elements

Suppose we want:

```text
Hello React
Welcome to my website
```

We might write:

```jsx
function App() {
    return (
        <h1>Hello React</h1>
        <p>Welcome to my website</p>
    );
}
```

❌ This causes an error.

Why?

Because a React component must return one parent JSX structure.

---

# 2️⃣1️⃣ Correct Multiple Elements

Wrap them inside a parent:

```jsx
function App() {
    return (
        <div>
            <h1>Hello React</h1>
            <p>Welcome to my website</p>
        </div>
    );
}

export default App;
```

### Output

```text
Hello React

Welcome to my website
```

The parent is:

```jsx
<div>
```

---

# 2️⃣2️⃣ React Fragment

Sometimes we don't want to add an unnecessary `<div>`.

We can use a **Fragment**.

Short syntax:

```jsx
<>
    <h1>Hello React</h1>
    <p>Welcome!</p>
</>
```

Example:

```jsx
function App() {

    return (
        <>
            <h1>Hello React</h1>
            <p>Welcome to my website</p>
        </>
    );
}

export default App;
```

### Output

```text
Hello React
Welcome to my website
```

---

# 2️⃣3️⃣ Why Use Fragment?

Suppose you write:

```jsx
<div>
    <h1>Hello</h1>
    <p>Welcome</p>
</div>
```

The `<div>` becomes an actual element in the DOM.

But:

```jsx
<>
    <h1>Hello</h1>
    <p>Welcome</p>
</>
```

allows you to group elements without adding an extra DOM element.

### 🧠 Memory Trick

```text
<div>
    = real HTML element

<>
</>
    = grouping without extra DOM element
```

---

# 2️⃣4️⃣ JSX Rule — Close Tags

HTML sometimes allows certain tags to be written without explicit closing syntax.

In JSX, elements must be properly closed.

For example:

```jsx
<img src="photo.jpg" />
```

Notice:

```text
/>
```

not simply:

```text
>
```

Similarly:

```jsx
<input type="text" />
```

---

# 2️⃣5️⃣ JSX Rule — Correct Nesting

### ❌ Incorrect

```jsx
<div>
    <h1>Hello</div>
</h1>
```

The tags are incorrectly nested.

### ✅ Correct

```jsx
<div>
    <h1>Hello</h1>
</div>
```

Always keep JSX properly nested.

---

# 2️⃣6️⃣ JSX Rule — JavaScript Expressions Use `{}`

Suppose:

```jsx
const name = "Ramesh";
```

### ❌ Wrong

```jsx
<h1>Hello name</h1>
```

Output:

```text
Hello name
```

It treats `name` as text.

### ✅ Correct

```jsx
<h1>Hello {name}</h1>
```

Output:

```text
Hello Ramesh
```

---

# 2️⃣7️⃣ JSX Rule — Use `className`

### ❌ Wrong

```jsx
<div class="box">
```

### ✅ Correct

```jsx
<div className="box">
```

Remember:

```text
HTML → class

JSX → className
```

---

# 2️⃣8️⃣ Practical Example — User Profile

Let's build a small profile.

```jsx
function App() {

    const name = "Ramesh";
    const age = 25;
    const city = "Hyderabad";

    return (
        <div>
            <h1>My Profile</h1>

            <p>Name: {name}</p>
            <p>Age: {age}</p>
            <p>City: {city}</p>
        </div>
    );
}

export default App;
```

### Output

```text
My Profile

Name: Ramesh
Age: 25
City: Hyderabad
```

---

# 2️⃣9️⃣ Dry Run 🧠

JavaScript variables:

```text
name = "Ramesh"
age = 25
city = "Hyderabad"
```

JSX:

```jsx
<p>Name: {name}</p>
```

React reads:

```text
{name}
 ↓
"Ramesh"
```

Result:

```text
Name: Ramesh
```

Next:

```jsx
<p>Age: {age}</p>
```

becomes:

```text
Age: 25
```

Next:

```jsx
<p>City: {city}</p>
```

becomes:

```text
City: Hyderabad
```

---

# 3️⃣0️⃣ Practical Example — Product

Now let's use an object.

```jsx
function App() {

    const product = {
        name: "Laptop",
        price: 50000,
        brand: "Dell"
    };

    return (
        <div>
            <h1>{product.name}</h1>

            <p>Brand: {product.brand}</p>

            <p>Price: ₹{product.price}</p>
        </div>
    );
}

export default App;
```

### Output

```text
Laptop
Brand: Dell
Price: ₹50000
```

---

# 3️⃣1️⃣ JavaScript Inside JSX

You can use many JavaScript expressions.

### Addition

```jsx
<p>{10 + 20}</p>
```

Output:

```text
30
```

### Multiplication

```jsx
<p>{10 * 5}</p>
```

Output:

```text
50
```

### String method

```jsx
<p>{"hello".toUpperCase()}</p>
```

Output:

```text
HELLO
```

### Array length

```jsx
const users = ["A", "B", "C"];

<p>{users.length}</p>
```

Output:

```text
3
```

---

# 3️⃣2️⃣ Important: JSX Is Not a String

This:

```jsx
<h1>Hello</h1>
```

is not simply a string.

It describes a UI element.

And this:

```jsx
<h1>{name}</h1>
```

combines:

```text
JSX
 +
JavaScript value
```

This is one of the main reasons JSX is useful in React.

---

# 3️⃣3️⃣ Common Beginner Mistakes ❌

## Mistake 1 — Using `class`

```jsx
<div class="container">
```

❌ Wrong JSX syntax.

Use:

```jsx
<div className="container">
```

---

## Mistake 2 — Forgetting `{}`

```jsx
const name = "Ramesh";

<h1>Hello name</h1>
```

Output:

```text
Hello name
```

Correct:

```jsx
<h1>Hello {name}</h1>
```

---

## Mistake 3 — Multiple root elements

❌

```jsx
return (
    <h1>Hello</h1>
    <p>Welcome</p>
);
```

Correct:

```jsx
return (
    <>
        <h1>Hello</h1>
        <p>Welcome</p>
    </>
);
```

---

## Mistake 4 — Not closing elements

❌

```jsx
<img src="photo.jpg">
```

Correct:

```jsx
<img src="photo.jpg" />
```

---

## Mistake 5 — Incorrect nesting

❌

```jsx
<div>
    <p>Hello</div>
</p>
```

Correct:

```jsx
<div>
    <p>Hello</p>
</div>
```

---

# 3️⃣4️⃣ Best Practices ⭐

### ✅ 1. Keep JSX readable

Instead of:

```jsx
<div><h1>Hello</h1><p>Welcome</p></div>
```

prefer:

```jsx
<div>
    <h1>Hello</h1>
    <p>Welcome</p>
</div>
```

---

### ✅ 2. Use meaningful variable names

Good:

```jsx
const userName = "Ramesh";
```

Avoid unclear names:

```jsx
const x = "Ramesh";
```

when the variable represents a user name.

---

### ✅ 3. Use `className`

```jsx
<div className="container">
```

---

### ✅ 4. Use components to organize large UI

Don't put a huge application inside one JSX block.

Later we will learn:

```text
App
 ├── Navbar
 ├── Sidebar
 ├── ProductList
 │    └── ProductCard
 └── Footer
```

---

# 3️⃣5️⃣ Interview Questions 🎤

## Beginner

### Q1. What is JSX?

**Answer:**

JSX is a syntax extension commonly used with React that allows developers to describe UI using HTML-like syntax inside JavaScript.

---

### Q2. Is JSX HTML?

**Answer:**

No.

JSX looks similar to HTML, but it is used inside JavaScript/React code to describe UI.

---

### Q3. Why do we use JSX?

**Answer:**

JSX makes React UI easier to write and understand by allowing HTML-like syntax inside JavaScript.

---

### Q4. How do you put a JavaScript variable inside JSX?

**Answer:**

Use curly braces:

```jsx
<h1>Hello {name}</h1>
```

---

## Intermediate

### Q5. Why do we use `className` instead of `class`?

**Answer:**

In JSX, CSS classes are specified using `className`.

```jsx
<div className="container">
```

---

### Q6. Can a React component return multiple elements?

**Answer:**

Yes, but they must be wrapped in one parent structure, such as a `<div>` or Fragment.

Example:

```jsx
<>
    <h1>Hello</h1>
    <p>Welcome</p>
</>
```

---

### Q7. What is a Fragment?

**Answer:**

A Fragment lets us group multiple JSX elements without adding an extra DOM element.

---

## Advanced Beginner

### Q8. What can be placed inside JSX curly braces?

**Answer:**

JavaScript expressions such as:

```jsx
{name}
```

```jsx
{10 + 20}
```

```jsx
{user.name}
```

and other valid expressions.

---

### Q9. Why is this invalid?

```jsx
<h1>
    {if (age > 18)}
</h1>
```

Because `if` is a statement, not an expression that can be directly placed inside JSX braces.

---

### Q10. What is the difference between these?

```jsx
<h1>name</h1>
```

and:

```jsx
<h1>{name}</h1>
```

The first displays the literal text:

```text
name
```

The second displays the value stored in the JavaScript variable `name`.

---

# 🧪 3️⃣6️⃣ Practice Questions

Try these yourself.

## Practice 1 — Name

Create:

```text
Hello Ramesh
```

using a variable.

---

## Practice 2 — Age

Create:

```text
My age is 25
```

using a variable.

---

## Practice 3 — Calculation

Display:

```text
Total = 150
```

using:

```text
100 + 50
```

inside JSX.

---

## Practice 4 — Profile

Create:

```text
Name: Ramesh
Age: 25
City: Hyderabad
```

using JavaScript variables.

---

## Practice 5 — Product

Create:

```text
Product: Laptop
Price: ₹50000
```

using a JavaScript object.

---

# 🏆 3️⃣7️⃣ Mini Challenge

Create a simple **Student Profile**.

Your output should look like:

```text
-------------------------
     Student Profile
-------------------------

Name: Ramesh
Age: 25
Course: ReactJS
City: Hyderabad

-------------------------
```

### Requirements

Create variables:

```jsx
const name = "Ramesh";
const age = 25;
const course = "ReactJS";
const city = "Hyderabad";
```

Then display them using JSX.

### 🎯 Don't copy the complete solution.

Try writing it yourself first.

---

# 📝 3️⃣8️⃣ 10-Question Quiz

## Q1. What does JSX stand for?

### ✅ Answer

JSX is commonly expanded as **JavaScript XML**.

---

## Q2. Is JSX exactly the same as HTML?

### ✅ Answer

No.

It looks similar to HTML but is used within JavaScript/React to describe UI.

---

## Q3. How do you display a variable in JSX?

### ✅ Answer

Using curly braces:

```jsx
{name}
```

---

## Q4. Which is correct?

```jsx
<div class="box">
```

or:

```jsx
<div className="box">
```

### ✅ Answer

```jsx
<div className="box">
```

---

## Q5. What happens here?

```jsx
<h1>{10 + 20}</h1>
```

### ✅ Answer

It displays:

```text
30
```

---

## Q6. Which is correct for an image?

### A

```jsx
<img src="photo.jpg">
```

### B

```jsx
<img src="photo.jpg" />
```

### ✅ Answer

**B**

```jsx
<img src="photo.jpg" />
```

---

## Q7. Why do we use Fragment?

### ✅ Answer

To group multiple JSX elements without adding an extra DOM element.

---

## Q8. What does this display?

```jsx
const name = "Ramesh";

<h1>Hello name</h1>
```

### ✅ Answer

```text
Hello name
```

Because `name` is treated as normal text.

---

## Q9. What does this display?

```jsx
const name = "Ramesh";

<h1>Hello {name}</h1>
```

### ✅ Answer

```text
Hello Ramesh
```

---

## Q10. Which one is valid?

### A

```jsx
return (
    <h1>Hello</h1>
    <p>Welcome</p>
);
```

### B

```jsx
return (
    <>
        <h1>Hello</h1>
        <p>Welcome</p>
    </>
);
```

### ✅ Answer

**B**

Because the elements are wrapped inside a Fragment.

---

# 📚 3️⃣9️⃣ Chapter Summary

Today you learned:

```text
🧩 JSX
│
├── HTML-like syntax
│
├── Used inside React
│
├── JavaScript expressions
│      └── { }
│
├── JSX attributes
│      ├── className
│      ├── src
│      ├── alt
│      └── href
│
├── Inline styles
│
├── Multiple elements
│
├── Fragments
│
└── JSX rules
```

---

# 🧠 4️⃣0️⃣ Quick Revision

### Remember these 8 rules:

```text
1️⃣ JSX looks like HTML
2️⃣ JSX is used inside JavaScript/React
3️⃣ JavaScript values → { }
4️⃣ HTML class → className
5️⃣ JSX elements must be properly closed
6️⃣ JSX must be correctly nested
7️⃣ Multiple elements need one parent/Fragment
8️⃣ Fragment → <> ... </>
```

### ⭐ Most important memory trick

> **JSX = UI-looking syntax + JavaScript expressions**

Example:

```jsx
const name = "Ramesh";

return (
    <h1>Hello {name}</h1>
);
```

Think:

```text
<h1>          → UI
{name}        → JavaScript
```

---

# 🎯 Chapter 4 Checklist

Before continuing, make sure you can explain:

- [ ] What JSX is
- [ ] Why JSX is used
- [ ] JSX syntax
- [ ] `{}` curly braces
- [ ] JavaScript expressions
- [ ] JSX attributes
- [ ] `className`
- [ ] Inline styles
- [ ] Multiple JSX elements
- [ ] Fragments
- [ ] JSX rules
- [ ] Common JSX errors
- [ ] How to create a simple JSX UI

---

# 🚀 NEXT CHAPTER

## 🧩 CHAPTER 5 — COMPONENTS

We will learn:

```text
What is a Component?
        ↓
Why Components?
        ↓
Functional Components
        ↓
Component Naming
        ↓
Component Structure
        ↓
Reusable Components
        ↓
Parent & Child Components
        ↓
Component Composition
        ↓
Component Hierarchy
        ↓
Real-world Component Architecture
```

Your uploaded syllabus places **Components immediately after JSX**.

🛑 **STOP HERE.**

Type **`NEXT`** when you are ready for **Chapter 5 — Components**.