# 🌐 Chapter 14 — `useContext`

## 🎯 Chapter Goal

In this chapter, you will learn:

- What is Context?
- Why do we need Context?
- What is Prop Drilling?
- How Context solves Prop Drilling
- `createContext()`
- `Provider`
- `useContext()`
- Sharing data between components
- Theme example
- User/Login example
- Common mistakes
- Best practices
- Interview questions
- Practice
- Mini challenge

---

# 1️⃣ What Is Context?

In React, sometimes many components need the same data.

For example:

```text id="4m0qk5"
App
│
├── Header
│
├── Sidebar
│
└── Dashboard
```

Suppose all these components need:

```text
👤 User information
🎨 Theme
🌍 Language
🔐 Login status
```

One way is to pass the data using props.

But if the data has to travel through many components, the code becomes difficult.

This problem is called:

# 🚧 Prop Drilling

React Context provides another way to share data with components without passing the data manually through every intermediate component.

---

# 2️⃣ Real-World Analogy 🏢

Imagine an office building.

There is one common notice board:

```text
🏢 Office

       📋
   Common Notice
       │
 ┌─────┼─────┐
 ↓     ↓     ↓
👨‍💻   👩‍💻   👨‍💻
```

Everyone who needs the information can read it.

Instead of:

```text
Person A
   ↓
Person B
   ↓
Person C
   ↓
Person D
```

we have:

```text
       Common Data
       /    |    \
      ↓     ↓     ↓
     A      B      C
```

Context works in a similar way.

---

# 3️⃣ What Is Prop Drilling?

Suppose:

```text id="v6g1de"
App
 ↓
Header
 ↓
Navbar
 ↓
UserProfile
```

We want to send:

```text
userName = "Ramesh"
```

from `App` to `UserProfile`.

Without Context:

```text id="p7t0n2"
App
 ↓ props
Header
 ↓ props
Navbar
 ↓ props
UserProfile
```

Even if `Header` and `Navbar` don't need `userName`, they still have to receive and pass it.

This is **prop drilling**.

---

# 4️⃣ Prop Drilling Example

```jsx id="6p2mqa"
function App() {

    const userName = "Ramesh";

    return (
        <Header userName={userName} />
    );
}
```

Then:

```jsx id="l4f6hx"
function Header({ userName }) {

    return (
        <Navbar userName={userName} />
    );
}
```

Then:

```jsx id="k5e7qf"
function Navbar({ userName }) {

    return (
        <UserProfile userName={userName} />
    );
}
```

Finally:

```jsx id="6d2s9h"
function UserProfile({ userName }) {

    return <h1>Hello {userName}</h1>;
}
```

Notice:

```text id="zv1y9e"
App
 ↓
Header
 ↓
Navbar
 ↓
UserProfile
```

The data is travelling through components that don't actually need it.

---

# 5️⃣ Why Is Prop Drilling a Problem?

For a small application, it may be okay.

But imagine:

```text id="k7y4p1"
App
 ↓
Layout
 ↓
Header
 ↓
Navbar
 ↓
Menu
 ↓
UserMenu
 ↓
Profile
```

Now you may have to pass:

```text
user
theme
language
permissions
```

through many components.

This can make the code:

```text id="x5q4x2"
❌ Harder to maintain
❌ More props
❌ More repetitive
❌ More difficult to understand
```

Context can help with this type of shared data.

---

# 6️⃣ What Is React Context?

Context allows data to be made available to components in a part of the component tree without passing props through every intermediate component.

Basic flow:

```text id="n8s7r4"
        Context
           │
     ┌─────┼─────┐
     ↓     ↓     ↓
   Header  Main  Footer
```

Components can read the shared context when they need it.

---

# 7️⃣ Three Important Things ⭐⭐⭐

When learning Context, remember these three terms:

```text id="5v3kq1"
1️⃣ createContext()
2️⃣ Provider
3️⃣ useContext()
```

Think:

```text id="h6r9s1"
createContext()
      ↓
create the shared context

Provider
      ↓
provide the data

useContext()
      ↓
read the data
```

---

# 8️⃣ Step 1 — `createContext()`

First import:

```jsx id="p4y6k8"
import { createContext } from "react";
```

Then:

```jsx id="v3q1w8"
const UserContext = createContext();
```

Now we have a context:

```text id="d9r5q2"
UserContext
     ↓
Shared data channel
```

---

# 9️⃣ Better Default Value

You can provide a default value:

```jsx id="z5m3s7"
const UserContext = createContext(null);
```

Or:

```jsx id="q2p8v4"
const UserContext = createContext({
    name: "Guest"
});
```

For beginners, this is enough to remember:

```jsx id="j7x1c9"
const UserContext = createContext(null);
```

---

# 🔟 Step 2 — Provider

Now we need to provide data.

Conceptually:

```text id="f6k3r1"
UserContext
     ↓
  Provider
     ↓
  user data
```

Modern React versions also support provider syntax such as:

```jsx id="q2k7x9"
<UserContext value={user}>
    ...
</UserContext>
```

You may also see the traditional form:

```jsx id="x5n8m2"
<UserContext.Provider value={user}>
    ...
</UserContext.Provider>
```

The exact syntax can depend on the React version/template you are using. The important concept is:

> The provider makes the context value available to components below it.

---

# 1️⃣1️⃣ Simple Context Example

Let's create:

```text id="w4s8p2"
UserContext.jsx
App.jsx
UserProfile.jsx
```

### `UserContext.jsx`

```jsx id="n3q7v1"
import { createContext } from "react";

const UserContext = createContext(null);

export default UserContext;
```

---

# 1️⃣2️⃣ Providing the Data

In `App.jsx`:

```jsx id="k8m2r6"
import UserContext from "./UserContext";
import UserProfile from "./UserProfile";

function App() {

    const user = {
        name: "Ramesh",
        city: "Hyderabad"
    };

    return (
        <UserContext value={user}>
            <UserProfile />
        </UserContext>
    );
}

export default App;
```

Here:

```jsx id="v7n1s4"
<UserContext value={user}>
```

makes `user` available to components inside it.

If your project uses the traditional provider syntax, the equivalent is:

```jsx id="f3q9w6"
<UserContext.Provider value={user}>
    <UserProfile />
</UserContext.Provider>
```

---

# 1️⃣3️⃣ Step 3 — `useContext()`

Now the child component can read the context.

```jsx id="c6k2r8"
import { useContext } from "react";
import UserContext from "./UserContext";

function UserProfile() {

    const user = useContext(UserContext);

    return (
        <div>
            <h1>{user.name}</h1>
            <p>{user.city}</p>
        </div>
    );
}

export default UserProfile;
```

The important line:

```jsx id="q8m5t2"
const user = useContext(UserContext);
```

means:

> Give me the current value from `UserContext`.

---

# 1️⃣4️⃣ Complete Flow

```text id="z6q3m8"
             App
              │
              │ value={user}
              ↓
        UserContext
              │
              ↓
        UserProfile
              │
              │ useContext()
              ↓
          user data
```

The data does not need to be passed like:

```text id="j4m8s2"
App
 ↓
Header
 ↓
Navbar
 ↓
Profile
```

---

# 1️⃣5️⃣ Dry Run 🧠

Suppose:

```jsx id="s2n6q9"
const user = {
    name: "Ramesh",
    city: "Hyderabad"
};
```

We provide:

```jsx id="m7x4p1"
<UserContext value={user}>
```

Now `UserProfile` does:

```jsx id="k5v9r3"
const user = useContext(UserContext);
```

React gives:

```text id="y2s8c6"
user
 ↓
{
    name: "Ramesh",
    city: "Hyderabad"
}
```

Then:

```jsx id="g8q1m5"
user.name
```

gives:

```text id="f4k7p2"
Ramesh
```

and:

```jsx id="c9m3s8"
user.city
```

gives:

```text id="h6q2w4"
Hyderabad
```

---

# 1️⃣6️⃣ Context Flow Diagram

```text id="a8m3q7"
                Context
                   │
                   │ value
                   ↓
              Provider
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
      Header      Main      Footer
        │
        ↓
   UserProfile
        │
        ↓
  useContext()
        │
        ↓
   Shared data
```

---

# 1️⃣7️⃣ Context Does NOT Mean "Global Everything"

This is important.

Don't create Context for every variable.

For example:

```text id="r7k2p5"
❌ simple local input value
❌ simple button state
❌ small component state
```

For local state, use:

```jsx id="d6m1s8"
useState()
```

Context is useful when data needs to be shared across many components.

---

# 1️⃣8️⃣ Common Context Examples 🌍

Real applications may use Context for:

### 🎨 Theme

```text id="q3k7m9"
light
dark
```

### 👤 User

```text id="w8p2n5"
name
email
role
```

### 🔐 Authentication

```text id="s4m9q1"
loggedIn
user
token information
```

### 🌍 Language

```text id="v6k2r8"
English
Telugu
Hindi
```

### 🛒 Shopping Cart

```text id="n7p3x5"
cart items
cart count
```

---

# 1️⃣9️⃣ Practical Example — Theme

Let's create a theme context.

### `ThemeContext.jsx`

```jsx id="y4m8q2"
import { createContext } from "react";

const ThemeContext = createContext("light");

export default ThemeContext;
```

---

### `App.jsx`

```jsx id="r8p3n6"
import ThemeContext from "./ThemeContext";
import Home from "./Home";

function App() {

    return (
        <ThemeContext value="dark">
            <Home />
        </ThemeContext>
    );
}

export default App;
```

---

### `Home.jsx`

```jsx id="m6q2v9"
import { useContext } from "react";
import ThemeContext from "./ThemeContext";

function Home() {

    const theme = useContext(ThemeContext);

    return (
        <div>
            <h1>Current Theme: {theme}</h1>
        </div>
    );
}

export default Home;
```

Output:

```text id="w3n7k5"
Current Theme: dark
```

---

# 2️⃣0️⃣ Why Is This Better?

Without Context:

```text id="k8r2m4"
App
 ↓ theme
Layout
 ↓ theme
Navbar
 ↓ theme
Home
```

With Context:

```text id="p6q9s3"
       ThemeContext
            ↓
          Home
            ↓
       useContext()
```

Much less prop passing.

---

# 2️⃣1️⃣ Context + `useState` ⭐⭐⭐

This is a very important pattern.

Context can share state.

Example:

```jsx id="x4m7q2"
import { createContext, useState } from "react";

const ThemeContext = createContext(null);

function App() {

    const [theme, setTheme] = useState("light");

    return (
        <ThemeContext value={{ theme, setTheme }}>
            <Home />
        </ThemeContext>
    );
}

export default App;
```

Now components can receive:

```text id="v8n3p6"
theme
setTheme
```

This means Context can share both:

```text id="m2q7r9"
📦 Data
+
⚙️ Functions
```

---

# 2️⃣2️⃣ Reading Context

A child component:

```jsx id="q6m8s1"
import { useContext } from "react";

const { theme, setTheme } = useContext(ThemeContext);
```

Now:

```jsx id="n4p7x2"
theme
```

contains the current theme.

And:

```jsx id="r9m3k5"
setTheme
```

can change it.

---

# 2️⃣3️⃣ Context + State Flow

```text id="f7q2m8"
              App
               │
        useState("light")
               │
               ↓
       ┌────────────────┐
       │ Theme Context  │
       └────────────────┘
               │
        ┌──────┼──────┐
        ↓      ↓      ↓
      Header  Home  Settings
        │
        ↓
   useContext()
```

This pattern is extremely common.

---

# 2️⃣4️⃣ Context vs Props

| Props | Context |
|---|---|
| Parent → child | Shared through a context |
| Explicit | More implicit |
| Good for component-specific data | Good for shared data |
| Easy to understand locally | Useful across deeper trees |
| Can cause prop drilling | Helps reduce prop drilling |

---

# 2️⃣5️⃣ When Should You Use Props?

Suppose:

```text id="c4m8q7"
App
 ↓
UserCard
```

Only one child needs the data.

Use props:

```jsx id="v7n2p5"
<UserCard name="Ramesh" />
```

Simple.

Don't create Context unnecessarily.

---

# 2️⃣6️⃣ When Should You Use Context?

Suppose:

```text id="x9m3q6"
App
 ↓
Layout
 ↓
Header
 ↓
Navbar
 ↓
UserMenu
 ↓
Profile
```

Many distant components need:

```text id="s7k2n4"
user
theme
language
```

Context can be useful.

---

# 2️⃣7️⃣ Context Is Not a Replacement for Every State Management Tool

As your applications become larger, you may learn:

```text id="p8m4q2"
Context
Redux
Redux Toolkit
Zustand
Other state-management solutions
```

Context is one tool.

It does not automatically solve every state-management problem.

You will learn Redux later in your React course.

---

# 2️⃣8️⃣ Common Mistake — Forgetting Provider

Suppose:

```jsx id="b7m2q9"
const theme = useContext(ThemeContext);
```

but the component isn't inside the appropriate context provider.

Then the component receives the context's default value.

For example:

```jsx id="n4k8s3"
const ThemeContext = createContext("light");
```

If no provider is supplying another value:

```text id="m7q2p5"
useContext(ThemeContext)
        ↓
"light"
```

So always understand where your provider is placed.

---

# 2️⃣9️⃣ Common Mistake — Wrong Context

Suppose you create:

```jsx id="x8q3m6"
const UserContext = createContext(null);
```

But accidentally consume another context:

```jsx id="p4n7s2"
useContext(ThemeContext);
```

You won't receive the expected user data.

Make sure:

```text id="k6m2r9"
Provider context
       ↓
same context
       ↓
useContext()
```

---

# 3️⃣0️⃣ Common Mistake — Too Much Context

Don't create:

```text id="v9q3m7"
NameContext
AgeContext
CityContext
EmailContext
PhoneContext
ColorContext
```

for every tiny value.

This can make your application harder to understand.

Group logically related shared values.

For example:

```jsx id="c6m8p2"
<UserContext value={{ user, setUser }}>
```

---

# 3️⃣1️⃣ Common Mistake — Putting Everything in Context

Bad thinking:

> "Context is available, so I'll put all application data there."

Not recommended.

Ask:

```text id="w3q7n5"
❓ Does many components need this data?

❓ Is it truly shared?

❓ Can props solve it simply?

❓ Is local state enough?
```

Choose the simplest solution.

---

# ⭐ 3️⃣2️⃣ Best Practices

### ✅ Keep Context focused

For example:

```text id="h7m2q4"
AuthContext
ThemeContext
CartContext
```

rather than one giant context for everything.

---

### ✅ Use state locally when possible

```jsx id="s8q3m6"
const [isOpen, setIsOpen] = useState(false);
```

If only one component needs it, don't create Context.

---

### ✅ Use meaningful names

Good:

```jsx id="n5m8q2"
UserContext
AuthContext
ThemeContext
CartContext
```

Avoid:

```jsx id="r3p7x9"
DataContext
Context1
MyContext2
```

---

### ✅ Keep provider placement sensible

Put the provider high enough for all required consumers, but don't automatically wrap the entire application if only a small section needs the context.

---

# 3️⃣3️⃣ Real-World Authentication Example 🔐

Imagine your application has:

```text id="v4q8m2"
App
│
├── Navbar
├── Dashboard
├── Orders
└── Profile
```

All these components may need:

```text id="x7n3p6"
currentUser
isLoggedIn
logout()
```

You could create:

```text id="s9m4q2"
AuthContext
```

Then:

```text id="h6p2r8"
              AuthContext
                   │
        ┌──────────┼──────────┐
        ↓          ↓          ↓
     Navbar    Dashboard   Profile
        │          │          │
        └──── useContext ─────┘
```

This is a realistic use of Context.

---

# 3️⃣4️⃣ Component Tree Example

```text id="a8q4m6"
App
│
├── Navbar
│   └── UserMenu
│       └── Profile
│
├── Dashboard
│   ├── Statistics
│   └── Orders
│
└── Footer
```

Suppose `Profile`, `Navbar`, and `Dashboard` all need the logged-in user.

Without Context:

```text id="p6m2q8"
App
 ↓
Navbar
 ↓
UserMenu
 ↓
Profile
```

and many props may need to be passed.

With Context:

```text id="w4n8s3"
             AuthContext
                  │
        ┌─────────┼─────────┐
        ↓         ↓         ↓
     Navbar   Dashboard   Profile
```

Much easier to share the common information.

---

# 🎤 3️⃣5️⃣ Interview Questions

## Q1. What is React Context?

Context provides a way to share data with components without passing props manually through every intermediate component.

---

## Q2. What problem does Context solve?

It can help reduce prop drilling.

---

## Q3. What is prop drilling?

Passing props through components that don't need the data themselves just to reach a deeper child.

---

## Q4. Which function creates a context?

```jsx id="z8m3q7"
createContext()
```

---

## Q5. Which Hook reads context?

```jsx id="r4p7n2"
useContext()
```

---

## Q6. What does a Provider do?

It supplies a context value to components below it in the component tree.

---

## Q7. Can Context share functions?

Yes.

For example:

```jsx id="k5m9s2"
{
    user,
    logout
}
```

can be provided through Context.

---

## Q8. Should every state value go into Context?

No.

Use local state when the data is only needed locally.

---

## Q9. Give examples of data suitable for Context.

Examples:

```text id="m7q2p8"
Theme
Authentication/user
Language
Shopping cart
```

---

## Q10. Is Context the same as Redux?

No.

Context is a React feature for sharing values. Redux is a separate state-management library/ecosystem.

---

# 🧪 3️⃣6️⃣ Practice Tasks

## Task 1 — Theme Context

Create:

```text id="p8m3q6"
ThemeContext
```

Provide:

```text id="k4q7n2"
"dark"
```

Then display:

```text id="r6m9s3"
Current Theme: dark
```

---

## Task 2 — User Context

Create:

```jsx id="w7n2p4"
const user = {
    name: "Ramesh",
    role: "Developer"
};
```

Share it using Context.

Display:

```text id="x3m8q5"
Name: Ramesh
Role: Developer
```

---

## Task 3 — Avoid Prop Drilling

Create:

```text id="s4q8m2"
App
 ↓
Header
 ↓
Navbar
 ↓
Profile
```

Share the username using Context instead of passing it through every component.

---

## Task 4 — Context + State

Create:

```text id="n7m3p9"
theme = "light"
```

Add a button:

```text id="v5q8r2"
Toggle Theme
```

Share both:

```text id="c6m2x7"
theme
setTheme
```

through Context.

---

# 🏆 3️⃣7️⃣ Mini Challenge — User Authentication Context

Create this structure:

```text id="k8q3m6"
src/
│
├── context/
│   └── AuthContext.jsx
│
├── components/
│   ├── Navbar.jsx
│   └── Profile.jsx
│
└── App.jsx
```

Create user information:

```jsx id="r4m7p2"
const user = {
    name: "Ramesh",
    role: "Full Stack Developer"
};
```

Make it available through Context.

Then `Navbar` should display:

```text id="w6q2n8"
Welcome Ramesh
```

and `Profile` should display:

```text id="p9m3s5"
Name: Ramesh
Role: Full Stack Developer
```

### Your flow:

```text id="y7k4q1"
App
 ↓
AuthContext
 ↓
Navbar
 ↓
Profile
```

No prop drilling.

---

# 📝 3️⃣8️⃣ 10-Question Quiz

## Q1. What problem does Context help solve?

### ✅ Answer

Prop drilling.

---

## Q2. Which function creates a Context?

### ✅ Answer

```jsx id="f3m8q2"
createContext()
```

---

## Q3. Which Hook reads Context?

### ✅ Answer

```jsx id="r7n2p5"
useContext()
```

---

## Q4. What does a Provider do?

### ✅ Answer

It makes a context value available to components below it.

---

## Q5. Is Context useful for theme settings?

### ✅ Answer

Yes.

---

## Q6. Should every component use Context?

### ❌ Answer

No.

Use it when shared data actually needs to be accessed by multiple components.

---

## Q7. What is prop drilling?

### ✅ Answer

Passing props through intermediate components just to reach a deeper component.

---

## Q8. Can Context provide functions?

### ✅ Answer

Yes.

For example:

```jsx id="v5q9m3"
{
    user,
    logout
}
```

---

## Q9. Can Context work together with `useState`?

### ✅ Answer

Yes.

A common pattern is to provide both state and its update function through Context.

---

## Q10. Is Context the same as Redux?

### ❌ Answer

No.

Context is built into React, while Redux is a separate state-management solution.

---

# 📚 3️⃣9️⃣ Quick Revision

| Concept | Meaning |
|---|---|
| 🌐 Context | Shared data mechanism |
| 🚧 Prop Drilling | Passing props through unnecessary layers |
| `createContext()` | Creates Context |
| Provider | Supplies the value |
| `useContext()` | Reads the value |
| 🎨 Theme | Common Context use |
| 🔐 Auth | Common Context use |
| 🌍 Language | Common Context use |
| 🛒 Cart | Common Context use |
| 🟢 `useState` | Local/changing UI state |
| 🔵 Context | Shared data |

---

# 🧠 4️⃣0️⃣ Final Memory Trick

Remember:

```text id="z5q8m3"
CREATE
  ↓
createContext()

PROVIDE
  ↓
Provider

READ
  ↓
useContext()
```

### ⭐ Three-word memory

> **Create → Provide → Read**

And remember the main purpose:

```text id="m7p2q9"
Props
 ↓
Prop Drilling 🚧
 ↓
Context 🌐
 ↓
Shared Data
```

---

# ✅ Chapter 14 Checklist

Before moving forward, make sure you understand:

- [ ] What is Context?
- [ ] What is prop drilling?
- [ ] Why Context is useful
- [ ] `createContext()`
- [ ] Provider
- [ ] `useContext()`
- [ ] Sharing objects
- [ ] Sharing functions
- [ ] Context + `useState`
- [ ] Theme example
- [ ] Authentication example
- [ ] Props vs Context
- [ ] When NOT to use Context
- [ ] Common mistakes

---

# 🚀 NEXT CHAPTER

## 🔄 CHAPTER 15 — `useReducer`

Next we will learn:

```text id="n8q3m6"
🔄 useReducer
      ↓
State Management
      ↓
Actions
      ↓
Reducer Function
      ↓
dispatch()
      ↓
Complex State
      ↓
useState vs useReducer
```

Type **`NEXT`** when you are ready for **Chapter 15 — `useReducer`**.