# 🖱️ REACTJS — CHAPTER 7
# ⚡ EVENTS

---

# 🎯 Chapter Goal

After this chapter, you will understand:

- 🖱️ What is an event?
- ⚛️ React event handling
- 🖱️ `onClick`
- ⌨️ `onChange`
- 📝 `onSubmit`
- 🖱️ `onMouseEnter`
- 🖱️ `onMouseLeave`
- ⌨️ Keyboard events
- 📦 Event object
- ⚙️ Event handlers
- 🎯 Passing arguments
- ❌ Common event mistakes

---

# 1️⃣ What Is an Event?

## 🧠 Simple Definition

An **event** is something that happens because of a user action or another interaction.

For example:

```text
🖱️ User clicks a button
⌨️ User types in an input
📝 User submits a form
🖱️ User moves the mouse
⌨️ User presses a keyboard key
```

These are all events.

---

# 2️⃣ Real-World Example 🏠

Imagine a doorbell.

```text
Person presses button
        ↓
     🔔 Event
        ↓
   Doorbell rings
```

In a React application:

```text
User clicks button
        ↓
    🖱️ Event
        ↓
React runs a function
        ↓
Something happens
```

Example:

```text
Click button
     ↓
showMessage()
     ↓
"Hello!"
```

---

# 3️⃣ Why Do We Need Events?

A website should respond to users.

For example:

```text
User Action              Application Response
------------------------------------------------
Click Login              → Login process
Click Add to Cart        → Add product
Type in Search           → Update search
Submit Form              → Send data
Move mouse               → Change UI
Press Enter              → Search/submit
```

Without event handling, your UI would mostly just display information.

Events make the UI interactive.

---

# 4️⃣ Basic `onClick` 🖱️

The most common React event is:

```jsx id="7q5g9s"
onClick
```

Example:

```jsx id="5w2x9p"
function App() {

    function showMessage() {
        alert("Hello Ramesh!");
    }

    return (
        <button onClick={showMessage}>
            Click Me
        </button>
    );
}

export default App;
```

When the user clicks:

```text
🖱️ Click
   ↓
showMessage()
   ↓
Alert
```

---

# 5️⃣ Understanding `onClick`

Look carefully:

```jsx id="a8p7k2"
<button onClick={showMessage}>
```

There are two parts:

```text
onClick
   +
showMessage
```

`onClick` means:

> "When this element is clicked..."

`showMessage` means:

> "...run this function."

---

# 6️⃣ Event Handler

A function that handles an event is called an **event handler**.

Example:

```jsx id="f8x2mc"
function handleClick() {
    alert("Button clicked");
}
```

Then:

```jsx id="2v4s7x"
<button onClick={handleClick}>
    Click
</button>
```

Here:

```text
handleClick
    ↓
Event Handler
```

### 🧠 Memory Trick

> **Event = Something happens**  
> **Handler = Function that handles it**

---

# 7️⃣ Naming Event Handlers

A common naming style is:

```text
handleClick
handleChange
handleSubmit
handleMouseEnter
handleMouseLeave
handleKeyDown
handleKeyUp
```

For example:

```jsx id="w8d3xm"
function handleClick() {
    console.log("Clicked");
}
```

This makes your code easy to understand.

---

# 8️⃣ Very Important: Don't Call the Function Immediately ❌

### ❌ Wrong

```jsx id="e2q7hy"
<button onClick={handleClick()}>
    Click
</button>
```

Why?

Because:

```text
handleClick()
```

means:

> Call the function now.

It is not waiting for the click.

---

### ✅ Correct

```jsx id="m5n9rx"
<button onClick={handleClick}>
    Click
</button>
```

This means:

> Give React the function to call when the click happens.

---

# 9️⃣ Simple Memory Trick ⭐

Remember:

```text
onClick={handleClick}
```

means:

```text
"When clicked → call handleClick"
```

While:

```text
onClick={handleClick()}
```

means:

```text
"Call handleClick immediately"
```

---

# 🔟 `onClick` With an Arrow Function

We can also write:

```jsx id="c7x3fa"
<button
    onClick={() => {
        alert("Hello!");
    }}
>
    Click Me
</button>
```

Here the function is written directly inside `onClick`.

---

# 1️⃣1️⃣ When Should We Use an Arrow Function?

Arrow functions are useful when we need to do something small directly.

Example:

```jsx id="w0b7pk"
<button onClick={() => alert("Hello")}>
    Click
</button>
```

Or when we need to pass an argument:

```jsx id="h6k4mr"
<button onClick={() => handleUser("Ramesh")}>
    Click
</button>
```

We will learn arguments shortly.

---

# 1️⃣2️⃣ `onChange` ⌨️

`onChange` is commonly used with form inputs.

Example:

```jsx id="3s8y7k"
function App() {

    function handleChange(event) {
        console.log(event.target.value);
    }

    return (
        <input
            type="text"
            onChange={handleChange}
        />
    );
}

export default App;
```

When the user types:

```text
R
Ra
Ram
Rame
Rames
Ramesh
```

the `onChange` event runs as the input value changes.

---

# 1️⃣3️⃣ Understanding `event.target.value`

This is very important.

Suppose the user types:

```text
Ramesh
```

The event object contains information about the event.

This:

```jsx id="h0d7qk"
event.target
```

refers to the element that triggered the event.

And:

```jsx id="4b1m9f"
event.target.value
```

gives the current input value.

So:

```text
event
 ↓
target
 ↓
value
 ↓
"Ramesh"
```

---

# 1️⃣4️⃣ Practical `onChange` Example

```jsx id="v3x8sm"
function App() {

    function handleChange(event) {

        console.log(event.target.value);

    }

    return (
        <div>

            <h1>Enter Your Name</h1>

            <input
                type="text"
                onChange={handleChange}
            />

        </div>
    );
}

export default App;
```

Open the browser console.

Type:

```text
Ramesh
```

You will see values changing as you type.

---

# 1️⃣5️⃣ `onSubmit` 📝

`onSubmit` is used when a form is submitted.

Example:

```jsx id="r5z3yq"
function App() {

    function handleSubmit(event) {

        event.preventDefault();

        console.log("Form submitted");

    }

    return (
        <form onSubmit={handleSubmit}>

            <input type="text" />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}

export default App;
```

---

# 1️⃣6️⃣ What Is `event.preventDefault()`?

Normally, when an HTML form is submitted, the browser may perform its default form behavior.

In React, we often want to handle the submission ourselves.

So:

```jsx id="y5h8r2"
event.preventDefault();
```

means:

> Stop the browser's default action for this event.

Then React can handle the form submission using JavaScript.

---

# 1️⃣7️⃣ Form Submission Flow

```text
User fills form
       ↓
Clicks Submit
       ↓
onSubmit
       ↓
handleSubmit()
       ↓
preventDefault()
       ↓
React handles form
```

This becomes very important when we build login, registration, and CRUD forms.

---

# 1️⃣8️⃣ `onMouseEnter` 🖱️

This event happens when the mouse enters an element.

Example:

```jsx id="b5c8nm"
function App() {

    function handleMouseEnter() {
        console.log("Mouse entered");
    }

    return (
        <div
            onMouseEnter={handleMouseEnter}
        >
            Move mouse here
        </div>
    );
}

export default App;
```

Flow:

```text
Mouse enters element
        ↓
onMouseEnter
        ↓
handleMouseEnter()
```

---

# 1️⃣9️⃣ `onMouseLeave`

This event happens when the mouse leaves an element.

```jsx id="p4x6ks"
function App() {

    function handleMouseLeave() {
        console.log("Mouse left");
    }

    return (
        <div
            onMouseLeave={handleMouseLeave}
        >
            Move mouse away
        </div>
    );
}

export default App;
```

---

# 2️⃣0️⃣ Mouse Events Together

```jsx id="9k5c3v"
function App() {

    function handleMouseEnter() {
        console.log("Mouse entered");
    }

    function handleMouseLeave() {
        console.log("Mouse left");
    }

    return (
        <div
            onMouseEnter={handleMouseEnter}
            onMouseLeave={handleMouseLeave}
        >
            Hover over me
        </div>
    );
}

export default App;
```

Flow:

```text
Mouse enters
    ↓
onMouseEnter

Mouse leaves
    ↓
onMouseLeave
```

---

# 2️⃣1️⃣ Keyboard Events ⌨️

React supports keyboard events.

Common ones:

```text
onKeyDown
onKeyUp
```

---

# 2️⃣2️⃣ `onKeyDown`

`onKeyDown` runs when a key is pressed down.

Example:

```jsx id="w2j7qs"
function App() {

    function handleKeyDown(event) {
        console.log(event.key);
    }

    return (
        <input
            type="text"
            onKeyDown={handleKeyDown}
        />
    );
}

export default App;
```

If the user presses:

```text
A
```

you may get:

```text
a
```

from:

```jsx id="5n7m0c"
event.key
```

---

# 2️⃣3️⃣ `onKeyUp`

`onKeyUp` runs when the key is released.

```jsx id="q8w3md"
function App() {

    function handleKeyUp(event) {
        console.log(event.key);
    }

    return (
        <input
            type="text"
            onKeyUp={handleKeyUp}
        />
    );
}

export default App;
```

---

# 2️⃣4️⃣ Detecting Enter Key

This is very useful in search boxes.

```jsx id="4k8z1c"
function App() {

    function handleKeyDown(event) {

        if (event.key === "Enter") {
            console.log("Enter pressed");
        }

    }

    return (
        <input
            type="text"
            onKeyDown={handleKeyDown}
        />
    );
}

export default App;
```

Flow:

```text
User presses Enter
       ↓
onKeyDown
       ↓
event.key
       ↓
"Enter"
       ↓
Run code
```

---

# 2️⃣5️⃣ Event Object 📦

React gives your event handler information about the event.

Example:

```jsx id="g5q7wp"
function handleClick(event) {

    console.log(event);

}
```

Here:

```text
event
```

is the **event object**.

It contains information related to the event.

---

# 2️⃣6️⃣ Useful Event Object Properties

Some useful properties include:

```text
event.target
event.target.value
event.target.checked
event.key
event.type
```

### Example

For an input:

```jsx id="z1b6rn"
event.target.value
```

gets the input value.

For a checkbox:

```jsx id="k4v8tc"
event.target.checked
```

tells whether it is checked.

For keyboard events:

```jsx id="h9p3vq"
event.key
```

tells which key was pressed.

---

# 2️⃣7️⃣ Event Object Example

```jsx id="s5w8pk"
function App() {

    function handleClick(event) {

        console.log("Type:", event.type);

        console.log("Element:", event.target);

    }

    return (
        <button onClick={handleClick}>
            Click Me
        </button>
    );
}

export default App;
```

The browser console will show information about the click event.

---

# 2️⃣8️⃣ `event.target`

Suppose:

```jsx id="n4k8s2"
<button onClick={handleClick}>
    Click Me
</button>
```

Inside:

```jsx id="8k2q5m"
function handleClick(event) {

    console.log(event.target);

}
```

`event.target` identifies the element that triggered the event.

In this case:

```text
button
```

---

# 2️⃣9️⃣ Passing Arguments 🎯

Suppose we have:

```jsx id="8k0r6a"
function showUser(name) {
    console.log(name);
}
```

We want to pass:

```text
Ramesh
```

when the button is clicked.

Use:

```jsx id="v6m4wq"
<button onClick={() => showUser("Ramesh")}>
    Click
</button>
```

---

# 3️⃣0️⃣ Why Do We Use `() =>`?

Look at:

```jsx id="n0d6xk"
onClick={() => showUser("Ramesh")}
```

The arrow function waits for the click.

Then after the click:

```text
Click
 ↓
Arrow function
 ↓
showUser("Ramesh")
 ↓
Ramesh
```

---

# 3️⃣1️⃣ Passing Multiple Arguments

```jsx id="q5x7cm"
function showUser(name, age) {

    console.log(name);
    console.log(age);

}
```

Use:

```jsx id="c8r4mv"
<button
    onClick={() => showUser("Ramesh", 25)}
>
    Show User
</button>
```

Flow:

```text
Click
 ↓
showUser("Ramesh", 25)
 ↓
name = Ramesh
age = 25
```

---

# 3️⃣2️⃣ Practical Example — Product

```jsx id="8w2m5k"
function showProduct(name, price) {

    console.log(name);
    console.log(price);

}

function App() {

    return (
        <button
            onClick={() => showProduct("Laptop", 50000)}
        >
            Show Product
        </button>
    );
}

export default App;
```

When clicked:

```text
Laptop
50000
```

appears in the console.

---

# 3️⃣3️⃣ Event + Props Together ⭐

Now let's connect Chapter 6 and Chapter 7.

Parent:

```jsx id="v7n3xc"
function App() {

    function handleMessage() {
        alert("Hello from App");
    }

    return (
        <Button onClick={handleMessage} />
    );
}

export default App;
```

Child:

```jsx id="r8q5mj"
function Button({ onClick }) {

    return (
        <button onClick={onClick}>
            Click Me
        </button>
    );
}

export default Button;
```

Here:

```text
Parent
   |
   | function as prop
   ↓
Child
   |
   | user clicks
   ↓
onClick
   |
   ↓
Parent function runs
```

This is the beginning of **component communication**.

---

# 3️⃣4️⃣ Practical Example — Login Button

```jsx id="w3c7pa"
function LoginButton() {

    function handleLogin() {

        alert("Login button clicked");

    }

    return (
        <button onClick={handleLogin}>
            Login
        </button>
    );
}

export default LoginButton;
```

Flow:

```text
User
 ↓
Click Login
 ↓
onClick
 ↓
handleLogin()
 ↓
Login logic
```

Later, this could become:

```text
Click Login
 ↓
Validate form
 ↓
API request
 ↓
Server
 ↓
Success/Error
```

We will learn API integration later.

---

# 3️⃣5️⃣ Practical Example — Search Input

```jsx id="j8c4xm"
function Search() {

    function handleChange(event) {

        console.log("Search:", event.target.value);

    }

    return (
        <input
            type="text"
            placeholder="Search products"
            onChange={handleChange}
        />
    );
}

export default Search;
```

If the user types:

```text
Laptop
```

the value changes as the user types.

This pattern is used in:

```text
🔎 Search
🔎 Product filtering
🔎 User lookup
🔎 Movie search
🔎 Dashboard filters
```

---

# 3️⃣6️⃣ Practical Example — Form

```jsx id="x4m7qd"
function LoginForm() {

    function handleSubmit(event) {

        event.preventDefault();

        console.log("Login submitted");

    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="email"
                placeholder="Email"
            />

            <input
                type="password"
                placeholder="Password"
            />

            <button type="submit">
                Login
            </button>

        </form>
    );
}

export default LoginForm;
```

Flow:

```text
User enters email
        ↓
User enters password
        ↓
Clicks Login
        ↓
onSubmit
        ↓
handleSubmit()
        ↓
preventDefault()
        ↓
Login logic
```

---

# 3️⃣7️⃣ Event Types — Quick Table

| Event | Used For |
|---|---|
| `onClick` | Button/element click |
| `onChange` | Input value changes |
| `onSubmit` | Form submission |
| `onMouseEnter` | Mouse enters element |
| `onMouseLeave` | Mouse leaves element |
| `onKeyDown` | Key pressed |
| `onKeyUp` | Key released |

These match the event categories in your uploaded course syllabus.

---

# 3️⃣8️⃣ Common Beginner Mistakes ❌

## Mistake 1 — Calling the function immediately

### ❌ Wrong

```jsx id="x2j7vm"
<button onClick={handleClick()}>
```

### ✅ Correct

```jsx id="5w8z3a"
<button onClick={handleClick}>
```

---

# 3️⃣9️⃣ Mistake 2 — Forgetting the event parameter

If you need the input value:

### ❌

```jsx id="g3n7xd"
function handleChange() {
    console.log(event.target.value);
}
```

### ✅

```jsx id="k7v2qs"
function handleChange(event) {
    console.log(event.target.value);
}
```

---

# 4️⃣0️⃣ Mistake 3 — Forgetting `preventDefault()`

For a React form:

```jsx id="m5x2vn"
function handleSubmit(event) {

    event.preventDefault();

}
```

Without preventing the default browser action, the browser may perform its normal form behavior instead of allowing your React code to fully control the interaction.

---

# 4️⃣1️⃣ Mistake 4 — Wrong event name

### ❌

```jsx id="z8m1pr"
<button onclick={handleClick}>
```

### ✅

```jsx id="a7q4wd"
<button onClick={handleClick}>
```

React event names use the expected camelCase form.

Examples:

```text
onClick
onChange
onSubmit
onMouseEnter
onMouseLeave
```

---

# 4️⃣2️⃣ Mistake 5 — Passing arguments incorrectly

### ❌

```jsx id="k2r8sd"
<button onClick={showUser("Ramesh")}>
```

This calls the function during rendering.

### ✅

```jsx id="q6t3wm"
<button onClick={() => showUser("Ramesh")}>
```

The arrow function waits for the click.

---

# 4️⃣3️⃣ Mistake 6 — Using the wrong property

For input values:

### ❌

```jsx id="n7x3cq"
event.target.text
```

### ✅

```jsx id="v5q8sm"
event.target.value
```

For a checkbox, use:

```jsx id="r1k4tw"
event.target.checked
```

---

# ⭐ 4️⃣4️⃣ Best Practices

### ✅ 1. Use meaningful handler names

Good:

```jsx id="k8m2vr"
handleLogin
handleSubmit
handleSearch
handleChange
```

---

### ✅ 2. Keep event handlers focused

For example:

```jsx id="x4q7pn"
function handleSearch(event) {
    // Search-related logic
}
```

Don't put unrelated application logic into one giant function.

---

### ✅ 3. Use the event object when needed

```jsx id="g2w8ms"
function handleChange(event) {
    console.log(event.target.value);
}
```

---

### ✅ 4. Use arrow functions for arguments

```jsx id="n5x7kc"
onClick={() => handleUser("Ramesh")}
```

---

### ✅ 5. Remember event direction

```text
User Action
    ↓
Event
    ↓
Handler
    ↓
Application Logic
    ↓
UI / Data Change
```

---

# 🧠 4️⃣5️⃣ The Most Important Event Flow

Memorize this:

```text
👤 USER
   ↓
🖱️ Click / ⌨️ Type / 📝 Submit
   ↓
⚡ EVENT
   ↓
⚙️ EVENT HANDLER
   ↓
🧠 JAVASCRIPT LOGIC
   ↓
📱 UI RESPONSE
```

Example:

```text
User clicks Add to Cart
        ↓
onClick
        ↓
handleAddToCart()
        ↓
Add product
        ↓
Cart UI updates
```

The last UI update becomes much more powerful once we learn **state** in Chapter 9.

---

# 🎤 4️⃣6️⃣ Interview Questions

## Q1. What is an event in React?

### Answer

An event represents an interaction such as clicking, typing, submitting a form, moving the mouse, or pressing a keyboard key.

---

## Q2. How do you handle a click in React?

### Answer

Use `onClick`.

```jsx id="t4c8mz"
<button onClick={handleClick}>
    Click
</button>
```

---

## Q3. What is an event handler?

### Answer

An event handler is a function that runs in response to an event.

Example:

```jsx id="j7w2xp"
function handleClick() {
    console.log("Clicked");
}
```

---

## Q4. What is the difference between these?

```jsx id="m8q5zc"
onClick={handleClick}
```

and:

```jsx id="p3x7vn"
onClick={handleClick()}
```

### Answer

```text
onClick={handleClick}
    ↓
Pass the function

onClick={handleClick()}
    ↓
Call the function immediately
```

---

## Q5. What is `onChange` used for?

### Answer

It is commonly used to detect changes in form controls such as text inputs.

---

## Q6. How do you get an input's current value?

### Answer

Use:

```jsx id="b4x8qm"
event.target.value
```

---

## Q7. What is `event.preventDefault()`?

### Answer

It prevents the browser's default action for the event.

It is commonly used when handling form submission with React.

---

## Q8. What is `event.target`?

### Answer

It identifies the element that triggered the event.

---

## Q9. How do you pass arguments to an event handler?

### Answer

Use a function wrapper such as an arrow function:

```jsx id="s7k3md"
onClick={() => handleUser("Ramesh")}
```

---

## Q10. What are common keyboard events?

### Answer

Common keyboard events include:

```text
onKeyDown
onKeyUp
```

---

# 🧪 4️⃣7️⃣ Practice

Try these yourself before looking for a solution.

### Practice 1 — Button

Create a button:

```text
[ Click Me ]
```

When clicked, display:

```text
Hello React
```

---

### Practice 2 — Input

Create an input.

When the user types, print the value in the console.

---

### Practice 3 — Form

Create a form with:

```text
Name
Email
[ Submit ]
```

When submitted:

```text
Form submitted
```

should appear in the console.

---

### Practice 4 — Mouse

Create a box.

When the mouse enters:

```text
Mouse Entered
```

When it leaves:

```text
Mouse Left
```

---

### Practice 5 — Keyboard

Create an input.

When the user presses Enter, print:

```text
Enter pressed
```

---

# 🏆 4️⃣8️⃣ Mini Challenge

Build a simple **Product Button**.

Your UI should contain:

```text
Product: Laptop
Price: ₹50000

[ Add to Cart ]
```

When the user clicks:

```text
Add to Cart
```

display:

```text
Laptop added to cart!
```

### Think before coding 🧠

```text
1. What is the user action?
       ↓
   Button click

2. Which event?
       ↓
   onClick

3. Which function?
       ↓
   handleAddToCart

4. What information do we need?
       ↓
   Product name

5. What should happen?
       ↓
   Show message
```

### Hint

You can pass the product name:

```jsx
onClick={() => handleAddToCart("Laptop")}
```

Try it yourself first.

---

# 📝 4️⃣9️⃣ 10-Question Quiz

## Q1. Which event is used for button clicks?

### ✅ Answer

```text
onClick
```

---

## Q2. Which event is commonly used when an input value changes?

### ✅ Answer

```text
onChange
```

---

## Q3. Which event is used for form submission?

### ✅ Answer

```text
onSubmit
```

---

## Q4. Which event occurs when the mouse enters an element?

### ✅ Answer

```text
onMouseEnter
```

---

## Q5. Which event occurs when the mouse leaves?

### ✅ Answer

```text
onMouseLeave
```

---

## Q6. How do you get the value of an input?

### ✅ Answer

```jsx id="n8v3qm"
event.target.value
```

---

## Q7. How do you detect the pressed keyboard key?

### ✅ Answer

```jsx id="p6c2wx"
event.key
```

---

## Q8. Why do we use `event.preventDefault()` in form handling?

### ✅ Answer

To prevent the browser's default form behavior so React can handle the submission.

---

## Q9. Which is correct?

```jsx id="4x7wpm"
onClick={handleClick}
```

or:

```jsx id="8q3nvc"
onClick={handleClick()}
```

### ✅ Answer

Usually:

```jsx id="n3c7xr"
onClick={handleClick}
```

because it passes the function to React.

---

## Q10. How do you pass an argument to an event handler?

### ✅ Answer

Use a wrapper function:

```jsx id="z5m8kq"
onClick={() => handleUser("Ramesh")}
```

---

# 📚 5️⃣0️⃣ Chapter Summary

| Concept | Meaning |
|---|---|
| ⚡ Event | User/application interaction |
| 🖱️ `onClick` | Handles clicks |
| ⌨️ `onChange` | Handles value changes |
| 📝 `onSubmit` | Handles form submission |
| 🖱️ `onMouseEnter` | Mouse enters |
| 🖱️ `onMouseLeave` | Mouse leaves |
| ⌨️ `onKeyDown` | Key pressed |
| ⌨️ `onKeyUp` | Key released |
| 📦 Event Object | Information about the event |
| ⚙️ Event Handler | Function handling an event |
| 🎯 Arguments | Values passed to handler |
| 🛑 `preventDefault()` | Stops default browser action |

---

# 🧠 5️⃣1️⃣ Final Memory Trick

Remember this formula:

```text
USER ACTION
    ↓
EVENT
    ↓
HANDLER
    ↓
LOGIC
    ↓
RESULT
```

Example:

```text
🖱️ Click
   ↓
onClick
   ↓
handleAddToCart()
   ↓
Add product
   ↓
🛒 Cart changes
```

### ⭐ Most important rules

```text
onClick={handleClick}
        ↑
Pass function

onClick={() => handleUser("Ramesh")}
        ↑
Use wrapper when passing arguments

event.target.value
        ↑
Input value

event.key
        ↑
Keyboard key

event.preventDefault()
        ↑
Stop default browser action
```

---

# ✅ Chapter 7 Checklist

Before moving forward, make sure you understand:

- [ ] What is an event?
- [ ] What is an event handler?
- [ ] `onClick`
- [ ] `onChange`
- [ ] `onSubmit`
- [ ] `onMouseEnter`
- [ ] `onMouseLeave`
- [ ] `onKeyDown`
- [ ] `onKeyUp`
- [ ] Event object
- [ ] `event.target`
- [ ] `event.target.value`
- [ ] `event.key`
- [ ] `event.preventDefault()`
- [ ] Passing arguments
- [ ] Common event mistakes
- [ ] Event + props relationship

---

# 🚀 NEXT CHAPTER

## 🔀 CHAPTER 8 — CONDITIONAL RENDERING

We will learn how React decides **what UI to show and when**:

```text
🔀 if / else
      ↓
❓ Ternary Operator
      ↓
&& Logical AND
      ↓
|| Logical OR
      ↓
🔢 Multiple Conditions
      ↓
🧩 Conditional Components
      ↓
⏳ Loading UI
      ↓
❌ Error UI
      ↓
📭 Empty-State UI
```

These are the exact Chapter 8 topics in your uploaded React syllabus.

🛑 **STOP HERE.**

Type **`NEXT`** when you are ready for **Chapter 8 — Conditional Rendering**.