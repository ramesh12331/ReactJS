# 🔗 ReactJS — Chapter 19
# Component Communication

## 🎯 Chapter Goal

In this chapter, you will learn how React components **communicate with each other**.

Your course source lists these Chapter 19 topics:

- 👨‍👦 Parent → Child
- 👶 Child → Parent
- 👫 Sibling communication
- 📦 Props
- 📞 Callback functions
- 🌐 Context
- ⬆️ State lifting
- 🛠️ Practical examples

---

# 1️⃣ What Is Component Communication?

In React, applications are divided into components.

For example:

```text
App
│
├── Header
├── Sidebar
├── ProductList
│     ├── ProductCard
│     ├── ProductCard
│     └── ProductCard
│
└── Footer
```

These components sometimes need to **share information**.

For example:

```text
Parent
   ↓
send user name
   ↓
Child
```

Or:

```text
Child
   ↓
send button click
   ↓
Parent
```

This is called:

# 🔗 Component Communication

---

# 2️⃣ Why Do Components Need Communication?

Imagine an e-commerce application.

You have:

```text
App
│
├── ProductList
│
├── Cart
│
└── Header
```

When a user clicks:

```text
🛒 Add to Cart
```

the application may need to tell another component:

```text
ProductCard
     ↓
Product added
     ↓
Cart
     ↓
Cart count becomes 1
```

Components need a way to share information.

---

# 3️⃣ Main Communication Patterns 🧠

Remember these:

```text
1️⃣ Parent → Child
       ↓
     Props

2️⃣ Child → Parent
       ↓
Callback Function

3️⃣ Sibling → Sibling
       ↓
Lift State Up

4️⃣ Many components
       ↓
Context
```

This is the most important idea in this chapter.

---

# 4️⃣ Parent → Child 👨‍👦

This is the easiest communication pattern.

The parent sends data to the child using:

# 📦 Props

You already learned props in Chapter 6.

---

## Example

### Parent

```jsx
function App() {

    return (
        <User name="Ramesh" />
    );
}

export default App;
```

### Child

```jsx
function User({ name }) {

    return (
        <h1>Hello {name}</h1>
    );
}

export default User;
```

Output:

```text
Hello Ramesh
```

Flow:

```text
App
 │
 │ name="Ramesh"
 ↓
User
 │
 ↓
Hello Ramesh
```

---

# 5️⃣ Parent → Child With Multiple Props

Parent:

```jsx
function App() {

    return (
        <Student
            name="Ramesh"
            age={25}
            city="Hyderabad"
        />
    );
}

export default App;
```

Child:

```jsx
function Student({ name, age, city }) {

    return (
        <div>
            <h2>{name}</h2>
            <p>Age: {age}</p>
            <p>City: {city}</p>
        </div>
    );
}

export default Student;
```

Output:

```text
Ramesh
Age: 25
City: Hyderabad
```

---

# 6️⃣ Remember This Direction ⬇️

```text
👨 Parent
     │
     │ Props
     ↓
👶 Child
```

### ⭐ Important

Props normally flow:

> **Parent → Child**

---

# 7️⃣ Can a Child Directly Change Parent Props?

No.

Props are read-only.

For example:

```jsx
function Child({ name }) {

    name = "Kumar"; // ❌ Don't do this

    return <h1>{name}</h1>;
}
```

The child should not directly modify the prop.

Instead, the parent can provide a function.

This gives us:

# 📞 Child → Parent Communication

---

# 8️⃣ Child → Parent 👶⬆️👨

This is one of the most important React concepts.

React does not normally send data directly upward through props.

Instead:

```text
Parent
   ↓
passes function
   ↓
Child
   ↓
calls function
   ↓
Parent receives data
```

This is called using a:

# 📞 Callback Function

---

# 9️⃣ Simple Callback Example

Parent:

```jsx
function App() {

    function receiveMessage(message) {

        console.log(message);

    }

    return (
        <Child onSend={receiveMessage} />
    );
}

export default App;
```

Child:

```jsx
function Child({ onSend }) {

    function handleClick() {

        onSend("Hello Parent");

    }

    return (
        <button onClick={handleClick}>
            Send Message
        </button>
    );
}

export default Child;
```

When the child button is clicked:

```text
Child
  ↓
onSend("Hello Parent")
  ↓
Parent
  ↓
receiveMessage()
  ↓
"Hello Parent"
```

---

# 🔟 Why Is It Called a Callback?

The parent gives the child a function:

```jsx
<Child onSend={receiveMessage} />
```

The child later calls that function:

```jsx
onSend("Hello Parent");
```

So:

```text
Parent gives function
       ↓
Child stores/receives function
       ↓
Child calls function
       ↓
Parent function runs
```

That function is a callback.

---

# 1️⃣1️⃣ Real-World Analogy 📞

Imagine your manager gives you a phone number:

```text
Manager
   ↓
"Call me when the work is finished"
   ↓
You
```

You don't directly change the manager's data.

Instead, you call the manager when something happens.

React callback functions work similarly:

```text
Parent
   ↓
"Call this function when something happens"
   ↓
Child
   ↓
event happens
   ↓
callback()
   ↓
Parent
```

---

# 1️⃣2️⃣ Child Sends Data to Parent

This is extremely useful.

### Parent

```jsx
function App() {

    function handleUser(name) {

        console.log("User:", name);

    }

    return (
        <Child onUserChange={handleUser} />
    );
}
```

### Child

```jsx
function Child({ onUserChange }) {

    return (
        <button onClick={() => onUserChange("Ramesh")}>
            Send User
        </button>
    );
}
```

When clicked:

```text
Child
   ↓
"Ramesh"
   ↓
onUserChange()
   ↓
Parent
   ↓
handleUser("Ramesh")
```

---

# 1️⃣3️⃣ Child → Parent With a Value

The child can send:

- string
- number
- object
- array
- event information
- form data

Example:

```jsx
onUserChange({
    name: "Ramesh",
    age: 25,
    city: "Hyderabad"
});
```

The parent receives the object.

---

# 1️⃣4️⃣ Practical Example — Login Form 🔐

Imagine:

```text
Login
│
├── Email
├── Password
└── Login Button
```

The child form can send login information to the parent.

### Parent

```jsx
function App() {

    function handleLogin(data) {

        console.log(data);

    }

    return (
        <LoginForm onLogin={handleLogin} />
    );
}
```

### Child

```jsx
function LoginForm({ onLogin }) {

    function handleSubmit() {

        const user = {
            email: "ramesh@example.com",
            password: "123456"
        };

        onLogin(user);
    }

    return (
        <button onClick={handleSubmit}>
            Login
        </button>
    );
}
```

Flow:

```text
LoginForm
    ↓
user data
    ↓
onLogin(user)
    ↓
App
    ↓
handleLogin(data)
```

Later, you will use this pattern in real forms and authentication.

---

# 1️⃣5️⃣ Sibling Communication 👫

Now suppose:

```text
Parent
│
├── Child A
│
└── Child B
```

Child A wants to send information to Child B.

Can Child A directly pass props to Child B?

❌ No.

They are siblings.

The common solution is:

# ⬆️ Lift State Up

---

# 1️⃣6️⃣ What Is State Lifting?

Suppose:

```text
        Parent
       /      \
      ↓        ↓
 Child A    Child B
```

Instead of:

```text
Child A → Child B
```

we move shared data to:

```text
        Parent
       /      \
      ↓        ↓
 Child A    Child B
```

The parent becomes the common place for the shared state.

---

# 1️⃣7️⃣ Why Do We Lift State?

Suppose two components need the same data.

```text
Component A
     ↓
needs user data

Component B
     ↓
needs user data
```

Instead of keeping two separate copies:

```text
A → user data
B → user data
```

we can keep it in their common parent:

```text
        Parent
          │
      user data
       /      \
      ↓        ↓
     A         B
```

This keeps the data synchronized.

---

# 1️⃣8️⃣ Simple Sibling Example

Imagine:

```text
Parent
│
├── Input
│
└── Display
```

The user enters a name in `Input`.

`Display` should show:

```text
Hello Ramesh
```

We can keep the shared value in the parent.

---

# 1️⃣9️⃣ State Lifting Flow

```text
Input
  │
  │ user enters data
  ↓
Parent State
  │
  │ props
  ├────────────→ Display
  │
  ↓
updated UI
```

The parent is the middle point.

---

# 2️⃣0️⃣ Example Architecture

```text
                 App
                  │
            shared state
             /         \
            ↓           ↓
        InputBox     Display
            │
            │ callback
            ↓
           App
```

The flow is:

```text
InputBox
   ↓
callback
   ↓
App state
   ↓
props
   ↓
Display
```

This is one of the most important React patterns.

---

# 2️⃣1️⃣ Practical Example — Search Box 🔎

Imagine:

```text
App
│
├── SearchBox
│
└── ProductList
```

User types:

```text
Laptop
```

We want `ProductList` to know:

```text
search = "Laptop"
```

We can keep the search value in `App`.

Flow:

```text
SearchBox
    ↓
callback
    ↓
App state
    ↓
search value
    ↓
ProductList
```

This is state lifting.

---

# 2️⃣2️⃣ Another Example — Cart 🛒

Imagine:

```text
App
│
├── ProductList
│
└── Cart
```

When the user clicks:

```text
Add to Cart
```

the product needs to reach the cart.

A common architecture is:

```text
ProductList
      ↓
callback
      ↓
App
      ↓
cart state
      ↓
Cart
```

Later, you will build this kind of logic in real projects.

---

# 2️⃣3️⃣ Context 🌐

Now imagine your component tree becomes very large.

```text
App
│
└── Layout
     │
     └── Dashboard
          │
          └── Sidebar
               │
               └── UserProfile
```

Suppose `UserProfile` needs user information from `App`.

Without another approach, you might pass props through components that don't actually need them.

This is called:

# 📦 Prop Drilling

---

# 2️⃣4️⃣ What Is Prop Drilling?

Imagine:

```text
App
 ↓ user
Layout
 ↓ user
Dashboard
 ↓ user
Sidebar
 ↓ user
UserProfile
```

Only `UserProfile` actually needs:

```text
user
```

But several components must pass it along.

This can become difficult to maintain.

---

# 2️⃣5️⃣ Prop Drilling Diagram

```text
App
 │
 │ user
 ↓
Layout
 │
 │ user
 ↓
Dashboard
 │
 │ user
 ↓
Sidebar
 │
 │ user
 ↓
UserProfile
```

The data is traveling through components that don't need it.

---

# 2️⃣6️⃣ Context Can Help

React provides:

# 🌐 Context

Context allows values to be made available to components deeper in the component tree without manually passing props through every level.

Simple idea:

```text
Context
   ↓
Provider
   ↓
many components
   ↓
useContext()
```

You already have a separate Chapter 14 dedicated to `useContext`, so here we only need to understand its role in communication. The course source lists Context, prop drilling, Provider, Consumer, `useContext`, theme, authentication, and language settings under Chapter 14.

---

# 2️⃣7️⃣ Context Example

Conceptually:

```text
App
 │
 │ provides user
 ↓
Context
 │
 ├── Header
 ├── Sidebar
 └── Profile
```

The components can access the shared value through Context.

---

# 2️⃣8️⃣ When Should We Use Props? 📦

Use props when:

```text
Parent
   ↓
Child
```

and the communication is simple.

Example:

```jsx
<User name="Ramesh" />
```

Props are usually the first solution you should think about.

---

# 2️⃣9️⃣ When Should We Use Callback Functions? 📞

Use callbacks when:

```text
Child
   ↓
needs to notify parent
```

Example:

```jsx
<Child onSave={handleSave} />
```

Child:

```jsx
onSave(data);
```

---

# 3️⃣0️⃣ When Should We Lift State? ⬆️

Lift state when:

```text
Two or more components
        ↓
need the same data
```

Move the state to their nearest common parent.

Example:

```text
       Parent
       /    \
      A      B
```

If A and B need the same state:

```text
Parent
  ↓
shared state
```

---

# 3️⃣1️⃣ When Should We Consider Context? 🌐

Consider Context when:

```text
Many components
      ↓
need the same data
```

Examples:

- 🌙 Theme
- 👤 Current user
- 🔐 Authentication information
- 🌍 Language
- ⚙️ Application-level settings

Don't automatically use Context for every piece of state.

---

# 3️⃣2️⃣ Communication Cheat Sheet 🧠

| Situation | Common approach |
|---|---|
| Parent → Child | Props |
| Child → Parent | Callback function |
| Sibling → Sibling | Lift state to parent |
| Many deep components | Context may help |
| Shared application state | Context / state management tools |

---

# 3️⃣3️⃣ Important Direction Diagram

Memorize this:

```text
             👨 Parent
             /      \
            /        \
       Props ↓        ↓ Props
          👶 A        👶 B
            │
            │ callback
            ↑
          Parent
```

And:

```text
Sibling A
    ↓
Callback
    ↓
Parent State
    ↓
Props
    ↓
Sibling B
```

---

# 3️⃣4️⃣ Practical Example — Student App 🎓

Let's design:

```text
App
│
├── StudentForm
│
└── StudentList
```

The user enters:

```text
Name: Ramesh
Course: React
```

The form needs to send data to `App`.

```text
StudentForm
     ↓
onAdd(student)
     ↓
App
     ↓
student state
     ↓
StudentList
```

This combines:

- props
- callback functions
- state lifting

---

# 3️⃣5️⃣ Practical Code Structure

### Parent

```jsx
function App() {

    function handleAdd(student) {

        console.log(student);

    }

    return (
        <div>

            <StudentForm onAdd={handleAdd} />

            <StudentList />

        </div>
    );
}

export default App;
```

### Child

```jsx
function StudentForm({ onAdd }) {

    function handleClick() {

        const student = {
            name: "Ramesh",
            course: "React"
        };

        onAdd(student);
    }

    return (
        <button onClick={handleClick}>
            Add Student
        </button>
    );
}

export default StudentForm;
```

Flow:

```text
StudentForm
     ↓
student object
     ↓
onAdd(student)
     ↓
App
     ↓
handleAdd(student)
```

---

# 3️⃣6️⃣ Dry Run 🧠

Suppose the user clicks:

```text
[Add Student]
```

### Step 1

Child runs:

```jsx
handleClick()
```

### Step 2

Child creates:

```jsx
const student = {
    name: "Ramesh",
    course: "React"
};
```

### Step 3

Child calls:

```jsx
onAdd(student);
```

### Step 4

The parent's function runs:

```jsx
handleAdd(student)
```

### Step 5

Parent receives:

```text
{
    name: "Ramesh",
    course: "React"
}
```

Flow:

```text
Click
 ↓
Child function
 ↓
callback
 ↓
Parent function
 ↓
Parent receives data
```

---

# 3️⃣7️⃣ Common Mistake ❌

Wrong:

```jsx
<Child onSend={handleSend()} />
```

This calls the function immediately.

Correct:

```jsx
<Child onSend={handleSend} />
```

Now the child can call it later.

---

# 3️⃣8️⃣ Common Mistake — Wrong Callback

Wrong:

```jsx
function Child({ onSend }) {

    return (
        <button onClick={onSend("Hello")}>
            Send
        </button>
    );
}
```

Why?

Because:

```jsx
onSend("Hello")
```

runs during rendering.

Correct:

```jsx
function Child({ onSend }) {

    return (
        <button onClick={() => onSend("Hello")}>
            Send
        </button>
    );
}
```

---

# 3️⃣9️⃣ Common Mistake — Child Trying to Modify Props

Wrong:

```jsx
function Child({ name }) {

    name = "Kumar";

    return <h1>{name}</h1>;
}
```

Don't modify props.

Instead, ask the parent to update its own data through a callback.

---

# 4️⃣0️⃣ Common Mistake — Too Much Prop Drilling

If you have:

```text
App
 ↓
A
 ↓
B
 ↓
C
 ↓
D
 ↓
E
```

and you're passing the same data through every component:

```text
user
user
user
user
```

you may have a communication design problem.

Depending on the application, Context or another state-management approach may be more suitable.

---

# 4️⃣1️⃣ Best Practices ⭐

### ✅ Start with props

For simple parent → child communication:

```text
Props
```

is usually enough.

### ✅ Use callbacks for child → parent

```text
Child
 ↓
callback
 ↓
Parent
```

### ✅ Lift shared state

If siblings need the same state:

```text
Sibling A
   ↘
    Parent
   ↗
Sibling B
```

### ✅ Don't use Context unnecessarily

Context is useful, but don't make every small value global.

### ✅ Keep data flow understandable

A good React application should make it reasonably clear:

```text
Where does the data come from?
Where does it change?
Who needs it?
```

---

# 4️⃣2️⃣ Real-World Communication Architecture

A typical application can look like:

```text
                    App
                     │
              Shared State
                     │
        ┌────────────┴────────────┐
        ↓                         ↓
    Header                    Dashboard
                                  │
                         ┌────────┴────────┐
                         ↓                 ↓
                    ProductList         Cart
                         │
                         ↓
                    ProductCard
```

Communication can use:

```text
Props
Callbacks
Lifted State
Context
```

depending on the situation.

---

# 4️⃣3️⃣ Interview Questions 🎤

## Q1. How does a parent pass data to a child?

Using props.

```jsx
<Child name="Ramesh" />
```

---

## Q2. How can a child communicate with a parent?

The parent passes a callback function to the child.

The child calls that function.

---

## Q3. What is a callback function in React?

A function passed from one component to another, commonly from parent to child, so the child can call it when something happens.

---

## Q4. Can props be directly modified by a child?

No.

Props should be treated as read-only.

---

## Q5. How do sibling components communicate?

A common approach is to lift the shared state to their common parent and pass data/functions through props.

---

## Q6. What is state lifting?

Moving shared state to the nearest common parent so multiple child components can use the same source of truth.

---

## Q7. What is prop drilling?

Passing props through several intermediate components even though those components don't actually need the data.

---

## Q8. How can Context help?

Context can make shared values available to components deeper in the tree without manually passing props through every intermediate component.

---

## Q9. When should you use Context?

When multiple components, especially deeply nested components, need the same shared value.

---

## Q10. What is the simplest Parent → Child communication method?

Props.

---

# 🧪 4️⃣4️⃣ Practice Questions

### Practice 1

Create:

```text
App
 ↓
User
```

Send:

```text
name
age
city
```

using props.

---

### Practice 2

Create:

```text
App
 ↓
Button
```

Parent should provide:

```text
handleMessage()
```

Child should call it when the button is clicked.

---

### Practice 3

Make the child send:

```text
"Hello Parent"
```

to the parent.

---

### Practice 4

Create:

```text
App
├── SearchBox
└── ProductList
```

Think about where the search value should live.

---

### Practice 5

Create:

```text
App
├── Product
└── Cart
```

Design the communication flow for:

```text
[Add to Cart]
```

Don't code immediately.

First draw the flow.

---

# 🏆 4️⃣5️⃣ Mini Challenge

## 🛒 Shopping Cart Communication

Build this structure:

```text
App
│
├── ProductList
│     │
│     ├── ProductCard
│     ├── ProductCard
│     └── ProductCard
│
└── Cart
```

Product:

```text
Laptop
₹50,000

[Add to Cart]
```

### Your task

When the user clicks:

```text
Add to Cart
```

the product should communicate with the parent.

Think:

```text
ProductCard
      ↓
callback
      ↓
App
      ↓
cart data
      ↓
Cart
```

### 🤔 Before coding, answer:

1. Who owns the shared cart data?
2. Who sends the product?
3. What callback should the parent provide?
4. Which component receives the callback?
5. How does the Cart component receive the updated data?

This exercise is very important because it prepares you for your future CRUD and e-commerce projects.

---

# 📝 4️⃣6️⃣ 10-Question Quiz

## Q1. Parent → Child communication uses what?

### ✅ Answer

Props.

---

## Q2. Child → Parent communication commonly uses what?

### ✅ Answer

A callback function passed from the parent.

---

## Q3. Can a child directly modify its props?

### ❌ Answer

No.

Props should be treated as read-only.

---

## Q4. What is state lifting?

### ✅ Answer

Moving shared state to a common parent so multiple components can use the same source of truth.

---

## Q5. How do sibling components commonly share data?

### ✅ Answer

Lift the shared state to their common parent and communicate through props/callbacks.

---

## Q6. What is prop drilling?

### ✅ Answer

Passing props through multiple intermediate components that don't actually need the data.

---

## Q7. What can help reduce prop drilling?

### ✅ Answer

Context can help when many or deeply nested components need shared data.

---

## Q8. Which direction do props normally flow?

### ✅ Answer

```text
Parent → Child
```

---

## Q9. What does the child do to send data through a callback?

### ✅ Answer

It calls the callback provided by the parent.

Example:

```jsx
onSend(data);
```

---

## Q10. What should you use first for simple Parent → Child communication?

### ✅ Answer

Props.

---

# 📚 4️⃣7️⃣ Quick Revision

| Concept | Simple Meaning |
|---|---|
| 👨‍👦 Parent → Child | Props |
| 👶⬆️ Parent | Callback function |
| 👫 Siblings | Lift state |
| 📦 Props | Data passed to child |
| 📞 Callback | Function passed for child to call |
| ⬆️ State lifting | Move shared state to parent |
| 🌐 Context | Share values through component tree |
| 🚧 Prop drilling | Passing props through unnecessary levels |
| 🛒 Cart example | Parent can own shared cart state |

---

# 🧠 4️⃣8️⃣ Final Memory Trick

Remember this one diagram:

```text
                 👨 Parent
                 │
          ┌──────┴──────┐
          │             │
       Props ↓       Props ↓
          │             │
          👶 A           👶 B
          │
          │ Callback
          │
          └──────→ 👨 Parent
                    │
                    ↓
               Shared State
                    │
              ┌─────┴─────┐
              ↓           ↓
             A             B
```

### ⭐ The four rules

```text
📦 Parent → Child
   = Props

📞 Child → Parent
   = Callback

⬆️ Sibling → Sibling
   = Lift State

🌐 Many/deep components
   = Context may help
```

---

# ✅ Chapter 19 Checklist

Before moving to the next chapter, make sure you can explain:

- [ ] What is component communication?
- [ ] Parent → Child
- [ ] Props
- [ ] Child → Parent
- [ ] Callback functions
- [ ] Passing data through callbacks
- [ ] Sibling communication
- [ ] State lifting
- [ ] Prop drilling
- [ ] Context
- [ ] When to use props
- [ ] When to use callbacks
- [ ] When to lift state
- [ ] When Context may help
- [ ] Common callback mistakes
- [ ] Shopping cart communication flow

---

# 🚀 NEXT CHAPTER

## 🗃️ CHAPTER 20 — State Management

Next we will learn:

```text
📦 Local State
      ↓
⬆️ Lifted State
      ↓
🌐 Global State
      ↓
Context API
      ↓
Redux
      ↓
Zustand Overview
      ↓
🤔 Choosing the Right Approach
```

These are the Chapter 20 topics in your course source.

Type **`NEXT`** when you are ready for **Chapter 20 — State Management**.