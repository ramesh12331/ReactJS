
# 🎯 Chapter 13 — `useRef`

## 🎯 Chapter Goal

In this chapter, you will learn:

- What is `useRef()`?
- Why do we need `useRef()`?
- How to access DOM elements
- How to access an input
- How to focus an input
- How to store values using `useRef`
- `useRef` vs `useState`
- Using `useRef` with timers
- Storing previous values
- Practical examples
- Common mistakes

---

# 1️⃣ What Is `useRef()`?

`useRef()` is a React Hook that lets us keep a value between renders **without causing a re-render when that value changes**.

Basic syntax:

```jsx
const reference = useRef(initialValue);
```

Example:

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    return (
        <input ref={inputRef} />
    );
}

export default App;
```

The important part is:

```jsx
const inputRef = useRef(null);
```

and:

```jsx
<input ref={inputRef} />
```

---

# 2️⃣ Why Do We Need `useRef()`?

One very common use is:

> **Directly accessing a DOM element.**

For example, suppose we have:

```text
📦 Input
```

and we want to automatically focus it.

Normally, the browser DOM has methods like:

```javascript
input.focus()
```

But React manages the UI.

`useRef()` gives us a reference to that DOM element.

Flow:

```text
React Component
      ↓
   useRef()
      ↓
   DOM Element
      ↓
    Input
      ↓
   focus()
```

---

# 3️⃣ Real-World Analogy 🏷️

Imagine a school has 100 students.

Instead of searching for a student every time, you have the student's ID card.

```text
Student
   ↓
ID Card
   ↓
Easy to identify
```

Similarly:

```text
DOM Element
   ↓
ref
   ↓
Easy to access
```

Think of `useRef` as a **reference/handle** to something.

---

# 4️⃣ First `useRef()` Example

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    return (
        <div>

            <input ref={inputRef} />

            <button>
                Focus Input
            </button>

        </div>
    );
}

export default App;
```

We created:

```jsx
const inputRef = useRef(null);
```

Then attached it:

```jsx
<input ref={inputRef} />
```

Now `inputRef` can refer to that input element.

---

# 5️⃣ How Does `ref` Work?

This:

```jsx
<input ref={inputRef} />
```

means:

```text
React
 ↓
Find this input
 ↓
Store reference
 ↓
inputRef.current
```

The actual DOM element can be accessed through:

```jsx
inputRef.current
```

⭐ Remember this:

```text
useRef()
   ↓
ref object
   ↓
.current
   ↓
actual value / DOM element
```

---

# 6️⃣ `.current` Is Very Important ⭐⭐⭐

When you create:

```jsx
const inputRef = useRef(null);
```

React gives you an object conceptually like:

```javascript
{
    current: null
}
```

Initially:

```text
current → null
```

After attaching it to an input:

```jsx
<input ref={inputRef} />
```

it can become:

```text
inputRef
   ↓
current
   ↓
<input>
```

So:

```jsx
inputRef.current
```

gives access to the referenced value.

---

# 7️⃣ Focus an Input 🎯

Now let's create a real example.

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    function focusInput() {
        inputRef.current.focus();
    }

    return (
        <div>

            <input ref={inputRef} />

            <button onClick={focusInput}>
                Focus Input
            </button>

        </div>
    );
}

export default App;
```

---

# 8️⃣ Dry Run 🧠

Initially:

```text
inputRef.current
        ↓
      null
```

React renders:

```jsx
<input ref={inputRef} />
```

React connects the reference:

```text
inputRef.current
        ↓
     input DOM
```

User clicks:

```text
Focus Input
```

Then:

```jsx
inputRef.current.focus();
```

The browser focuses the input.

Result:

```text
┌──────────────────────┐
│ |                    │
└──────────────────────┘
        ↑
      Focus
```

---

# 9️⃣ Why `useRef()` Instead of `document.getElementById()`?

You might already know JavaScript:

```javascript
document.getElementById("username")
```

You could use:

```jsx
<input id="username" />
```

and then:

```javascript
document.getElementById("username")
```

But React provides a cleaner way for specific DOM access:

```jsx
const inputRef = useRef(null);
```

then:

```jsx
<input ref={inputRef} />
```

and:

```jsx
inputRef.current
```

In React, prefer refs when you genuinely need direct DOM access.

---

# 🔟 Practical Example — Login Form

Imagine a login page:

```text
Email
[________________]

Password
[________________]

[ Login ]
```

After the page opens, we might want the email field focused.

We can use:

```jsx
import { useEffect, useRef } from "react";

function Login() {

    const emailRef = useRef(null);

    useEffect(() => {
        emailRef.current.focus();
    }, []);

    return (
        <div>

            <h1>Login</h1>

            <input
                ref={emailRef}
                placeholder="Enter email"
            />

            <br />

            <input
                type="password"
                placeholder="Enter password"
            />

        </div>
    );
}

export default Login;
```

Flow:

```text
Component renders
       ↓
emailRef points to input
       ↓
useEffect runs
       ↓
emailRef.current.focus()
       ↓
Email input gets focus
```

This combines the concepts from the previous chapter:

```text
useEffect + useRef
```

---

# 1️⃣1️⃣ `useRef` Can Store Values

`useRef()` is not only for DOM elements.

It can also store a value:

```jsx
const countRef = useRef(0);
```

Then:

```jsx
countRef.current = countRef.current + 1;
```

Example:

```jsx
import { useRef } from "react";

function App() {

    const countRef = useRef(0);

    function handleClick() {

        countRef.current++;

        console.log(countRef.current);
    }

    return (
        <button onClick={handleClick}>
            Click Me
        </button>
    );
}

export default App;
```

Click the button:

```text
1
2
3
4
5
```

will appear in the console.

---

# 1️⃣2️⃣ Important Difference

When you change:

```jsx
countRef.current
```

React does **not** automatically re-render the component because of that ref change.

This is one of the most important differences between `useRef` and `useState`.

---

# 1️⃣3️⃣ `useRef` vs `useState` ⭐⭐⭐

This is an important interview question.

## `useState`

```jsx
const [count, setCount] = useState(0);
```

When you do:

```jsx
setCount(1);
```

React schedules a re-render.

Flow:

```text
setCount()
   ↓
State changes
   ↓
React re-renders
   ↓
UI updates
```

---

## `useRef`

```jsx
const countRef = useRef(0);
```

When you do:

```jsx
countRef.current = 1;
```

React does not re-render because of that ref update.

Flow:

```text
ref.current changes
       ↓
No automatic re-render
```

---

# 1️⃣4️⃣ Easy Comparison

| `useState` | `useRef` |
|---|---|
| Stores state | Stores a mutable reference |
| Changes cause re-render | Changes do not cause re-render |
| Used for UI data | Used for refs/values |
| UI usually depends on it | UI doesn't automatically update |
| `setState()` | `.current = ...` |
| Common for counters/forms | Common for DOM access/timers |

---

# 1️⃣5️⃣ Very Simple Example

### State

```jsx
const [count, setCount] = useState(0);
```

Update:

```jsx
setCount(count + 1);
```

React:

```text
🔄 Re-render
```

---

### Ref

```jsx
const countRef = useRef(0);
```

Update:

```jsx
countRef.current++;
```

React:

```text
❌ No automatic re-render
```

---

# 1️⃣6️⃣ When Should You Use `useState`?

Use `useState` when the value should affect the UI.

Example:

```text
Counter
Form input
Selected product
Login status
Modal open/closed
```

If the UI needs to show the changed value, state is usually appropriate.

---

# 1️⃣7️⃣ When Should You Use `useRef`?

Use `useRef` when you need:

```text
🎯 DOM access
⏱️ Timer ID
💾 Value that persists between renders
📝 Previous value
🔢 Mutable value that doesn't need to trigger rendering
```

---

# 1️⃣8️⃣ Storing Previous Values

One useful pattern is storing a previous value.

Example:

```jsx
import { useEffect, useRef, useState } from "react";

function App() {

    const [count, setCount] = useState(0);

    const previousCount = useRef();

    useEffect(() => {
        previousCount.current = count;
    }, [count]);

    return (
        <div>

            <h1>Current: {count}</h1>

            <h2>
                Previous: {previousCount.current}
            </h2>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

        </div>
    );
}

export default App;
```

Conceptually:

```text
Current
  ↓
count

Previous
  ↓
previousCount.current
```

---

# 1️⃣9️⃣ Dry Run — Previous Value

Initial:

```text
count = 0
previousCount.current = undefined
```

After effect:

```text
previousCount.current = 0
```

Click:

```text
count = 1
```

Before the effect updates the ref, the ref still contains the previous value.

Then the effect stores:

```text
previousCount.current = 1
```

Next render:

```text
Current: 1
Previous: 0
```

The exact timing matters here, so don't think of `useRef` as automatically giving you a previous value. **You create the previous-value pattern yourself.**

---

# 2️⃣0️⃣ Timer IDs With `useRef` ⏱️

Suppose we create an interval:

```jsx
const timerRef = useRef(null);
```

Start:

```jsx
timerRef.current = setInterval(() => {
    console.log("Running...");
}, 1000);
```

Stop:

```jsx
clearInterval(timerRef.current);
```

This is useful because the timer ID needs to remain available between renders, but changing that ID doesn't need to update the UI.

---

# 2️⃣1️⃣ Complete Timer Example

```jsx
import { useRef } from "react";

function App() {

    const timerRef = useRef(null);

    function startTimer() {

        timerRef.current = setInterval(() => {
            console.log("Timer running...");
        }, 1000);

    }

    function stopTimer() {

        clearInterval(timerRef.current);

    }

    return (
        <div>

            <button onClick={startTimer}>
                Start
            </button>

            <button onClick={stopTimer}>
                Stop
            </button>

        </div>
    );
}

export default App;
```

---

# 2️⃣2️⃣ Why Ref Is Useful for Timer IDs

Imagine:

```text
Start button
     ↓
setInterval()
     ↓
timer ID
```

We need to remember that ID.

A ref can hold it:

```text
timerRef.current
       ↓
   timer ID
```

Then:

```jsx
clearInterval(timerRef.current);
```

can stop it.

---

# 2️⃣3️⃣ `useRef` Does Not Mean "Direct DOM Only"

Beginners often think:

```text
useRef = DOM
```

Not exactly.

`useRef` can be used for:

```text
🎯 DOM references
💾 Persistent mutable values
⏱️ Timer IDs
📝 Previous values
```

So remember:

> `useRef` stores a reference whose `.current` value persists across renders.

---

# 2️⃣4️⃣ `ref` on an Input

Example:

```jsx
const usernameRef = useRef(null);
```

Attach:

```jsx
<input ref={usernameRef} />
```

Access:

```jsx
usernameRef.current
```

Focus:

```jsx
usernameRef.current.focus();
```

Read the current input value:

```jsx
usernameRef.current.value
```

For example:

```jsx
function showValue() {

    console.log(usernameRef.current.value);

}
```

---

# 2️⃣5️⃣ Uncontrolled Input

When you use a ref to read the input directly:

```jsx
<input ref={inputRef} />
```

the DOM itself holds the current input value.

You can read:

```jsx
inputRef.current.value
```

This is commonly called an **uncontrolled input**.

You will later learn controlled and uncontrolled forms in more detail.

---

# 2️⃣6️⃣ Practical Example — Clear Input

```jsx
import { useRef } from "react";

function App() {

    const inputRef = useRef(null);

    function clearInput() {
        inputRef.current.value = "";
        inputRef.current.focus();
    }

    return (
        <div>

            <input
                ref={inputRef}
                placeholder="Enter name"
            />

            <button onClick={clearInput}>
                Clear
            </button>

        </div>
    );
}

export default App;
```

Click:

```text
Clear
```

The input becomes empty and gets focus.

---

# 2️⃣7️⃣ Practical Example — Focus Search Box 🔍

Imagine an application:

```text
--------------------------------
🔍 Search products

[________________________]

[ Search ]
--------------------------------
```

We can automatically focus the search box:

```jsx
import { useEffect, useRef } from "react";

function Search() {

    const searchRef = useRef(null);

    useEffect(() => {
        searchRef.current.focus();
    }, []);

    return (
        <input
            ref={searchRef}
            placeholder="Search products..."
        />
    );
}

export default Search;
```

This is a realistic use of `useRef`.

---

# 2️⃣8️⃣ DOM Methods You May Use With Refs

When a ref points to a DOM element, you may use normal DOM APIs when appropriate.

Examples:

```jsx
inputRef.current.focus();
```

```jsx
inputRef.current.blur();
```

```jsx
inputRef.current.value;
```

For an element:

```jsx
elementRef.current.scrollIntoView();
```

But don't use refs to control normal application state.

---

# 2️⃣9️⃣ Common Mistake — Forgetting `.current`

Wrong:

```jsx
inputRef.focus();
```

Correct:

```jsx
inputRef.current.focus();
```

Remember:

```text
ref
 ↓
current
 ↓
DOM element
```

---

# 3️⃣0️⃣ Common Mistake — Accessing Ref Too Early

This can be a problem:

```jsx
const inputRef = useRef(null);

console.log(inputRef.current);
```

before the element has been attached.

It can be:

```text
null
```

After React has rendered the element, the ref can point to it.

For actions that should happen after rendering, `useEffect` is often useful.

---

# 3️⃣1️⃣ Common Mistake — Expecting Ref Changes to Update UI

Suppose:

```jsx
const countRef = useRef(0);
```

Then:

```jsx
countRef.current++;
```

You might expect:

```text
UI changes
```

But it doesn't automatically re-render.

If the UI needs to change, use state:

```jsx
const [count, setCount] = useState(0);
```

---

# 3️⃣2️⃣ Common Mistake — Using Ref Instead of State

Bad idea:

```jsx
const nameRef = useRef("");

nameRef.current = "Ramesh";
```

and expecting React to update:

```jsx
<h1>{nameRef.current}</h1>
```

automatically.

It won't re-render just because `.current` changed.

If the name should update the UI, use:

```jsx
const [name, setName] = useState("");
```

---

# 3️⃣3️⃣ Common Mistake — Manipulating the DOM Everywhere

Don't use refs for normal rendering.

For example, don't manually do:

```javascript
element.style.color = "red";
element.innerText = "Hello";
```

for normal UI updates when React state/props can handle the job.

React should normally control the UI.

Use refs when you genuinely need imperative access.

---

# ⭐ 3️⃣4️⃣ Best Practices

### ✅ Use refs for DOM access

```jsx
inputRef.current.focus();
```

### ✅ Use state for UI data

```jsx
const [name, setName] = useState("");
```

### ✅ Remember `.current`

```jsx
ref.current
```

### ✅ Use stable refs for timer IDs

```jsx
timerRef.current
```

### ✅ Don't expect ref changes to re-render

Remember:

```text
ref.current change
       ↓
No automatic render
```

---

# 🧠 3️⃣5️⃣ `useState` vs `useRef` — Final Comparison

```text
             useState
                │
                ↓
          Value changes
                │
                ↓
          React re-renders
                │
                ↓
              UI
```

But:

```text
              useRef
                │
                ↓
        .current changes
                │
                ↓
       No automatic render
```

Use:

```text
🟢 useState → UI needs to update

🔵 useRef → Need a persistent reference/value
```

---

# 🎤 3️⃣6️⃣ Interview Questions

## Q1. What is `useRef()`?

`useRef()` is a React Hook that creates a persistent mutable reference object whose `.current` value can be changed without causing a re-render.

---

## Q2. What is the most common use of `useRef()`?

Accessing DOM elements.

Example:

```jsx
inputRef.current.focus();
```

---

## Q3. What is `.current`?

`.current` stores the current value of the ref.

For a DOM ref, it can point to the DOM element.

---

## Q4. Does changing `ref.current` cause a re-render?

No.

Changing `.current` does not automatically trigger a React re-render.

---

## Q5. What is the difference between `useState` and `useRef`?

`useState` updates can cause a re-render. Changing `useRef().current` does not.

---

## Q6. Can `useRef()` store values other than DOM elements?

Yes.

It can store values such as:

```text
Timer IDs
Previous values
Mutable values
DOM references
```

---

## Q7. How do you focus an input using `useRef`?

```jsx
inputRef.current.focus();
```

---

## Q8. What is an uncontrolled input?

An input where the DOM maintains its current value rather than React state directly controlling the value.

A ref can be used to access its value.

---

## Q9. What happens to a ref when the component re-renders?

The same ref object persists between renders.

---

## Q10. Should `useRef` replace `useState`?

No.

Use state when changes should update the UI. Use refs for persistent values or imperative access that does not need to trigger rendering.

---

# 🧪 3️⃣7️⃣ Practice Tasks

## Task 1 — Focus Input

Create:

```text
Input
[________________]

[Focus]
```

Clicking **Focus** should focus the input.

---

## Task 2 — Clear Input

Create:

```text
[Hello Ramesh]

[Clear]
```

Clicking **Clear** should:

```text
1. Empty the input
2. Focus the input
```

---

## Task 3 — Show Input Value in Console

Create an input and button.

When the button is clicked:

```javascript
console.log(inputRef.current.value);
```

---

## Task 4 — Timer

Create:

```text
[Start Timer]
[Stop Timer]
```

Use:

```jsx
useRef()
```

to store the interval ID.

---

## Task 5 — Previous Value

Create a counter and use:

```jsx
useRef()
```

to remember the previous count.

Display:

```text
Current: 5
Previous: 4
```

---

# 🏆 3️⃣8️⃣ Mini Challenge — Auto Focus Search

Build this:

```text
----------------------------------
        Product Search

🔍 [ Search products... ]

        [ Search ]
----------------------------------
```

Requirements:

1. Use `useRef`.
2. Automatically focus the search input when the component loads.
3. Add a Search button.
4. When Search is clicked, print the input value to the console.
5. Add a Clear button.
6. Clear the input and focus it again.

Expected flow:

```text
Component loads
      ↓
useRef()
      ↓
Input reference
      ↓
useEffect()
      ↓
focus()
```

---

# 📝 3️⃣9️⃣ 10-Question Quiz

## Q1. Which Hook creates a reference?

### ✅ Answer

```jsx
useRef()
```

---

## Q2. Where is the current ref value stored?

### ✅ Answer

Inside:

```jsx
ref.current
```

---

## Q3. How do you focus an input?

### ✅ Answer

```jsx
inputRef.current.focus();
```

---

## Q4. Does changing `.current` cause a re-render?

### ❌ Answer

No.

---

## Q5. Which is better when UI must update?

```text
useRef
or
useState
```

### ✅ Answer

```text
useState
```

---

## Q6. Can `useRef` store a timer ID?

### ✅ Answer

Yes.

---

## Q7. What does this do?

```jsx
const inputRef = useRef(null);
```

### ✅ Answer

It creates a ref whose initial `.current` value is `null`.

---

## Q8. What is wrong here?

```jsx
inputRef.focus();
```

### ❌ Answer

You need:

```jsx
inputRef.current.focus();
```

---

## Q9. Can `useRef` store previous values?

### ✅ Answer

Yes. A ref can be used to create a previous-value pattern.

---

## Q10. What is the main difference between `useState` and `useRef`?

### ✅ Answer

State updates can trigger a re-render, while changing `ref.current` does not automatically trigger a re-render.

---

# 📚 4️⃣0️⃣ Quick Revision

| Concept | Remember |
|---|---|
| 🎯 `useRef()` | Creates a persistent reference |
| `.current` | Stores the current ref value |
| 🌐 DOM | Ref can access DOM elements |
| 🎯 Focus | `inputRef.current.focus()` |
| ⏱️ Timer | Ref can store timer ID |
| 📝 Previous value | Ref can store previous value |
| 🔄 Re-render | Ref change does not trigger one |
| 🟢 `useState` | Use for UI-changing data |
| 🔵 `useRef` | Use for persistent references/values |
| ⚠️ Important | Don't forget `.current` |

---

# 🧠 4️⃣1️⃣ Final Memory Trick

Remember these three words:

```text
useRef
  ↓
.current
  ↓
Reference
```

For DOM:

```text
useRef()
   ↓
ref
   ↓
ref.current
   ↓
DOM element
   ↓
focus()
```

And remember:

```text
🟢 useState
→ UI changes

🔵 useRef
→ Reference/value without automatic re-render
```

### ⭐ One important sentence

> **`useRef` lets you keep a value between renders and access DOM elements without causing a re-render when `.current` changes.**

---

# ✅ Chapter 13 Checklist

Before moving ahead, make sure you can explain:

- [ ] What is `useRef()`?
- [ ] Why do we use it?
- [ ] What is `.current`?
- [ ] How to access an input
- [ ] How to focus an input
- [ ] How to clear an input
- [ ] How to read an uncontrolled input
- [ ] How to store timer IDs
- [ ] How to store previous values
- [ ] `useRef` vs `useState`
- [ ] Why ref changes don't re-render
- [ ] Common `useRef` mistakes

---

# 🚀 NEXT CHAPTER

## 🌐 CHAPTER 14 — `useContext`

Next we will learn:

```text
🌐 Context
      ↓
🚫 Prop Drilling
      ↓
📦 Context Provider
      ↓
👤 Consumer
      ↓
useContext()
      ↓
🎨 Theme
🔐 Authentication
🌍 Language Settings
```

Type **`NEXT`** when you are ready for **Chapter 14 — `useContext`**.