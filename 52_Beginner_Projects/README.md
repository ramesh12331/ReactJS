# 🏗️ CHAPTER 52 — BEGINNER REACT PROJECTS

## 🎯 Goal of This Chapter

Until now, we learned React concepts separately.

Now we start **building projects**.

The projects in this chapter are:

```text
1. 🔢 Counter App
2. 📝 Todo App
3. 🧮 Calculator
4. 🕐 Digital Clock
5. 💬 Random Quote App
6. 📝 Notes App
```

These six projects are the beginner projects specified in your React course plan.

---

# 1. 🧠 Why Do We Need Projects?

Learning React concepts individually is not enough.

For example, you may know:

```text
useState()
useEffect()
props
events
map()
filter()
components
```

But an interviewer may ask:

> "Build a Todo application."

Now you need to combine multiple concepts.

That is why projects are important.

---

# 2. 🗺️ Our Project Learning Method

For every project, we will follow:

```text
Problem
   ↓
Requirements
   ↓
UI Design
   ↓
State
   ↓
Events
   ↓
Logic
   ↓
Components
   ↓
Code
   ↓
Dry Run
   ↓
Test
   ↓
Improve
```

We will **not** immediately jump into complicated code.

---

# 3. 🟢 PROJECT 1 — COUNTER APP

We will start with the easiest project.

## 🎯 Requirements

Our Counter App should have:

```text
Count: 0

[ + ]

[ - ]

[ Reset ]
```

Features:

```text
➕ Increase
➖ Decrease
🔄 Reset
```

---

# 4. 🧠 What React Concepts Will We Practice?

This small project uses:

```text
useState
Events
Event handlers
JSX
Conditional logic
Functional state updates
```

This is a good first project because the logic is simple.

---

# 5. 🏗️ Project Structure

Start with:

```text
src/
│
├── App.jsx
├── App.css
└── main.jsx
```

Later, when the project becomes larger, we can split it into components.

---

# 6. 🧠 Step 1 — Identify State

Ask:

> What value changes?

Answer:

```text
count
```

Therefore:

```jsx
const [count, setCount] =
    useState(0);
```

Initial value:

```text
count = 0
```

---

# 7. 🖱️ Step 2 — Identify Events

We have three buttons.

```text
+      → increment
-      → decrement
Reset  → reset
```

Therefore:

```text
onClick
```

---

# 8. 📝 Step 3 — Write the Logic

Before coding:

```text
Increment:
    count + 1

Decrement:
    count - 1

Reset:
    count = 0
```

This is our logic.

---

# 9. 💻 Step 4 — Complete App

```jsx
import { useState } from "react";


function App() {

    const [count, setCount] =
        useState(0);


    function handleIncrement() {

        setCount(
            previousCount =>
                previousCount + 1
        );

    }


    function handleDecrement() {

        setCount(
            previousCount =>
                previousCount - 1
        );

    }


    function handleReset() {

        setCount(0);

    }


    return (
        <div>

            <h1>
                Counter App
            </h1>


            <h2>
                Count: {count}
            </h2>


            <button
                onClick={handleIncrement}
            >
                +
            </button>


            <button
                onClick={handleDecrement}
            >
                -
            </button>


            <button
                onClick={handleReset}
            >
                Reset
            </button>

        </div>
    );
}


export default App;
```

---

# 10. 🔍 Dry Run

Initial state:

```text
count = 0
```

UI:

```text
Counter App

Count: 0

[ + ] [ - ] [ Reset ]
```

Click `+`.

```text
previousCount = 0

0 + 1
= 1
```

UI becomes:

```text
Count: 1
```

Click `+` again:

```text
1 + 1
= 2
```

UI:

```text
Count: 2
```

Click `-`:

```text
2 - 1
= 1
```

UI:

```text
Count: 1
```

Click Reset:

```text
setCount(0)
```

UI:

```text
Count: 0
```

---

# 11. 📤 Expected Output

```text
        Counter App

           Count: 0

       [ + ]  [ - ]

            [ Reset ]
```

After clicking `+` three times:

```text
        Counter App

           Count: 3
```

---

# 12. 🧠 Important Line

This:

```jsx
setCount(
    previousCount =>
        previousCount + 1
);
```

means:

```text
Take previous count
       ↓
Add 1
       ↓
Update state
       ↓
React re-renders UI
```

---

# 13. ⚠️ Common Mistake

Don't do this:

```jsx
<button
    onClick={handleIncrement()}
>
    +
</button>
```

Why?

Because:

```text
handleIncrement()
```

calls the function immediately during rendering.

Instead:

```jsx
<button
    onClick={handleIncrement}
>
    +
</button>
```

React calls it when the user clicks.

---

# 14. 🛡️ Improvement — Don't Go Below Zero

Suppose we want:

```text
0
↓
Click -
↓
0
```

The counter should never become negative.

Use:

```jsx
function handleDecrement() {

    setCount(previousCount =>
        Math.max(
            0,
            previousCount - 1
        )
    );

}
```

Now:

```text
0 → 0
```

instead of:

```text
0 → -1
```

---

# 15. 🎨 Add Basic CSS

`App.css`

```css
body {
    font-family: Arial, sans-serif;
    text-align: center;
    margin-top: 100px;
}

button {
    margin: 5px;
    padding: 10px 20px;
    font-size: 18px;
    cursor: pointer;
}
```

Then import it:

```jsx
import "./App.css";
```

---

# 16. 🧩 PROJECT 1 — COMPONENT VERSION

Now let's make our project more professional.

Create:

```text
src/
│
├── components/
│   └── Counter.jsx
│
├── App.jsx
└── App.css
```

`Counter.jsx`:

```jsx
import { useState } from "react";


function Counter() {

    const [count, setCount] =
        useState(0);


    function increment() {

        setCount(
            previousCount =>
                previousCount + 1
        );

    }


    function decrement() {

        setCount(
            previousCount =>
                Math.max(
                    0,
                    previousCount - 1
                )
        );

    }


    function reset() {

        setCount(0);

    }


    return (
        <div>

            <h2>
                Count: {count}
            </h2>

            <button onClick={increment}>
                +
            </button>

            <button onClick={decrement}>
                -
            </button>

            <button onClick={reset}>
                Reset
            </button>

        </div>
    );
}


export default Counter;
```

Then `App.jsx`:

```jsx
import Counter from "./components/Counter";


function App() {

    return (
        <div>

            <h1>
                My Counter Project
            </h1>

            <Counter />

        </div>
    );
}


export default App;
```

---

# 17. 🧠 Why Separate Components?

Imagine a real application.

You may have:

```text
App
│
├── Header
├── Sidebar
├── Counter
├── Products
└── Footer
```

Instead of putting everything inside `App.jsx`, we create reusable components.

This makes the project:

```text
✅ Cleaner
✅ Reusable
✅ Easier to debug
✅ Easier to maintain
```

---

# 18. 🔥 Counter App — Final Architecture

```text
App
 │
 └── Counter
       │
       ├── count state
       │
       ├── increment()
       │
       ├── decrement()
       │
       └── reset()
```

---

# 19. 🧪 Practice Task 1

Modify the Counter App.

Add:

```text
[ +5 ]
```

When clicked:

```text
0 → 5
5 → 10
10 → 15
```

Hint:

```jsx
setCount(
    previousCount =>
        previousCount + 5
);
```

---

# 20. 🧪 Practice Task 2

Add:

```text
[ -5 ]
```

But don't allow:

```text
0 → -5
```

Hint:

```jsx
Math.max()
```

---

# 21. 🧪 Practice Task 3

Display:

```text
Even
```

or:

```text
Odd
```

Example:

```text
Count: 4

Even
```

Hint:

```javascript
count % 2 === 0
```

---

# 22. 🧪 Practice Task 4

Add a step input.

Example:

```text
Step: [ 5 ]

[ + ] [ - ]
```

If:

```text
Count = 10
Step = 5
```

Click `+`:

```text
15
```

Click `-`:

```text
10
```

This teaches you how multiple pieces of state can work together.

---

# 23. 🧠 PROJECT 2 — TODO APP

Now we move to the second beginner project from your syllabus.

A Todo App is more important than Counter because it introduces arrays and list rendering.

---

# 24. 🎯 Todo Requirements

Our basic Todo App will have:

```text
Todo App

[ Enter todo... ] [ Add ]

☐ Learn React
☐ Learn JavaScript
☐ Build Project
```

Features:

```text
➕ Add todo
🗑️ Delete todo
✅ Complete todo
```

---

# 25. 🧠 Identify the Data

What changes?

The todo list.

Example:

```javascript
[
    {
        id: 1,
        text: "Learn React",
        completed: false
    },
    {
        id: 2,
        text: "Learn JavaScript",
        completed: true
    }
]
```

Therefore state:

```jsx
const [todos, setTodos] =
    useState([]);
```

We also need input state:

```jsx
const [text, setText] =
    useState("");
```

---

# 26. 🧠 Todo Flow

```text
User types
    ↓
text state

User clicks Add
    ↓
Create todo object
    ↓
Add to todos
    ↓
React re-renders
    ↓
Todo appears
```

---

# 27. 💻 Basic Todo App

```jsx
import { useState } from "react";


function App() {

    const [text, setText] =
        useState("");


    const [todos, setTodos] =
        useState([]);


    function addTodo() {

        if (text.trim() === "") {
            return;
        }


        const newTodo = {
            id: Date.now(),
            text: text,
            completed: false
        };


        setTodos(previousTodos => [
            ...previousTodos,
            newTodo
        ]);


        setText("");

    }


    return (
        <div>

            <h1>
                Todo App
            </h1>


            <input
                type="text"
                value={text}
                onChange={event =>
                    setText(
                        event.target.value
                    )
                }
                placeholder="Enter todo"
            />


            <button
                onClick={addTodo}
            >
                Add
            </button>


            <ul>

                {todos.map(todo => (

                    <li key={todo.id}>

                        {todo.text}

                    </li>

                ))}

            </ul>

        </div>
    );
}


export default App;
```

---

# 28. 🔍 Todo Dry Run

Initial:

```text
text = ""
todos = []
```

User types:

```text
Learn React
```

Now:

```text
text = "Learn React"
```

Click Add.

Create:

```javascript
{
    id: 123,
    text: "Learn React",
    completed: false
}
```

Then:

```text
todos = [
    {
        id: 123,
        text: "Learn React",
        completed: false
    }
]
```

React renders:

```text
☐ Learn React
```

---

# 29. ➕ Add Second Todo

User types:

```text
Learn JavaScript
```

Click Add.

Now:

```text
todos = [
    {
        id: 1,
        text: "Learn React",
        completed: false
    },

    {
        id: 2,
        text: "Learn JavaScript",
        completed: false
    }
]
```

`.map()` renders both.

---

# 30. 🧠 Why Use `...previousTodos`?

This:

```jsx
setTodos(previousTodos => [
    ...previousTodos,
    newTodo
]);
```

means:

```text
Previous todos
       +
New todo
       ↓
New array
```

Example:

```text
Old:
[A, B]

New:
[A, B, C]
```

We don't directly mutate the existing state array.

---

# 31. 🗑️ Delete Todo

Logic:

```text
Click Delete
    ↓
Get todo ID
    ↓
Remove matching todo
```

Use:

```jsx
function deleteTodo(todoId) {

    setTodos(previousTodos =>
        previousTodos.filter(
            todo =>
                todo.id !== todoId
        )
    );

}
```

Then:

```jsx
<button
    onClick={() =>
        deleteTodo(todo.id)
    }
>
    Delete
</button>
```

---

# 32. 🧠 Delete Logic Dry Run

Suppose:

```text
todos:

1 → Learn React
2 → Learn JS
3 → Learn SQL
```

Delete:

```text
id = 2
```

Filter condition:

```text
1 !== 2 → keep
2 !== 2 → remove
3 !== 2 → keep
```

Result:

```text
1 → Learn React
3 → Learn SQL
```

---

# 33. ✅ Complete Todo

We need to change:

```text
completed: false
```

to:

```text
completed: true
```

Logic:

```jsx
function toggleTodo(todoId) {

    setTodos(previousTodos =>
        previousTodos.map(todo => {

            if (todo.id === todoId) {

                return {
                    ...todo,
                    completed:
                        !todo.completed
                };

            }

            return todo;

        })
    );

}
```

---

# 34. 🧠 Todo Logic

```text
Todo clicked
     ↓
Find todo
     ↓
Reverse completed
     ↓
false → true
true → false
     ↓
Render updated todo
```

---

# 35. 🎨 Display Completed Todo

```jsx
<li
    key={todo.id}
    style={{
        textDecoration:
            todo.completed
                ? "line-through"
                : "none"
    }}
>
    {todo.text}
</li>
```

Result:

```text
Learn React
Learn JavaScript
──────────────
Learn SQL
```

Completed tasks can be shown with a line through them.

---

# 36. 🧠 Todo App Architecture

As the project grows:

```text
App
│
├── TodoForm
│
└── TodoList
     │
     └── TodoItem
```

Communication:

```text
App
 ↓ props
TodoForm

App
 ↓ todos + functions
TodoList
 ↓
TodoItem
```

This gives us practice with:

```text
Props
Callbacks
State lifting
Component communication
```

---

# 37. ⚠️ Common Todo Mistakes

### Mistake 1 — Missing key

Bad:

```jsx
todos.map(todo => (
    <li>
        {todo.text}
    </li>
))
```

Better:

```jsx
todos.map(todo => (
    <li key={todo.id}>
        {todo.text}
    </li>
))
```

---

### Mistake 2 — Mutating state

Avoid:

```javascript
todos.push(newTodo);
```

Instead:

```javascript
setTodos(previousTodos => [
    ...previousTodos,
    newTodo
]);
```

---

### Mistake 3 — Allowing empty todos

Avoid adding:

```text
""
```

Use:

```javascript
if (text.trim() === "") {
    return;
}
```

---

# 38. 🧪 MINI CHALLENGE

Build the Counter App yourself first.

Requirements:

```text
Counter App

Count: 0

[ + ]
[ - ]
[ Reset ]

Rules:

1. + increases by 1
2. - decreases by 1
3. Count cannot go below 0
4. Reset returns to 0
5. Display Even/Odd
```

### Don't look at the previous solution immediately.

Think:

```text
State?
Events?
Functions?
UI?
```

Try it yourself.

---

# 39. 📝 PRACTICE QUESTIONS

### Beginner

1. Add a `+5` button.
2. Add a `-5` button.
3. Add an even/odd indicator.
4. Add a maximum count of 100.
5. Add a minimum count of 0.

### Todo

6. Add a todo.
7. Delete a todo.
8. Mark a todo complete.
9. Display total todos.
10. Display completed todos.

---

# 40. 🎤 INTERVIEW QUESTIONS

### Q1. Why is Counter App useful for learning React?

**Answer:**

Because it teaches state, events, event handlers, and re-rendering with very simple logic.

---

### Q2. Why do we use `useState()` in the Counter App?

**Answer:**

Because the counter value changes and the UI needs to update when the value changes.

---

### Q3. Why do we use `map()` in Todo App?

**Answer:**

To render a React element for each todo item.

---

### Q4. Why do todos need a `key`?

**Answer:**

React uses keys to identify list items efficiently when the list changes.

---

### Q5. Why shouldn't we use `todos.push()` directly?

**Answer:**

Because it mutates the existing state array. React state should be updated using the state setter and immutable update patterns.

---

# 🧠 FINAL MEMORY TRICK

## Counter

```text
useState
   ↓
Events
   ↓
Update number
```

## Todo

```text
useState
   ↓
Array
   ↓
map()
filter()
   ↓
Render list
```

Remember:

```text
➕ Add     → spread [...]
🗑️ Delete  → filter()
🔄 Update  → map()
📋 Display → map()
```

---

# 📚 BEGINNER PROJECT ROADMAP

Your source defines six beginner projects:

```text
PROJECT 1 → 🔢 Counter App
                ↓
PROJECT 2 → 📝 Todo App
                ↓
PROJECT 3 → 🧮 Calculator
                ↓
PROJECT 4 → 🕐 Digital Clock
                ↓
PROJECT 5 → 💬 Random Quote App
                ↓
PROJECT 6 → 📝 Notes App
```

We have started **Project 1 and Project 2** in this chapter.

The important goal is not simply to copy the code.

The goal is:

> 🧠 **Understand the requirement → think about state → design the logic → write React code.**

---

# 🔁 QUICK REVISION

```text
Counter
→ useState
→ onClick
→ functional state update

Todo
→ useState
→ arrays
→ map()
→ filter()
→ immutable updates
→ keys
→ callbacks
```

## ⭐ Most Important Patterns

```jsx
// Add
setItems(previous => [
    ...previous,
    newItem
]);

// Delete
setItems(previous =>
    previous.filter(
        item => item.id !== id
    )
);

// Update
setItems(previous =>
    previous.map(item =>
        item.id === id
            ? { ...item, completed: !item.completed }
            : item
    )
);
```

These three patterns are extremely useful in React applications.

---

# 🛑 CHAPTER 52 — PART 1 COMPLETE

Next, we continue the same **Chapter 52** with the remaining beginner projects:

```text
🧮 Calculator
🕐 Digital Clock
💬 Random Quote App
📝 Notes App
```

Type **NEXT** to continue.