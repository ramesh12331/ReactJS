# ⚡ REACTJS — CHAPTER 12
# `useEffect`

## 🎯 Chapter Goal

In this chapter, we will learn one of the most important React Hooks:

```jsx
useEffect()
```

You will understand:

- What is `useEffect()`?
- Why do we need it?
- What is a side effect?
- Basic syntax
- Dependency array
- Empty dependency array
- Effect with dependencies
- Cleanup function
- API calls
- Timers
- Event listeners
- Infinite loops
- Common mistakes
- Real-world usage

---

# 1️⃣ What is `useEffect()`?

`useEffect()` is a React Hook used to perform **side effects** in a component.

Basic example:

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {
        console.log("Component rendered");
    });

    return (
        <h1>Hello React</h1>
    );
}

export default App;
```

The important part is:

```jsx
useEffect(() => {
    console.log("Component rendered");
});
```

---

# 2️⃣ Why Do We Need `useEffect()`?

A React component mainly does this:

```text
Data
 ↓
Component
 ↓
UI
```

But sometimes our component needs to do something outside simply displaying UI.

For example:

```text
🌐 Call an API
⏰ Start a timer
🎧 Listen for browser events
📦 Read external data
🧹 Remove something when component leaves
```

These are called **side effects**.

`useEffect()` gives us a place to handle them.

---

# 3️⃣ What Is a Side Effect?

This word can sound difficult.

Don't worry.

A side effect simply means:

> Something your component does in addition to calculating and displaying its UI.

Examples:

### 🌐 API request

```text
React Component
      ↓
API Request
      ↓
Server
```

### ⏰ Timer

```text
React Component
      ↓
setInterval()
      ↓
Every second
```

### 🎧 Event listener

```text
Browser
   ↓
Window resize event
   ↓
React code
```

These are examples of side effects.

---

# 4️⃣ Real-World Analogy 🏠

Imagine a restaurant.

The restaurant displays the menu:

```text
Pizza
Burger
Biryani
```

That is similar to rendering UI.

But the restaurant also needs to:

```text
📞 Receive orders
🔥 Start cooking
⏰ Track preparation time
🧹 Clean tables
```

These are additional activities.

In React:

```text
UI rendering
     +
additional work
     ↓
side effects
```

`useEffect()` helps manage this additional work.

---

# 5️⃣ Basic Syntax

The basic syntax is:

```jsx
useEffect(() => {
    // side effect
});
```

You first import it:

```jsx
import { useEffect } from "react";
```

Then use it inside the component:

```jsx
function App() {

    useEffect(() => {
        console.log("Hello");
    });

    return <h1>Hello React</h1>;
}
```

---

# 6️⃣ First Simple Example

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {
        console.log("useEffect executed");
    });

    return (
        <h1>Hello React</h1>
    );
}

export default App;
```

Open the browser console.

You will see:

```text
useEffect executed
```

---

# 7️⃣ Where Can We Use `useEffect()`?

Common examples:

```text
🌐 API calls
⏰ Timers
🎧 Event listeners
📦 External subscriptions
🧹 Cleanup
```

The course source specifically includes API calls, timers, event listeners, and cleanup functions as core `useEffect` topics.

---

# 8️⃣ The Dependency Array ⭐⭐⭐

This is one of the most important concepts.

You may see:

```jsx
useEffect(() => {

    // code

}, []);
```

The second argument:

```jsx
[]
```

is called the **dependency array**.

Think of it as:

```text
"What should cause this effect to run again?"
```

---

# 9️⃣ Three Important Forms

There are three patterns you should understand.

### Pattern 1 — No dependency array

```jsx
useEffect(() => {
    console.log("Effect");
});
```

### Pattern 2 — Empty dependency array

```jsx
useEffect(() => {
    console.log("Effect");
}, []);
```

### Pattern 3 — Dependency array with values

```jsx
useEffect(() => {
    console.log("Effect");
}, [name]);
```

These behave differently.

---

# 🔟 Pattern 1 — No Dependency Array

Example:

```jsx
useEffect(() => {
    console.log("Effect executed");
});
```

There is no dependency array.

Conceptually, the effect can run after **each render**.

```text
Render
 ↓
Effect

Render
 ↓
Effect

Render
 ↓
Effect
```

This is why you should be careful when the effect causes something that triggers another render.

---

# 1️⃣1️⃣ Pattern 2 — Empty Dependency Array

```jsx
useEffect(() => {
    console.log("Effect executed");
}, []);
```

The empty array means:

```jsx
[]
```

There are no dependencies.

For learning purposes, remember:

> It runs after the initial render and does not re-run because of later state/prop changes.

This pattern is commonly used for initialization work such as fetching initial data.

---

# 1️⃣2️⃣ Pattern 3 — Dependency

Suppose we have:

```jsx
useEffect(() => {
    console.log("Name changed");
}, [name]);
```

Here:

```jsx
[name]
```

is the dependency array.

React watches `name`.

Conceptually:

```text
name changes
     ↓
effect runs
```

---

# 1️⃣3️⃣ Simple Example

```jsx
import { useEffect } from "react";

function App() {

    const name = "Ramesh";

    useEffect(() => {
        console.log("Name:", name);
    }, [name]);

    return (
        <h1>Hello {name}</h1>
    );
}

export default App;
```

Here:

```jsx
[name]
```

tells React that the effect depends on `name`.

---

# 1️⃣4️⃣ Important Connection With `useState`

You learned `useState()` earlier.

`useState` allows a component to store changing data.

For example:

```jsx
const [count, setCount] = useState(0);
```

When state changes:

```text
setCount()
   ↓
State changes
   ↓
Component renders again
```

Now suppose we want to perform an additional action whenever `count` changes.

We can use:

```jsx
useEffect(() => {
    console.log("Count changed");
}, [count]);
```

So:

```text
count changes
      ↓
component renders
      ↓
useEffect runs
```

This is a very important React pattern.

---

# 1️⃣5️⃣ Complete Example

```jsx
import { useState, useEffect } from "react";

function App() {

    const [count, setCount] = useState(0);

    useEffect(() => {
        console.log("Count changed:", count);
    }, [count]);

    return (
        <div>

            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

        </div>
    );
}

export default App;
```

---

# 1️⃣6️⃣ Dry Run 🧠

Initial state:

```text
count = 0
```

Component renders:

```text
Count: 0
```

Then:

```text
useEffect
```

runs.

Console:

```text
Count changed: 0
```

Click:

```text
Increase
```

Then:

```text
count = 1
```

React renders again.

Then the dependency:

```jsx
[count]
```

has changed.

So effect runs again:

```text
Count changed: 1
```

Click again:

```text
count = 2
```

Effect:

```text
Count changed: 2
```

---

# 1️⃣7️⃣ Flow Diagram

```text
👆 User clicks button
        ↓
setCount()
        ↓
count changes
        ↓
React re-renders
        ↓
React checks [count]
        ↓
count changed
        ↓
useEffect runs
```

---

# 1️⃣8️⃣ API Calls 🌐

One of the most common uses of `useEffect()` is loading data from an API.

For example:

```text
React Application
       ↓
   API request
       ↓
     Server
       ↓
     JSON
       ↓
React UI
```

A simple example:

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {

        fetch("https://example.com/api/products")
            .then((response) => response.json())
            .then((data) => {
                console.log(data);
            });

    }, []);

    return (
        <h1>Products</h1>
    );
}

export default App;
```

Here:

```jsx
[]
```

means this initialization effect is not intended to repeat because of component state/prop changes.

---

# 1️⃣9️⃣ Why API Calls Are Often Inside `useEffect`

Imagine:

```jsx
function App() {

    fetch("/api/products");

    return <h1>Products</h1>;
}
```

The component body can run again whenever React renders the component.

You generally don't want to accidentally start a new request every time the component renders.

So API synchronization is commonly placed inside an effect with an appropriate dependency list:

```jsx
useEffect(() => {

    fetch("/api/products");

}, []);
```

Later, you will learn a much more complete API pattern with:

```text
Loading
Success
Error
Data
```

in the API chapters.

---

# 2️⃣0️⃣ Timer Example ⏰

`useEffect()` can also be used with timers.

Example:

```jsx
import { useEffect } from "react";

function App() {

    useEffect(() => {

        const timer = setTimeout(() => {
            console.log("Hello after 2 seconds");
        }, 2000);

        return () => {
            clearTimeout(timer);
        };

    }, []);

    return (
        <h1>Timer Example</h1>
    );
}

export default App;
```

After approximately 2 seconds:

```text
Hello after 2 seconds
```

---

# 2️⃣1️⃣ What Is Cleanup?

Sometimes an effect starts something that needs to be stopped later.

Examples:

```text
⏰ Timer
🎧 Event listener
🔌 Subscription
```

React allows the effect to return a cleanup function.

Example:

```jsx
useEffect(() => {

    const timer = setTimeout(() => {
        console.log("Hello");
    }, 2000);

    return () => {
        clearTimeout(timer);
    };

}, []);
```

This part:

```jsx
return () => {
    clearTimeout(timer);
};
```

is the cleanup function.

---

# 2️⃣2️⃣ Why Do We Need Cleanup?

Imagine you create a timer:

```text
Component
   ↓
Timer starts
```

Later the component is removed.

If the timer still exists unnecessarily, it can cause unwanted work.

So:

```text
Start something
      ↓
Component lifecycle changes
      ↓
Clean it up
```

Think:

```text
🟢 Start
   ↓
🔴 Stop / Cleanup
```

---

# 2️⃣3️⃣ Event Listener Example

Suppose we want to listen to browser window resizing.

```jsx
useEffect(() => {

    function handleResize() {
        console.log("Window resized");
    }

    window.addEventListener("resize", handleResize);

    return () => {
        window.removeEventListener("resize", handleResize);
    };

}, []);
```

We do two things:

### Start

```jsx
window.addEventListener("resize", handleResize);
```

### Cleanup

```jsx
window.removeEventListener("resize", handleResize);
```

This follows an important pattern:

```text
Subscribe
   ↓
Use it
   ↓
Unsubscribe
```

---

# 2️⃣4️⃣ Effect Lifecycle 🧠

A useful mental model:

```text
Component renders
       ↓
Effect runs
       ↓
Effect performs side effect
       ↓
Dependencies may change
       ↓
Cleanup previous effect
       ↓
Run effect again
```

And when the component is removed:

```text
Component removed
       ↓
Cleanup
```

---

# 2️⃣5️⃣ Infinite Loop ⚠️

This is one of the most common `useEffect` mistakes.

Imagine:

```jsx
useEffect(() => {

    setCount(count + 1);

});
```

There is no dependency array.

What happens?

```text
Effect runs
   ↓
setCount()
   ↓
State changes
   ↓
Render
   ↓
Effect runs
   ↓
setCount()
   ↓
State changes
   ↓
Render
   ↓
Effect runs
   ↓
...
```

This can create an infinite render/update loop.

---

# 2️⃣6️⃣ Think Before Using `setState()` Inside `useEffect`

Whenever you write:

```jsx
useEffect(() => {
    setSomething(...);
});
```

ask yourself:

```text
❓ What causes this effect to run?

❓ Does the state update cause another render?

❓ Will that render run the effect again?

❓ Do I need dependencies?
```

This simple habit will prevent many bugs.

---

# 2️⃣7️⃣ Common Mistake — Missing Dependency

Suppose:

```jsx
useEffect(() => {
    console.log(userName);
}, []);
```

If the effect is intended to react to changes in `userName`, then `userName` belongs in the dependency list:

```jsx
useEffect(() => {
    console.log(userName);
}, [userName]);
```

The dependency array should describe the values that the effect uses and needs to synchronize with.

---

# 2️⃣8️⃣ Common Mistake — Using Effect for Everything

Do not think:

```text
"I have JavaScript code."
        ↓
"useEffect!"
```

❌ No.

Use effects for synchronization with things outside the component's normal rendering calculation.

Examples:

```text
🌐 API synchronization
⏰ Timer
🎧 Event listener
🔌 External subscription
```

Don't use `useEffect` just because you can.

---

# 2️⃣9️⃣ Common Mistake — Forgetting Cleanup

Bad:

```jsx
useEffect(() => {

    window.addEventListener("resize", handleResize);

}, []);
```

If the listener needs to be removed when the effect is no longer active, add cleanup:

```jsx
useEffect(() => {

    window.addEventListener("resize", handleResize);

    return () => {
        window.removeEventListener("resize", handleResize);
    };

}, []);
```

---

# 3️⃣0️⃣ Common Mistake — Wrong Timer Cleanup

Bad:

```jsx
useEffect(() => {

    const timer = setTimeout(() => {
        console.log("Hello");
    }, 2000);

    return () => {
        clearInterval(timer);
    };

}, []);
```

The timer was created with:

```jsx
setTimeout()
```

so clean it up with:

```jsx
clearTimeout()
```

Correct:

```jsx
return () => {
    clearTimeout(timer);
};
```

Remember:

```text
setTimeout
    ↓
clearTimeout

setInterval
    ↓
clearInterval
```

---

# 3️⃣1️⃣ Practical Example — Page Title

A simple practical use is synchronizing the browser document title.

```jsx
import { useState, useEffect } from "react";

function App() {

    const [count, setCount] = useState(0);

    useEffect(() => {
        document.title = `Count: ${count}`;
    }, [count]);

    return (
        <div>

            <h1>Count: {count}</h1>

            <button onClick={() => setCount(count + 1)}>
                Increase
            </button>

        </div>
    );
}

export default App;
```

Now when:

```text
count = 0
```

browser title becomes:

```text
Count: 0
```

After clicking:

```text
Count: 1
```

then:

```text
Count: 2
```

The effect synchronizes the browser title with React state.

---

# 3️⃣2️⃣ Important Mental Model

Don't memorize `useEffect` only as:

> "Code that runs after rendering."

Instead, think:

> **"I need to synchronize my component with something outside React's normal rendering."**

Examples:

```text
React State
     ↕
Browser title

React Component
     ↕
API

React Component
     ↕
Timer

React Component
     ↕
Event listener
```

This mental model will become very useful later.

---

# 3️⃣3️⃣ `useEffect` Syntax Cheat Sheet

### No dependency array

```jsx
useEffect(() => {
    // effect
});
```

Conceptually runs after renders.

---

### Empty dependency array

```jsx
useEffect(() => {
    // effect
}, []);
```

Runs after the initial render and does not re-run because of later state/prop changes.

---

### Dependency

```jsx
useEffect(() => {
    // effect
}, [count]);
```

Runs after the initial render and again when `count` changes.

---

### Cleanup

```jsx
useEffect(() => {

    // start

    return () => {
        // cleanup
    };

}, []);
```

---

# 🎤 3️⃣4️⃣ Interview Questions

## Q1. What is `useEffect()`?

`useEffect()` is a React Hook used to synchronize a component with external systems and perform side effects such as API requests, timers, and event listeners.

---

## Q2. What is a side effect?

A side effect is work performed outside the normal calculation of the UI.

Examples:

```text
API request
Timer
Event listener
Subscription
```

---

## Q3. What is a dependency array?

It tells React which reactive values the effect depends on.

Example:

```jsx
useEffect(() => {

}, [count]);
```

The effect is associated with `count`.

---

## Q4. What does `[]` mean?

An empty dependency array means the effect has no reactive dependencies.

It runs after the initial render and does not re-run due to later state/prop changes.

---

## Q5. What is a cleanup function?

A function returned from an effect that performs cleanup work.

Example:

```jsx
return () => {
    clearTimeout(timer);
};
```

---

## Q6. Where is cleanup useful?

Common examples:

```text
Timers
Event listeners
Subscriptions
```

---

## Q7. Can `useEffect()` cause an infinite loop?

Yes.

For example, an effect that updates state on every render can cause repeated renders and repeated effects.

---

## Q8. Why are API calls commonly placed inside `useEffect()`?

Because an API request is an external side effect, and the effect lets us control when that synchronization happens.

---

## Q9. Can `useEffect()` have dependencies?

Yes.

Example:

```jsx
useEffect(() => {
    console.log(count);
}, [count]);
```

---

## Q10. Why should we be careful with dependencies?

Because the dependency list controls when the effect is re-synchronized. Incorrect dependencies can cause stale values, unnecessary work, or repeated effects.

---

# 🧪 3️⃣5️⃣ Practice Tasks

### Task 1 — Console Message

Create a component that logs:

```text
React application started
```

using `useEffect()`.

---

### Task 2 — Page Title

Create:

```text
Count: 0
Count: 1
Count: 2
```

in the browser title when the count changes.

---

### Task 3 — Timer

Create an effect that starts a timer and properly cleans it up.

---

### Task 4 — Window Resize

Create a window resize listener.

When the browser size changes, print:

```text
Window resized
```

Remember to remove the listener in cleanup.

---

### Task 5 — API Practice

Create an effect that calls:

```text
/api/products
```

and prints the response to the console.

You don't need to build a complete UI yet.

---

# 🏆 3️⃣6️⃣ Mini Challenge

Create a **Product Loading Simulation**.

Your component should have:

```text
📦 Product page
      ↓
useEffect()
      ↓
API request
      ↓
console.log(data)
```

Start with:

```jsx
useEffect(() => {

    fetch("/api/products")
        .then((response) => response.json())
        .then((data) => {
            console.log(data);
        });

}, []);
```

Your goal is to understand:

```text
Component
    ↓
Effect
    ↓
API
    ↓
Response
    ↓
Data
```

Don't worry about loading/error UI yet. We will build that properly in the API chapters.

---

# 📝 3️⃣7️⃣ 10-Question Quiz

## Q1. Which Hook is commonly used for side effects?

### ✅ Answer

```jsx
useEffect()
```

---

## Q2. Is an API request a side effect?

### ✅ Answer

Yes.

---

## Q3. What is this?

```jsx
useEffect(() => {

}, []);
```

### ✅ Answer

An effect with an empty dependency array.

---

## Q4. What does this dependency mean?

```jsx
[count]
```

### ✅ Answer

The effect depends on `count` and can re-run when `count` changes.

---

## Q5. What is cleanup?

### ✅ Answer

Cleanup is code used to stop or remove something created by an effect.

Examples:

```text
clearTimeout()
clearInterval()
removeEventListener()
```

---

## Q6. Can an effect create an infinite loop?

### ✅ Answer

Yes, especially when the effect updates state and its dependencies cause it to run again repeatedly.

---

## Q7. What is commonly used to clean a `setTimeout()`?

### ✅ Answer

```jsx
clearTimeout()
```

---

## Q8. What is commonly used to clean a `setInterval()`?

### ✅ Answer

```jsx
clearInterval()
```

---

## Q9. What is the purpose of the dependency array?

### ✅ Answer

It controls which reactive values the effect is synchronized with and therefore when the effect should re-run.

---

## Q10. Should we use `useEffect()` for every piece of JavaScript code?

### ❌ Answer

No.

Use it when you need to synchronize with external systems or perform side effects.

---

# 📚 3️⃣8️⃣ Quick Revision

| Concept | Meaning |
|---|---|
| ⚡ `useEffect` | Handles side effects/synchronization |
| 🌐 API | External data request |
| ⏰ Timer | Time-based side effect |
| 🎧 Event listener | Browser event |
| 📦 Dependency | Value effect depends on |
| `[]` | No reactive dependencies |
| 🧹 Cleanup | Stop/remove external work |
| 🔄 Re-run | Effect runs again when dependencies change |
| ⚠️ Infinite loop | Repeated effect/state updates |
| 🧠 Main idea | Synchronize with external systems |

---

# 🧠 3️⃣9️⃣ Final Memory Trick

Remember:

```text
⚛️ React Component
       ↓
   useEffect()
       ↓
🌐 API
⏰ Timer
🎧 Event
🔌 Subscription
       ↓
🧹 Cleanup
```

And remember the three patterns:

```jsx
// 1. No dependency array
useEffect(() => {
    // effect
});
```

```jsx
// 2. Empty dependency array
useEffect(() => {
    // effect
}, []);
```

```jsx
// 3. Dependency
useEffect(() => {
    // effect
}, [count]);
```

### ⭐ One-line memory

> **`useEffect` = synchronize React with something outside the normal UI rendering.**

---

# ✅ Chapter 12 Checklist

Before moving forward, make sure you understand:

- [ ] What is `useEffect()`?
- [ ] What is a side effect?
- [ ] Why do we need effects?
- [ ] Basic `useEffect` syntax
- [ ] Dependency array
- [ ] Empty dependency array
- [ ] Effects with dependencies
- [ ] Cleanup functions
- [ ] API calls
- [ ] Timers
- [ ] Event listeners
- [ ] Infinite loops
- [ ] Common mistakes
- [ ] How to think about effects

---

# 🚀 NEXT CHAPTER

## 🎯 CHAPTER 13 — `useRef`

Next we will learn:

```text
🎯 useRef
   ↓
🌐 DOM References
   ↓
⌨️ Access Input
   ↓
🎯 Focus Input
   ↓
💾 Store Values
   ↓
⏱️ Timers
   ↓
⚛️ useRef vs useState
```

Type **`NEXT`** when you are ready for **Chapter 13 — `useRef`**.