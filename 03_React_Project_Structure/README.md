# 🚀 ReactJS — Chapter 3: Project Structure

## 🎯 Chapter Goal

After completing this chapter, you should understand:

- 📂 React project folders
- 📄 Important React files
- 🧩 `src` folder
- ⚛️ `App.jsx`
- 🚪 `main.jsx`
- 🌐 `index.html`
- 📦 `package.json`
- 🔒 `package-lock.json`
- ⚙️ `vite.config.js`
- 🚫 `.gitignore`
- 🔄 How React files connect together
- 🧠 Which files you normally edit
- ❌ Which folders you should not edit manually

---

# 🗺️ 1. React Project Structure

When we create a React project using Vite, you will see a structure similar to this:

```text
react-learning/
│
├── node_modules/
│
├── public/
│
├── src/
│   ├── assets/
│   ├── App.css
│   ├── App.jsx
│   ├── index.css
│   └── main.jsx
│
├── .gitignore
├── eslint.config.js
├── index.html
├── package.json
├── package-lock.json
└── vite.config.js
```

⚠️ Your Vite version may generate slightly different files.

Don't worry if your project does not look exactly the same.

The important thing is to understand the **purpose of each file**.

---

# 🧠 2. First Understand the Big Picture

Think about a React project like a house 🏠.

```text
🏠 React Project
│
├── 📂 node_modules → Tools/packages
│
├── 📂 public → Public/static files
│
├── 📂 src → Main application code
│
├── 📄 index.html → Main HTML page
│
├── 📄 package.json → Project information
│
├── 📄 package-lock.json → Exact package versions
│
└── ⚙️ vite.config.js → Vite configuration
```

The most important folder for you as a React developer is:

```text
📂 src
```

Most of your React coding will happen inside `src`.

---

# 📂 3. `node_modules`

```text
node_modules/
```

### 🤔 What is it?

`node_modules` contains the packages/dependencies installed for your project.

For example:

```text
React
React DOM
Vite
ESLint
```

and many packages required by them.

---

## 🧠 Simple Example

Suppose you run:

```bash
npm install
```

npm reads your project dependencies and downloads the required packages into:

```text
node_modules/
```

---

## ⚠️ Important

Normally:

❌ Don't manually edit files inside `node_modules`.

You don't need to understand every file inside it.

Think:

```text
package.json
     ↓
npm install
     ↓
node_modules
     ↓
Installed packages
```

---

# 📂 4. `public`

```text
public/
```

This folder is used for static files that you want to serve directly.

Examples:

```text
public/
│
├── logo.png
├── favicon.ico
└── images/
```

You can place files here when you want them available directly.

For example:

```text
public/logo.png
```

can be referenced as:

```jsx
<img src="/logo.png" alt="Logo" />
```

---

# 🧠 5. `public` vs `src/assets`

This is important.

You may have:

```text
public/
```

and:

```text
src/assets/
```

Both can contain images, but they are used differently.

### 📂 public

Static files that can be accessed directly.

```text
public/logo.png
```

Usage:

```jsx
<img src="/logo.png" alt="Logo" />
```

### 📂 src/assets

Assets that are part of your React source code.

Example:

```text
src/assets/logo.png
```

Usage:

```jsx
import logo from "./assets/logo.png";

<img src={logo} alt="Logo" />
```

For now, remember:

> 📂 `public` → direct/static files  
> 📂 `src/assets` → assets imported into React code

---

# ⭐ 6. `src` — MOST IMPORTANT FOLDER

```text
src/
```

`src` means:

> **Source**

This is where your main application code lives.

Example:

```text
src/
│
├── assets/
├── App.css
├── App.jsx
├── index.css
└── main.jsx
```

Later your project may become:

```text
src/
│
├── components/
├── pages/
├── hooks/
├── services/
├── context/
├── utils/
├── assets/
│
├── App.jsx
└── main.jsx
```

Don't worry about these folders now.

We will learn them step-by-step.

---

# 🧩 7. `App.jsx`

This is one of the most important files.

```text
src/App.jsx
```

It contains the main `App` component.

Example:

```jsx
function App() {
    return (
        <h1>Hello React</h1>
    );
}

export default App;
```

---

# 🔍 Understanding `App.jsx`

Let's break it down.

```jsx
function App() {
```

We create a React component called:

```text
App
```

Then:

```jsx
return (
    <h1>Hello React</h1>
);
```

The component returns UI.

Finally:

```jsx
export default App;
```

We make the component available to other files.

---

# 🧠 Simple Analogy

Think of:

```text
App.jsx
```

as the **main room of your application** 🏠.

Later, that room can contain other components:

```text
App
│
├── Navbar
├── Sidebar
├── Home
├── Products
└── Footer
```

---

# 🚪 8. `main.jsx`

Now we have:

```text
src/main.jsx
```

This file is very important.

It is the entry point where React starts rendering your application.

Example modern Vite structure:

```jsx
import { StrictMode } from 'react'
import { createRoot } from 'react-dom/client'
import './index.css'
import App from './App.jsx'

createRoot(document.getElementById('root')).render(
    <StrictMode>
        <App />
    </StrictMode>,
)
```

Don't worry if this looks difficult.

We will understand it slowly.

---

# 🔍 9. Understand `main.jsx` Step-by-Step

## Step 1 — Import React rendering function

```jsx
import { createRoot } from 'react-dom/client'
```

`createRoot()` is used to create the React root.

---

## Step 2 — Import CSS

```jsx
import './index.css'
```

This loads the global CSS.

---

## Step 3 — Import App

```jsx
import App from './App.jsx'
```

We are importing:

```text
App.jsx
```

into:

```text
main.jsx
```

---

## Step 4 — Find the root element

```jsx
document.getElementById('root')
```

React looks for an HTML element with:

```html
id="root"
```

---

## Step 5 — Render App

```jsx
<App />
```

This tells React:

> "Render the App component here."

---

# 🌐 10. `index.html`

Now let's look at:

```text
index.html
```

You will usually see something similar to:

```html
<!doctype html>

<html lang="en">

<head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>React App</title>
</head>

<body>

    <div id="root"></div>

    <script type="module" src="/src/main.jsx"></script>

</body>

</html>
```

The important part for now is:

```html
<div id="root"></div>
```

This is where React attaches the application.

---

# 🔄 11. How React Starts

This is one of the **most important concepts in this chapter**.

Remember this flow:

```text
🌐 index.html
      ↓
<div id="root"></div>
      ↓
📄 main.jsx
      ↓
⚛️ <App />
      ↓
📄 App.jsx
      ↓
🧩 Components
      ↓
🖥️ Browser UI
```

Let's understand it with a simple example.

---

# 🧪 12. Complete Flow Example

### `index.html`

```html
<div id="root"></div>
```

↓

### `main.jsx`

```jsx
createRoot(document.getElementById('root')).render(
    <App />
);
```

↓

### `App.jsx`

```jsx
function App() {
    return <h1>Hello React</h1>;
}

export default App;
```

↓

### Browser

```text
Hello React
```

---

# 📄 13. `index.css`

You may see:

```text
src/index.css
```

This is generally used for global CSS styles.

Example:

```css
body {
    margin: 0;
    font-family: Arial, sans-serif;
}
```

This CSS can affect the overall application.

---

# 🎨 14. `App.css`

You may also see:

```text
src/App.css
```

This file can contain styles related to the `App` component.

Example:

```css
.title {
    font-size: 30px;
}
```

Then:

```jsx
<h1 className="title">
    Hello React
</h1>
```

---

# 📦 15. `package.json`

This is another **very important file**.

```text
package.json
```

It contains project information and dependencies.

Example:

```json
{
    "name": "react-learning",
    "version": "0.0.0",
    "scripts": {
        "dev": "vite",
        "build": "vite build",
        "preview": "vite preview"
    },
    "dependencies": {
        "react": "...",
        "react-dom": "..."
    }
}
```

Your exact versions may be different.

---

# 🧠 What does `package.json` tell us?

It can contain:

### 📌 Project name

```json
"name": "react-learning"
```

### 📌 Version

```json
"version": "0.0.0"
```

### 📌 Scripts

```json
"scripts": {
    "dev": "vite",
    "build": "vite build"
}
```

### 📌 Dependencies

```json
"dependencies": {
    "react": "...",
    "react-dom": "..."
}
```

---

# ▶️ 16. npm Scripts

Because `package.json` contains:

```json
"dev": "vite"
```

we can run:

```bash
npm run dev
```

This executes:

```text
vite
```

So:

```text
npm run dev
      ↓
package.json
      ↓
"dev": "vite"
      ↓
Vite starts
      ↓
React development server
```

---

# 🔒 17. `package-lock.json`

You may see:

```text
package-lock.json
```

This file stores the exact dependency information generated by npm.

It helps npm install a consistent dependency tree.

Think:

```text
package.json
      ↓
What packages does the project need?

package-lock.json
      ↓
What exact dependency versions/tree were resolved?
```

### 🧠 Simple memory trick

```text
package.json
    =
Project requirements 📋

package-lock.json
    =
Locked dependency details 🔒
```

---

# ⚙️ 18. `vite.config.js`

You may see:

```text
vite.config.js
```

This contains Vite configuration.

For example:

```js
import { defineConfig } from 'vite'
import react from '@vitejs/plugin-react'

export default defineConfig({
    plugins: [react()],
})
```

Don't worry about understanding every line yet.

We will use Vite configuration later when needed.

For now:

> ⚙️ `vite.config.js` → configuration for Vite.

---

# 🚫 19. `.gitignore`

You may see:

```text
.gitignore
```

This tells Git which files/folders should not be tracked.

For example:

```text
node_modules/
dist/
.env
```

The exact content can vary.

---

# 🧠 Why ignore `node_modules`?

Because:

```text
node_modules
```

can contain a huge number of files.

Instead of uploading it to GitHub, we normally keep dependency information in:

```text
package.json
package-lock.json
```

Then another developer can run:

```bash
npm install
```

to install dependencies.

---

# 🧹 20. `eslint.config.js`

You may see:

```text
eslint.config.js
```

ESLint helps detect coding problems and enforce coding rules.

For example, it can warn about:

- unused variables
- some coding mistakes
- incorrect patterns
- style/rule violations

For now, simply remember:

```text
ESLint
   ↓
Checks JavaScript/React code
   ↓
Helps find problems
```

---

# ⭐ 21. Which Files Will YOU Mostly Edit?

As a beginner, you will mostly work inside:

```text
src/
```

Especially:

```text
App.jsx
main.jsx
index.css
App.css
```

Later you will create:

```text
components/
pages/
hooks/
services/
utils/
```

For example:

```text
src/
│
├── components/
│
├── pages/
│
├── services/
│
├── hooks/
│
├── App.jsx
└── main.jsx
```

---

# 🏗️ 22. Real Project Structure

A larger React application might look like:

```text
src/
│
├── assets/
│
├── components/
│   ├── Navbar.jsx
│   ├── Footer.jsx
│   └── Button.jsx
│
├── pages/
│   ├── Home.jsx
│   ├── About.jsx
│   └── Products.jsx
│
├── services/
│   └── api.js
│
├── hooks/
│   └── useProducts.js
│
├── App.jsx
├── main.jsx
└── index.css
```

Don't try to memorize this now.

We will create these folders when we learn the related concepts.

---

# 🔄 23. Complete React Flow

Let's make the whole thing very simple.

```text
             🌐 Browser
                 │
                 ↓
          📄 index.html
                 │
                 ↓
       <div id="root"></div>
                 │
                 ↓
            📄 main.jsx
                 │
                 ↓
            <App />
                 │
                 ↓
            📄 App.jsx
                 │
                 ↓
        🧩 Other Components
                 │
                 ↓
             🖥️ UI
```

### 🧠 Remember:

> `index.html` gives React a place to start.

> `main.jsx` starts React.

> `App.jsx` contains the main application component.

> Components build the UI.

---

# 🧪 24. Small Practical Example

Open:

```text
src/App.jsx
```

Replace the existing code with:

```jsx
function App() {
    return (
        <div>
            <h1>My React App</h1>
            <p>I am learning React.</p>
        </div>
    );
}

export default App;
```

Save the file.

Your browser should show:

```text
My React App

I am learning React.
```

---

# 🔍 25. Dry Run

Let's understand what happens.

### Step 1

Browser loads:

```text
index.html
```

### Step 2

HTML contains:

```html
<div id="root"></div>
```

### Step 3

Browser loads:

```text
main.jsx
```

### Step 4

`main.jsx` imports:

```jsx
App
```

from:

```text
App.jsx
```

### Step 5

React renders:

```jsx
<App />
```

### Step 6

`App.jsx` returns:

```jsx
<div>
    <h1>My React App</h1>
    <p>I am learning React.</p>
</div>
```

### Step 7

React displays it in:

```html
<div id="root"></div>
```

### Result

```text
My React App
I am learning React.
```

---

# ❌ 26. Common Beginner Mistakes

## Mistake 1 — Editing `node_modules`

Don't manually edit:

```text
node_modules/
```

❌ Avoid this.

---

## Mistake 2 — Deleting `package.json`

Don't delete:

```text
package.json
```

It contains important project information.

---

## Mistake 3 — Confusing `App.jsx` and `main.jsx`

Remember:

```text
main.jsx
    ↓
starts/renders React

App.jsx
    ↓
main application component
```

---

## Mistake 4 — Forgetting `export default`

If you have:

```jsx
function App() {
    return <h1>Hello</h1>;
}
```

and another file imports:

```jsx
import App from "./App.jsx";
```

you normally need:

```jsx
export default App;
```

---

## Mistake 5 — Wrong import path

Example:

```jsx
import App from "./App.jsx";
```

If the file doesn't exist at that location, you will get an error.

---

# ✅ 27. Best Practices

### ⭐ Practice 1

Keep your React source code inside:

```text
src/
```

### ⭐ Practice 2

Create reusable components instead of putting everything inside `App.jsx`.

### ⭐ Practice 3

Don't manually modify `node_modules`.

### ⭐ Practice 4

Understand the purpose of files before deleting them.

### ⭐ Practice 5

Keep components organized as the project grows.

---

# 🎤 28. Interview Questions

## Q1. What is the `src` folder?

### Answer:

The `src` folder contains the main source code of a React application.

React components, JavaScript/JSX files, CSS, hooks, services, and other application code are commonly stored inside `src`.

---

## Q2. What is `App.jsx`?

### Answer:

`App.jsx` usually contains the main `App` React component.

It can act as the main component that connects other components in the application.

---

## Q3. What is `main.jsx`?

### Answer:

`main.jsx` is the entry point that creates the React root and renders the main application component into the HTML root element.

---

## Q4. What is `index.html`?

### Answer:

`index.html` is the main HTML document used by the Vite application.

It contains the root element where React renders the application.

---

## Q5. What is `package.json`?

### Answer:

`package.json` contains project information, scripts, and dependency definitions for the project.

---

## Q6. What is `node_modules`?

### Answer:

`node_modules` contains installed npm packages and their dependencies.

---

## Q7. What is `.gitignore`?

### Answer:

`.gitignore` tells Git which files or folders should not be tracked.

---

## Q8. What is `vite.config.js`?

### Answer:

It contains configuration for Vite, the development/build tool used by the React project.

---

# 🧩 29. Practice Task

Open your React project.

Find these files:

```text
☐ index.html
☐ package.json
☐ package-lock.json
☐ vite.config.js
☐ src/
☐ src/App.jsx
☐ src/main.jsx
☐ src/index.css
```

Now answer these yourself:

### Task 1

Which file contains the root HTML element?

```text
Answer: __________
```

### Task 2

Which file renders `<App />`?

```text
Answer: __________
```

### Task 3

Where does most React source code live?

```text
Answer: __________
```

### Task 4

Where are installed npm packages stored?

```text
Answer: __________
```

### Task 5

Which file contains project dependencies?

```text
Answer: __________
```

---

# 🏆 30. Mini Challenge

Create this UI:

```text
-------------------------
      My Website
-------------------------

Welcome to React!

I am learning ReactJS.

-------------------------
```

Use:

```jsx
function App() {
    return (
        <div>
            <h1>My Website</h1>
            <p>Welcome to React!</p>
            <p>I am learning ReactJS.</p>
        </div>
    );
}

export default App;
```

### 🎯 Your goal

Don't just copy it.

Try writing it yourself.

---

# 📝 31. 10-Question Quiz

## Question 1

Which folder contains the main React source code?

A. `public`  
B. `src`  
C. `node_modules`  
D. `dist`

### ✅ Answer: B — `src`

---

## Question 2

Which file usually contains the main `App` component?

A. `App.jsx`  
B. `package.json`  
C. `index.html`  
D. `vite.config.js`

### ✅ Answer: A — `App.jsx`

---

## Question 3

Which file is the React entry point?

A. `App.jsx`  
B. `main.jsx`  
C. `App.css`  
D. `index.css`

### ✅ Answer: B — `main.jsx`

---

## Question 4

Where does React render the application?

```html
<div id="______"></div>
```

### ✅ Answer:

```text
root
```

---

## Question 5

Which file contains project dependencies?

A. `package.json`  
B. `index.html`  
C. `App.jsx`  
D. `.gitignore`

### ✅ Answer: A — `package.json`

---

## Question 6

Which folder contains installed npm packages?

### ✅ Answer:

```text
node_modules
```

---

## Question 7

Which file helps Git ignore files?

### ✅ Answer:

```text
.gitignore
```

---

## Question 8

What does `vite.config.js` contain?

### ✅ Answer:

Vite configuration.

---

## Question 9

What does this do?

```jsx
<App />
```

### ✅ Answer:

It renders the `App` React component.

---

## Question 10

Complete the flow:

```text
index.html
    ↓
________
    ↓
<App />
    ↓
App.jsx
```

### ✅ Answer:

```text
main.jsx
```

---

# 🧠 32. Final Memory Trick

Remember these **5 files** first:

```text
🌐 index.html
     ↓
🚪 main.jsx
     ↓
🧩 App.jsx
     ↓
📦 package.json
     ↓
⚙️ vite.config.js
```

But the most important relationship is:

```text
index.html
     ↓
main.jsx
     ↓
App.jsx
     ↓
Components
     ↓
UI
```

### 🧠 One-line memory

> **HTML gives the place → main.jsx starts React → App.jsx builds the main UI → components build the application.**

---

# 📌 33. Quick Revision Table

| File / Folder | Purpose |
|---|---|
| 📂 `node_modules` | Installed npm packages |
| 📂 `public` | Static/public files |
| 📂 `src` | Main source code |
| 📂 `src/assets` | Imported assets |
| 📄 `App.jsx` | Main App component |
| 📄 `main.jsx` | React entry point |
| 📄 `index.html` | HTML shell/root element |
| 📄 `index.css` | Global CSS |
| 📄 `App.css` | App-related CSS |
| 📄 `package.json` | Project info/dependencies/scripts |
| 📄 `package-lock.json` | Locked dependency details |
| ⚙️ `vite.config.js` | Vite configuration |
| 🚫 `.gitignore` | Files ignored by Git |
| 🧹 `eslint.config.js` | ESLint configuration |

---

# 🎯 Chapter 3 Checklist

Before moving forward, make sure you understand:

- [ ] What `src` means
- [ ] What `App.jsx` does
- [ ] What `main.jsx` does
- [ ] What `index.html` does
- [ ] What `package.json` does
- [ ] What `node_modules` is
- [ ] What `public` is
- [ ] What `vite.config.js` is
- [ ] What `.gitignore` does
- [ ] React application startup flow

---

# 🚀 Next Chapter

## 🧩 CHAPTER 4 — JSX

In the next chapter we will learn:

```text
What is JSX?
       ↓
Why JSX?
       ↓
HTML + JavaScript
       ↓
JSX Rules
       ↓
Expressions
       ↓
Variables inside JSX
       ↓
className
       ↓
JSX attributes
       ↓
Multiple elements
       ↓
Fragments
       ↓
Common JSX mistakes
       ↓
Practice
       ↓
Quiz
```

👉 **Stop here.**

Type **`NEXT`** when you are ready for **Chapter 4 — JSX**.