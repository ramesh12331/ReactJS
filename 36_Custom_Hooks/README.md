# 🚀 CHAPTER 36 — CUSTOM HOOKS

## 🎯 What You Will Learn

In this chapter:

- 🪝 What is a Custom Hook?
- 🤔 Why do we need Custom Hooks?
- 📜 Rules of Hooks
- 🧩 Creating your own Hook
- 📡 `useFetch`
- 💾 `useLocalStorage`
- ⏱️ `useDebounce`
- 🔘 `useToggle`
- 📝 `useForm`
- ♻️ Reusing logic
- ⚠️ Common mistakes
- 💼 Interview questions
- 🧩 Mini challenge
- 📝 Quiz

---

# 1. 🧠 What Is a Custom Hook?

A **Custom Hook** is a JavaScript function that lets us **reuse React logic** between components.

A Custom Hook normally starts with:

```text
use...
```

Examples:

```text
useFetch()
useLocalStorage()
useDebounce()
useToggle()
useForm()
```

### Simple definition

> A Custom Hook is a reusable function that contains React Hook logic.

---

# 2. 🤔 Why Do We Need Custom Hooks?

Imagine we have two components.

### Component 1

```jsx
function UserPage() {
    // fetch users
}
```

### Component 2

```jsx
function AdminPage() {
    // fetch users
}
```

Both components need the same API logic.

Without a Custom Hook:

```text
UserPage
   ↓
fetch logic

AdminPage
   ↓
same fetch logic
```

We are duplicating code.

With a Custom Hook:

```text
             useFetch()
             /       \
            ↓         ↓
       UserPage   AdminPage
```

Now both components can reuse the same logic.

---

# 3. 🏠 Real-World Analogy

Imagine you have a washing machine.

You don't need to understand every internal motor operation every time you use it.

You simply use:

```text
Start
```

Similarly, a component doesn't need to know all the internal logic of a Custom Hook.

For example:

```jsx
const { data, loading, error } = useFetch(url);
```

The component simply uses the result.

Internally:

```text
useFetch()
   ↓
useState()
useEffect()
fetch()
error handling
   ↓
return data
```

---

# 4. 🧠 Custom Hook Flow

```text
Component
    ↓
Custom Hook
    ↓
React Hooks
    ↓
Reusable Logic
    ↓
Result
    ↓
Component UI
```

Example:

```text
App
 ↓
useFetch()
 ↓
useState()
useEffect()
fetch()
 ↓
data
loading
error
 ↓
UI
```

---

# 5. 🧩 Basic Custom Hook

Let's create:

```text
src/
└── hooks/
    └── useCounter.js
```

Code:

```js
import { useState } from "react";

function useCounter() {

    const [count, setCount] = useState(0);

    const increment = () => {
        setCount(count + 1);
    };

    const decrement = () => {
        setCount(count - 1);
    };

    return {
        count,
        increment,
        decrement
    };
}

export default useCounter;
```

---

# 6. 🔍 Understanding `useCounter`

We have:

```js
function useCounter() {
```

Because this is a Hook, we use the naming convention:

```text
useCounter
```

not:

```text
counter
```

---

Inside:

```js
const [count, setCount] = useState(0);
```

We have state.

Then:

```js
const increment = () => {
    setCount(count + 1);
};
```

and:

```js
const decrement = () => {
    setCount(count - 1);
};
```

Finally:

```js
return {
    count,
    increment,
    decrement
};
```

The Hook gives these values/functions to the component.

---

# 7. 🖥️ Using the Custom Hook

In `App.jsx`:

```jsx
import useCounter from "./hooks/useCounter";

function App() {

    const {
        count,
        increment,
        decrement
    } = useCounter();

    return (
        <div>

            <h1>Count: {count}</h1>

            <button onClick={increment}>
                +
            </button>

            <button onClick={decrement}>
                -
            </button>

        </div>
    );
}

export default App;
```

---

# 8. 🧠 Dry Run

Initial:

```text
count = 0
```

UI:

```text
Count: 0
```

Click:

```text
+
```

Then:

```text
increment()
     ↓
setCount(1)
     ↓
React re-renders
     ↓
Count: 1
```

Click again:

```text
Count: 2
```

---

# 9. ♻️ Why Is This Better?

Without Custom Hook:

```text
Component A
   ├── state
   ├── increment
   └── decrement

Component B
   ├── state
   ├── increment
   └── decrement
```

With Custom Hook:

```text
             useCounter()
              /       \
             ↓         ↓
       Component A  Component B
```

The logic is reused.

---

# 10. 📜 Rules of Hooks

Custom Hooks follow React's Rules of Hooks.

## Rule 1 — Call Hooks at the Top Level

Correct:

```js
function useCounter() {

    const [count, setCount] =
        useState(0);

}
```

Wrong:

```js
function useCounter() {

    if (someCondition) {

        const [count, setCount] =
            useState(0);

    }

}
```

Don't conditionally call Hooks.

---

# 11. ❌ Don't Call Hooks Inside Loops

Wrong:

```js
for (let i = 0; i < 5; i++) {

    const [count, setCount] =
        useState(0);

}
```

Hooks should not be called inside loops.

---

# 12. ❌ Don't Call Hooks Inside Normal Functions

Wrong:

```js
function calculate() {

    const [value, setValue] =
        useState(0);

}
```

React Hooks should be called from:

```text
React Components
        OR
Custom Hooks
```

---

# 13. 🧠 Naming Rule

A Custom Hook should normally start with:

```text
use
```

Examples:

```text
useFetch
useForm
useToggle
useDebounce
useCounter
```

This is important because it communicates that the function follows Hook rules and uses React Hook logic.

---

# 14. 📡 Custom Hook — `useFetch`

Now let's build a more practical Custom Hook.

Create:

```text
src/hooks/useFetch.js
```

Code:

```js
import { useEffect, useState } from "react";

function useFetch(url) {

    const [data, setData] =
        useState(null);

    const [loading, setLoading] =
        useState(true);

    const [error, setError] =
        useState(null);


    useEffect(() => {

        async function fetchData() {

            try {

                setLoading(true);
                setError(null);

                const response =
                    await fetch(url);

                if (!response.ok) {
                    throw new Error(
                        "Failed to fetch data"
                    );
                }

                const result =
                    await response.json();

                setData(result);

            } catch (error) {

                setError(error.message);

            } finally {

                setLoading(false);

            }

        }

        fetchData();

    }, [url]);


    return {
        data,
        loading,
        error
    };
}

export default useFetch;
```

---

# 15. 🔍 Understanding `useFetch`

The Hook receives:

```js
function useFetch(url)
```

So we can call:

```js
useFetch(
    "https://example.com/users"
);
```

Inside it manages:

```text
data
loading
error
```

Therefore:

```text
useFetch()
   ↓
┌─────────────┐
│ data        │
│ loading     │
│ error       │
└─────────────┘
```

---

# 16. 🖥️ Using `useFetch`

```jsx
import useFetch from "./hooks/useFetch";

function App() {

    const {
        data,
        loading,
        error
    } = useFetch(
        "https://jsonplaceholder.typicode.com/users"
    );


    if (loading) {
        return <h2>⏳ Loading...</h2>;
    }


    if (error) {
        return <h2>❌ {error}</h2>;
    }


    return (
        <div>

            <h1>Users</h1>

            {data?.map((user) => (

                <div key={user.id}>

                    <h2>
                        {user.name}
                    </h2>

                    <p>
                        {user.email}
                    </p>

                </div>

            ))}

        </div>
    );
}

export default App;
```

---

# 17. 🔄 `useFetch` Flow

```text
App
 ↓
useFetch(url)
 ↓
useEffect()
 ↓
fetch(url)
 ↓
API
 ↓
┌───────────────┐
│               │
↓               ↓
Success        Error
↓               ↓
data           error
│               │
└───────┬───────┘
        ↓
       UI
```

---

# 18. 💾 Custom Hook — `useLocalStorage`

A very useful Custom Hook is:

```text
useLocalStorage()
```

It allows us to synchronize React state with browser local storage.

Create:

```text
src/hooks/useLocalStorage.js
```

```js
import { useState } from "react";

function useLocalStorage(key, initialValue) {

    const [value, setValue] = useState(() => {

        const storedValue =
            localStorage.getItem(key);

        return storedValue !== null
            ? JSON.parse(storedValue)
            : initialValue;
    });


    const setStoredValue = (newValue) => {

        setValue(newValue);

        localStorage.setItem(
            key,
            JSON.stringify(newValue)
        );
    };


    return [
        value,
        setStoredValue
    ];
}

export default useLocalStorage;
```

---

# 19. 🧠 Why `JSON.stringify()`?

`localStorage` stores strings.

Suppose:

```js
const user = {
    name: "Ramesh"
};
```

We cannot directly store the object as an object.

So:

```js
JSON.stringify(user)
```

converts:

```text
Object
  ↓
JSON String
```

And:

```js
JSON.parse(value)
```

converts:

```text
JSON String
  ↓
Object
```

---

# 20. 🖥️ Using `useLocalStorage`

```jsx
import useLocalStorage
    from "./hooks/useLocalStorage";

function App() {

    const [
        name,
        setName
    ] = useLocalStorage(
        "name",
        ""
    );


    return (
        <div>

            <h1>
                Hello {name}
            </h1>

            <input
                value={name}
                onChange={(e) =>
                    setName(e.target.value)
                }
            />

        </div>
    );
}

export default App;
```

Now if you type:

```text
Ramesh
```

the value can be persisted in:

```text
localStorage
```

---

# 21. 🔘 Custom Hook — `useToggle`

This is a very simple and useful Hook.

```js
import { useState } from "react";

function useToggle(initialValue = false) {

    const [value, setValue] =
        useState(initialValue);


    const toggle = () => {

        setValue(
            previousValue =>
                !previousValue
        );

    };


    return [
        value,
        toggle
    ];
}

export default useToggle;
```

---

# 22. 🖥️ Using `useToggle`

```jsx
import useToggle from "./hooks/useToggle";

function App() {

    const [
        isOpen,
        toggle
    ] = useToggle(false);


    return (
        <div>

            <button onClick={toggle}>
                Toggle
            </button>

            {isOpen && (
                <p>
                    Hello React!
                </p>
            )}

        </div>
    );
}

export default App;
```

Flow:

```text
false
 ↓
toggle()
 ↓
true
 ↓
toggle()
 ↓
false
```

---

# 23. ⏱️ Custom Hook — `useDebounce`

`useDebounce` is very useful for:

- 🔍 Search boxes
- API search
- Filtering
- Autocomplete

Imagine the user types:

```text
R
Ra
Ram
Rame
Ramesh
```

If we call the API after every keystroke:

```text
R       → API
Ra      → API
Ram     → API
Rame    → API
Ramesh  → API
```

That's unnecessary.

Debouncing waits until the user stops typing.

---

# 24. 🧠 Debounce Idea

```text
User types
   ↓
wait
   ↓
user types again
   ↓
restart timer
   ↓
user stops
   ↓
timer completes
   ↓
execute action
```

---

# 25. `useDebounce` Code

```js
import { useEffect, useState } from "react";

function useDebounce(value, delay = 500) {

    const [debouncedValue, setDebouncedValue] =
        useState(value);


    useEffect(() => {

        const timer = setTimeout(() => {

            setDebouncedValue(value);

        }, delay);


        return () => {

            clearTimeout(timer);

        };

    }, [value, delay]);


    return debouncedValue;
}

export default useDebounce;
```

---

# 26. 🖥️ Using `useDebounce`

```jsx
import { useState } from "react";

import useDebounce
    from "./hooks/useDebounce";


function App() {

    const [search, setSearch] =
        useState("");


    const debouncedSearch =
        useDebounce(search, 500);


    return (
        <div>

            <input
                value={search}
                onChange={(e) =>
                    setSearch(e.target.value)
                }
            />

            <p>
                Search:
                {debouncedSearch}
            </p>

        </div>
    );
}

export default App;
```

---

# 27. 🧠 Debounce Dry Run

Suppose the user types:

```text
R
```

Timer starts.

Then:

```text
Ra
```

Old timer is cleared.

New timer starts.

Then:

```text
Ram
```

Old timer is cleared.

New timer starts.

Finally user stops typing.

After 500ms:

```text
debouncedSearch = "Ram"
```

---

# 28. 📝 Custom Hook — `useForm`

Forms often contain repeated logic.

Instead of writing:

```text
name state
email state
password state
handleChange
handleSubmit
```

for every form, we can create:

```text
useForm()
```

Basic version:

```js
import { useState } from "react";

function useForm(initialValues) {

    const [values, setValues] =
        useState(initialValues);


    const handleChange = (event) => {

        const {
            name,
            value
        } = event.target;


        setValues((previousValues) => ({

            ...previousValues,

            [name]: value

        }));

    };


    const resetForm = () => {

        setValues(initialValues);

    };


    return {
        values,
        handleChange,
        resetForm
    };
}

export default useForm;
```

---

# 29. 🖥️ Using `useForm`

```jsx
import useForm from "./hooks/useForm";

function App() {

    const {
        values,
        handleChange,
        resetForm
    } = useForm({

        name: "",
        email: ""

    });


    return (
        <div>

            <input
                name="name"
                value={values.name}
                onChange={handleChange}
                placeholder="Name"
            />

            <input
                name="email"
                value={values.email}
                onChange={handleChange}
                placeholder="Email"
            />

            <button onClick={resetForm}>
                Reset
            </button>

            <pre>
                {JSON.stringify(
                    values,
                    null,
                    2
                )}
            </pre>

        </div>
    );
}

export default App;
```

---

# 30. 🧠 Why Custom Hooks Are Powerful

Without Custom Hooks:

```text
Component A
   ↓
fetch logic

Component B
   ↓
same fetch logic

Component C
   ↓
same fetch logic
```

With Custom Hooks:

```text
             useFetch()
           /     |      \
          ↓      ↓       ↓
      Component Component Component
```

We separate:

```text
UI
```

from:

```text
Reusable Logic
```

---

# 31. 🏗️ Recommended Folder Structure

For a larger application:

```text
src/
│
├── components/
│
├── pages/
│
├── hooks/
│   ├── useFetch.js
│   ├── useForm.js
│   ├── useToggle.js
│   ├── useDebounce.js
│   └── useLocalStorage.js
│
├── services/
│
├── features/
│
└── App.jsx
```

The `hooks` folder keeps reusable Hooks organized.

---

# 32. 🧠 Custom Hook vs Component

This is an important interview question.

### Component

Returns UI:

```jsx
function User() {

    return (
        <h1>
            User
        </h1>
    );
}
```

### Custom Hook

Returns reusable logic/data:

```js
function useCounter() {

    return {
        count,
        increment
    };
}
```

Remember:

```text
Component
    ↓
UI

Custom Hook
    ↓
Reusable Logic
```

---

# 33. 🧠 Custom Hook vs Normal Function

A normal function:

```js
function add(a, b) {
    return a + b;
}
```

doesn't use React Hooks.

A Custom Hook:

```js
function useCounter() {

    const [count, setCount] =
        useState(0);

}
```

uses React Hooks.

Therefore:

```text
Normal Function
→ General JavaScript logic

Custom Hook
→ Reusable React logic
```

---

# 34. ⚠️ Common Mistake #1

Naming a Custom Hook without `use`.

Avoid:

```js
function counter() {
```

Prefer:

```js
function useCounter() {
```

---

# 35. ⚠️ Common Mistake #2

Calling a Hook conditionally.

Wrong:

```js
if (loggedIn) {

    useEffect(() => {
        // ...
    });

}
```

Hooks should be called consistently at the top level.

---

# 36. ⚠️ Common Mistake #3

Calling a Custom Hook inside an event handler.

Wrong:

```jsx
function handleClick() {

    const data = useFetch(url);

}
```

Instead, call the Hook at the component's top level:

```jsx
function App() {

    const data = useFetch(url);

    function handleClick() {
        // use data here
    }

}
```

---

# 37. ⚠️ Common Mistake #4

Putting UI inside a Hook.

A Hook should generally provide logic/state.

Avoid turning it into a component.

Think:

```text
Hook
 ↓
Logic

Component
 ↓
UI
```

---

# 38. ⚠️ Common Mistake #5

Creating a Custom Hook when there is no reusable logic.

For example, don't create:

```text
useAdd()
```

just to wrap:

```js
const result = a + b;
```

A normal function may be enough.

Custom Hooks are useful when you are reusing **React-related logic**.

---

# 39. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is a Custom Hook?

A Custom Hook is a reusable function that contains React Hook logic.

### Q2. Why do Custom Hooks start with `use`?

The `use` naming convention identifies the function as a Hook and communicates that Hook rules apply to it.

### Q3. Can a Custom Hook use `useState()`?

Yes.

```js
function useCounter() {

    const [count, setCount] =
        useState(0);

}
```

---

## 🟡 Intermediate

### Q4. Why use Custom Hooks?

To reuse stateful or other React-related logic between components.

### Q5. Can one Custom Hook use another Custom Hook?

Yes.

Example:

```js
function useSomething() {

    const value =
        useLocalStorage("name", "");

}
```

### Q6. What is the difference between a Custom Hook and a component?

A component primarily returns UI, while a Custom Hook primarily encapsulates reusable React logic.

---

## 🔴 Advanced

### Q7. Can Custom Hooks share state between components?

They can share **logic**, but calling the same Custom Hook from two components does not automatically create one shared state instance.

For example:

```text
Component A
   ↓
useCounter()
   ↓
its own state


Component B
   ↓
useCounter()
   ↓
its own state
```

If you need truly shared state, use mechanisms such as:

```text
Context
Redux
Zustand
```

depending on the application.

### Q8. What are the Rules of Hooks?

Hooks should be called:

- At the top level
- From React components or Custom Hooks

and not conditionally, inside loops, or inside ordinary nested functions.

### Q9. What is `useDebounce()` useful for?

It is commonly useful for delaying an action until the user stops changing a value, such as in search inputs.

### Q10. Why are Custom Hooks useful in large applications?

They help separate reusable React logic from UI and reduce repeated logic across components.

---

# 40. 📝 PRACTICE

## Practice 1

Create:

```text
useCounter()
```

Support:

```text
+
-
reset
```

---

## Practice 2

Create:

```text
useToggle()
```

Use it to show/hide:

```text
Login Form
```

---

## Practice 3

Create:

```text
useLocalStorage()
```

Store:

```text
username
```

---

## Practice 4

Create:

```text
useFetch()
```

Fetch users from an API and display:

```text
Name
Email
```

---

## Practice 5

Create:

```text
useDebounce()
```

Build a search box.

---

# 41. 🧩 MINI CHALLENGE

Build:

# 🔍 Search Users App

Requirements:

```text
Search input
      ↓
useDebounce()
      ↓
API request
      ↓
Users
```

Example:

```text
Search:
[Ramesh____________]
```

After the user stops typing:

```text
⏳ Searching...
```

Then:

```text
👤 Ramesh

📧 ramesh@example.com
```

Create these files:

```text
src/
│
├── hooks/
│   ├── useFetch.js
│   └── useDebounce.js
│
└── App.jsx
```

The goal is to separate:

```text
API logic
```

from:

```text
UI logic
```

---

# 42. 📝 10-QUESTION QUIZ

### Q1. What is a Custom Hook?

**Answer:** A reusable function that contains React Hook logic.

---

### Q2. What should a Custom Hook normally start with?

**Answer:**

```text
use
```

---

### Q3. Can Custom Hooks use `useState()`?

**Answer:** Yes.

---

### Q4. Can Custom Hooks use `useEffect()`?

**Answer:** Yes.

---

### Q5. Where should Hooks be called?

**Answer:** At the top level of React components or Custom Hooks.

---

### Q6. Can Hooks be called inside `if` statements?

**Answer:** No.

---

### Q7. What does `useFetch()` commonly provide?

**Answer:**

```text
data
loading
error
```

---

### Q8. What is `useDebounce()` useful for?

**Answer:** Delaying an action until the user stops changing a value, commonly for search inputs.

---

### Q9. Does calling the same Custom Hook automatically create shared state?

**Answer:** No. Each component normally gets its own Hook state instance.

---

### Q10. What is the main purpose of Custom Hooks?

**Answer:**

> **Reuse React-related logic across components.**

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
        🪝 CUSTOM HOOK
              ↓
       Reusable Logic
              ↓
     ┌────────┼────────┐
     ↓        ↓        ↓
 useState  useEffect  Other Hooks
     │        │        │
     └────────┼────────┘
              ↓
        Return Logic/Data
              ↓
          ⚛️ Component
              ↓
               UI
```

### ⭐ Most important examples

```text
useCounter()
    ↓
Counter logic

useFetch()
    ↓
API logic

useForm()
    ↓
Form logic

useToggle()
    ↓
Boolean toggle logic

useDebounce()
    ↓
Delayed-value logic

useLocalStorage()
    ↓
localStorage + React state
```

### ⭐ One-line memory

> **Component = UI, Custom Hook = reusable React logic.**

---

# ✅ CHAPTER 36 COMPLETE

You now understand:

- 🪝 Custom Hooks
- Why Custom Hooks are useful
- Rules of Hooks
- `useCounter`
- `useFetch`
- `useLocalStorage`
- `useToggle`
- `useDebounce`
- `useForm`
- Custom Hook vs Component
- Custom Hook vs normal function
- Reusable React logic
- Common mistakes
- Interview questions
- Practice exercises

---

# 🚀 NEXT CHAPTER

## Chapter 37 — React Performance

We will learn:

```text
⚡ What is React Performance?
🔄 Re-rendering
🐌 Why components become slow
🧠 Memoization
React.memo
useMemo
useCallback
📦 Lazy loading
🔍 Performance optimization
⚠️ Common performance mistakes
```

Type **NEXT** to continue.