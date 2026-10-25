
# 🔀 REACTJS — CHAPTER 8
# 🎯 CONDITIONAL RENDERING

Your course syllabus defines Chapter 8 around `if/else`, ternary operators, logical `&&` and `||`, multiple conditions, conditional components, loading UI, error UI, and empty-state UI.

---

# 🎯 Chapter Goal

After this chapter, you will understand how React can show **different UI depending on a condition**.

For example:

```text
User is logged in
        ↓
Show Dashboard

User is NOT logged in
        ↓
Show Login
```

Or:

```text
Data is loading
        ↓
⏳ Loading...

Data loaded
        ↓
📦 Show Products

Error occurred
        ↓
❌ Show Error
```

---

# 1️⃣ What Is Conditional Rendering?

## 🧠 Simple Definition

**Conditional rendering means showing different UI depending on a condition.**

In simple words:

> "If something is true, show this. Otherwise, show something else."

This is very common in real applications.

---

# 2️⃣ Real-World Example 🏠

Think about a traffic signal.

```text
Signal = RED
    ↓
🛑 STOP

Signal = GREEN
    ↓
🚗 GO
```

The action changes based on the condition.

React works similarly:

```text
Condition
   ↓
True  → Show UI A
False → Show UI B
```

---

# 3️⃣ Why Do We Need Conditional Rendering?

Real applications constantly need decisions.

### Login

```text
Logged in?
   ↓
Yes → Dashboard
No  → Login Page
```

### Loading

```text
Data loading?
   ↓
Yes → Loading...
No  → Show data
```

### Error

```text
Error?
   ↓
Yes → Error message
No  → Normal UI
```

### Empty data

```text
Products available?
   ↓
Yes → Product list
No  → No products found
```

So conditional rendering is extremely important in React.

---

# 4️⃣ Basic `if / else`

Let's start with normal JavaScript.

```jsx
function App() {

    const isLoggedIn = true;

    if (isLoggedIn) {
        return <h1>Welcome User</h1>;
    } else {
        return <h1>Please Login</h1>;
    }
}

export default App;
```

---

# 5️⃣ Dry Run 🧠

We have:

```jsx
const isLoggedIn = true;
```

So:

```text
isLoggedIn
    ↓
true
    ↓
if condition
    ↓
TRUE
    ↓
Welcome User
```

Browser:

```text
Welcome User
```

---

# 6️⃣ What If It Is `false`?

Change:

```jsx
const isLoggedIn = false;
```

Now:

```text
isLoggedIn
    ↓
false
    ↓
if condition
    ↓
FALSE
    ↓
else
    ↓
Please Login
```

Browser:

```text
Please Login
```

---

# 7️⃣ Important Point ⚠️

You **cannot directly put a normal `if` statement inside JSX** like this:

```jsx
return (
    <div>

        if (isLoggedIn) {
            <h1>Welcome</h1>
        }

    </div>
);
```

❌ This is not valid JSX.

Instead, you can use:

- `if / else` before `return`
- ternary operator
- `&&`
- `||`

We will learn each one.

---

# 8️⃣ `if / else` With a Component

Example:

```jsx
function App() {

    const isAdmin = true;

    if (isAdmin) {
        return <h1>Admin Dashboard</h1>;
    }

    return <h1>User Dashboard</h1>;
}

export default App;
```

Here:

```text
isAdmin = true
     ↓
Admin Dashboard
```

If:

```text
isAdmin = false
     ↓
User Dashboard
```

---

# 9️⃣ Ternary Operator `? :` ⭐

The ternary operator is one of the most commonly used ways to conditionally render small pieces of JSX.

Syntax:

```text
condition ? trueResult : falseResult
```

Think:

```text
condition ?
    YES
    :
    NO
```

---

# 🔟 Simple Ternary Example

```jsx
function App() {

    const isLoggedIn = true;

    return (
        <div>

            <h1>
                {isLoggedIn ? "Welcome User" : "Please Login"}
            </h1>

        </div>
    );
}

export default App;
```

---

# 1️⃣1️⃣ Dry Run

Condition:

```jsx
isLoggedIn
```

Value:

```text
true
```

Therefore:

```text
true ? "Welcome User" : "Please Login"
```

Result:

```text
Welcome User
```

If it becomes:

```jsx
const isLoggedIn = false;
```

Result:

```text
Please Login
```

---

# 1️⃣2️⃣ Ternary With JSX

We can also return complete JSX.

```jsx
function App() {

    const isLoggedIn = true;

    return (
        <div>

            {isLoggedIn ? (
                <h1>Welcome to Dashboard</h1>
            ) : (
                <h1>Please Login</h1>
            )}

        </div>
    );
}

export default App;
```

---

# 1️⃣3️⃣ Ternary Structure 🧠

Remember:

```jsx
condition ? A : B
```

Means:

```text
condition
   ↓
 ┌───────┐
 │ TRUE? │
 └───┬───┘
     │
  ┌──┴───┐
  ↓      ↓
  A      B
```

More simply:

```text
true  → A
false → B
```

---

# 1️⃣4️⃣ Real Example — User Status

```jsx
function App() {

    const isOnline = true;

    return (
        <div>

            <h1>
                {isOnline ? "🟢 Online" : "🔴 Offline"}
            </h1>

        </div>
    );
}

export default App;
```

Output:

```text
🟢 Online
```

If:

```jsx
const isOnline = false;
```

Output:

```text
🔴 Offline
```

---

# 1️⃣5️⃣ Logical AND `&&`

Another important technique is:

```jsx
&&
```

It is useful when you want to show something **only when a condition is true**.

Syntax:

```jsx
condition && <Component />
```

Meaning:

```text
If condition is true
        ↓
Show Component
```

If false:

```text
Show nothing
```

---

# 1️⃣6️⃣ Basic `&&` Example

```jsx
function App() {

    const isLoggedIn = true;

    return (
        <div>

            {isLoggedIn && <h1>Welcome User</h1>}

        </div>
    );
}

export default App;
```

Because:

```text
isLoggedIn = true
```

React renders:

```text
Welcome User
```

---

# 1️⃣7️⃣ What Happens When It Is False?

```jsx
const isLoggedIn = false;
```

Then:

```jsx
{isLoggedIn && <h1>Welcome User</h1>}
```

becomes conceptually:

```text
false && something
```

The UI is not rendered.

So:

```text
Browser
   ↓
Nothing
```

---

# 1️⃣8️⃣ `&&` vs Ternary

This difference is very important.

### Ternary

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

Use when you have:

```text
TRUE  → one UI
FALSE → another UI
```

### `&&`

```jsx
{isLoggedIn && <Dashboard />}
```

Use when you have:

```text
TRUE  → show UI
FALSE → show nothing
```

---

# 1️⃣9️⃣ Easy Memory Trick 🧠

```text
? : 
 ↓
YES or NO

&&
 ↓
YES or NOTHING
```

---

# 2️⃣0️⃣ Practical `&&` Example

Suppose we want to show an admin button only for admins.

```jsx
function App() {

    const isAdmin = true;

    return (
        <div>

            <h1>Dashboard</h1>

            {isAdmin && (
                <button>
                    Admin Panel
                </button>
            )}

        </div>
    );
}

export default App;
```

Output:

```text
Dashboard

[ Admin Panel ]
```

If:

```jsx
const isAdmin = false;
```

Output:

```text
Dashboard
```

The admin button disappears.

---

# 2️⃣1️⃣ Logical OR `||`

The `||` operator means:

> Use another value when the first value is unavailable/falsy.

Example:

```jsx
function App() {

    const name = "";

    return (
        <h1>
            Hello {name || "Guest"}
        </h1>
    );
}

export default App;
```

Because `name` is empty:

```text
name
 ↓
empty
 ↓
use "Guest"
```

Output:

```text
Hello Guest
```

---

# 2️⃣2️⃣ Practical `||` Example

```jsx
function App() {

    const username = "";

    return (
        <h1>
            Welcome {username || "Guest User"}
        </h1>
    );
}

export default App;
```

Output:

```text
Welcome Guest User
```

If:

```jsx
const username = "Ramesh";
```

Output:

```text
Welcome Ramesh
```

---

# 2️⃣3️⃣ `&&` and `||` Together

You may see patterns like:

```jsx
{isLoggedIn && <Dashboard />}
```

and:

```jsx
{username || "Guest"}
```

Their basic idea:

```text
&&
→ Show something when condition is true

||
→ Use a fallback when the first value is falsy
```

---

# 2️⃣4️⃣ Multiple Conditions

Sometimes there are more than two possibilities.

For example:

```text
Marks >= 90 → Excellent
Marks >= 60 → Good
Marks >= 40 → Pass
Otherwise   → Fail
```

We can use `if / else if / else`.

```jsx
function App() {

    const marks = 75;

    if (marks >= 90) {
        return <h1>Excellent</h1>;
    } else if (marks >= 60) {
        return <h1>Good</h1>;
    } else if (marks >= 40) {
        return <h1>Pass</h1>;
    } else {
        return <h1>Fail</h1>;
    }
}

export default App;
```

---

# 2️⃣5️⃣ Dry Run

Suppose:

```jsx
const marks = 75;
```

Check:

```text
75 >= 90
   ↓
false

75 >= 60
   ↓
true
```

Therefore:

```text
Good
```

---

# 2️⃣6️⃣ Multiple Conditions With Ternary

You can technically nest ternaries:

```jsx
const marks = 75;

return (
    <h1>
        {marks >= 90
            ? "Excellent"
            : marks >= 60
                ? "Good"
                : "Needs Improvement"}
    </h1>
);
```

But be careful.

Too many nested ternaries can make code difficult to read.

### 🧠 Beginner Best Practice

For simple:

```text
true / false
```

Use ternary.

For many conditions:

```text
condition 1
condition 2
condition 3
condition 4
```

Prefer `if / else if / else` or a cleaner component/function structure.

---

# 2️⃣7️⃣ Conditional Components 🧩

We can create separate components for different situations.

Example:

```jsx
function Login() {
    return <h1>Please Login</h1>;
}

function Dashboard() {
    return <h1>Welcome to Dashboard</h1>;
}

function App() {

    const isLoggedIn = true;

    if (isLoggedIn) {
        return <Dashboard />;
    }

    return <Login />;
}

export default App;
```

Now our application is easier to organize.

---

# 2️⃣8️⃣ Why Separate Components?

Imagine a large application.

Instead of:

```text
App.jsx
 └── 1000 lines
```

we can have:

```text
App
│
├── Header
├── Login
├── Dashboard
├── Sidebar
├── ProductList
└── Footer
```

Conditional rendering can decide which component to display.

---

# 2️⃣9️⃣ Loading UI ⏳

This is extremely important in real React applications.

Suppose we are getting data from an API.

Initially:

```text
API request
   ↓
Waiting...
```

We can show:

```text
⏳ Loading...
```

After data arrives:

```text
📦 Product List
```

Conceptually:

```jsx
function App() {

    const isLoading = true;

    return (
        <div>

            {isLoading ? (
                <h1>⏳ Loading...</h1>
            ) : (
                <h1>📦 Products Loaded</h1>
            )}

        </div>
    );
}

export default App;
```

---

# 3️⃣0️⃣ Loading Flow

```text
Application starts
       ↓
API request
       ↓
isLoading = true
       ↓
⏳ Loading...
       ↓
Data arrives
       ↓
isLoading = false
       ↓
📦 Product list
```

Later, `useState` and `useEffect` will make this dynamic.

For now, understand the **logic**.

---

# 3️⃣1️⃣ Error UI ❌

Sometimes an API fails.

Example:

```text
Server problem
Network problem
Invalid request
```

We can display an error message.

```jsx
function App() {

    const hasError = true;

    return (
        <div>

            {hasError ? (
                <h1>❌ Something went wrong</h1>
            ) : (
                <h1>Data loaded successfully</h1>
            )}

        </div>
    );
}

export default App;
```

---

# 3️⃣2️⃣ Error Flow

```text
API request
    ↓
Success? ───── Yes → Show data
    │
    No
    ↓
❌ Show error
```

This is a very common pattern in production applications.

---

# 3️⃣3️⃣ Empty-State UI 📭

What if the API succeeds but there is no data?

For example:

```text
Products = []
```

Instead of showing a blank page, show:

```text
📭 No products found
```

Example:

```jsx
function App() {

    const products = [];

    return (
        <div>

            {products.length === 0 ? (
                <h1>📭 No products found</h1>
            ) : (
                <h1>📦 Products available</h1>
            )}

        </div>
    );
}

export default App;
```

---

# 3️⃣4️⃣ Why Empty State Is Important

A professional application should explain what is happening.

### Bad UI ❌

```text
[Blank screen]
```

User thinks:

> "Is the application broken?"

### Better UI ✅

```text
📭 No products found
```

User understands:

> "There are currently no products."

---

# 3️⃣5️⃣ Three Important UI States ⭐

Remember these:

```text
⏳ LOADING
     ↓
Data is being requested

📦 SUCCESS
     ↓
Data is available

❌ ERROR
     ↓
Something went wrong
```

And:

```text
📭 EMPTY
     ↓
Request succeeded but no data exists
```

This pattern will become extremely important when we work with APIs.

---

# 3️⃣6️⃣ Real-World Product Example 🛒

Imagine an e-commerce application.

```text
                Product Page
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
       Loading     Success     Error
          ↓          ↓          ↓
      ⏳ Wait      📦 List     ❌ Error
                     │
                     ↓
               Products = []
                     │
                     ↓
                📭 Empty
```

This is how a real application thinks about UI states.

---

# 3️⃣7️⃣ Conditional Rendering Decision Tree 🧠

Before writing React code, ask:

```text
What should I show?
       ↓
Is there a condition?
       ↓
     YES
       ↓
How many outcomes?
       ↓
 ┌─────┴─────┐
 ↓           ↓
2 outcomes  Many outcomes
 ↓           ↓
Ternary     if/else
or &&       if/else-if
```

---

# 3️⃣8️⃣ Which Method Should I Use?

| Situation | Recommended |
|---|---|
| Two different UI choices | `? :` |
| Show only when true | `&&` |
| Fallback value | `||` |
| Many conditions | `if / else if / else` |
| Large conditional UI | Separate components |
| Loading vs data | Ternary / conditional component |
| Error vs success | Ternary / conditional component |
| Empty data | Conditional rendering |

---

# 3️⃣9️⃣ Common Mistakes ❌

## Mistake 1 — Trying to use `if` directly inside JSX

❌

```jsx
return (
    <div>
        if (isLoggedIn) {
            <h1>Welcome</h1>
        }
    </div>
);
```

Use a JavaScript `if` before the `return`, or use a JSX-friendly conditional expression.

---

# 4️⃣0️⃣ Mistake 2 — Confusing `&&` and ternary

This:

```jsx
{isLoggedIn && <Dashboard />}
```

does **not** mean:

```text
true  → Dashboard
false → Login
```

It means:

```text
true  → Dashboard
false → nothing
```

If you need Login:

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

---

# 4️⃣1️⃣ Mistake 3 — Too Many Nested Ternaries

Avoid code like:

```jsx
condition1
    ? condition2
        ? condition3
            ? ...
            : ...
        : ...
    : ...
```

It becomes difficult to understand.

Use clearer logic.

---

# 4️⃣2️⃣ Mistake 4 — Forgetting Empty Data

When building API applications, don't only think:

```text
Success
Error
```

Also think:

```text
Loading
Success
Empty
Error
```

This is a very useful professional habit.

---

# ⭐ 4️⃣3️⃣ Best Practices

### ✅ 1. Keep conditions easy to understand

Good:

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

---

### ✅ 2. Use descriptive variable names

Good:

```jsx
const isLoading = true;
const hasError = false;
const isLoggedIn = true;
```

Better than:

```jsx
const x = true;
const y = false;
```

---

### ✅ 3. Separate large UI into components

Instead of a huge `App.jsx`:

```text
Loading
Error
Empty
ProductList
```

can become separate components.

---

### ✅ 4. Always think about UI states

For data-driven applications:

```text
⏳ Loading
📦 Success
📭 Empty
❌ Error
```

---

# 🧪 4️⃣4️⃣ Practice Tasks

## Practice 1 — Login

Create:

```jsx
const isLoggedIn = true;
```

Show:

```text
Welcome User
```

when true.

Show:

```text
Please Login
```

when false.

---

## Practice 2 — Admin

Create:

```jsx
const isAdmin = true;
```

Show:

```text
Admin Panel
```

only when the user is an admin.

Use:

```jsx
&&
```

---

## Practice 3 — Online Status

Create:

```jsx
const isOnline = false;
```

Display:

```text
🟢 Online
```

or:

```text
🔴 Offline
```

Use a ternary.

---

## Practice 4 — Marks

Create:

```jsx
const marks = 85;
```

Display:

```text
Excellent
Good
Pass
Fail
```

depending on the marks.

Use `if / else if / else`.

---

## Practice 5 — Products

Create:

```jsx
const products = [];
```

If there are no products:

```text
📭 No products found
```

Otherwise:

```text
📦 Products available
```

---

# 🏆 4️⃣5️⃣ Mini Challenge

Build a **User Dashboard Status**.

Start with:

```jsx
const isLoggedIn = true;
const isLoading = false;
const hasError = false;
```

Your UI should logically support:

### Loading

```text
⏳ Loading...
```

### Error

```text
❌ Something went wrong
```

### Logged out

```text
🔐 Please Login
```

### Logged in

```text
👋 Welcome to Dashboard
```

### 🧠 Think first

Before coding, decide the priority:

```text
Is loading?
    ↓
Yes → Loading

No
 ↓
Has error?
 ↓
Yes → Error

No
 ↓
Logged in?
 ↓
Yes → Dashboard
No  → Login
```

This is the type of thinking you will use later in real React applications.

---

# 🎤 4️⃣6️⃣ Interview Questions

## Q1. What is conditional rendering?

### Answer

Conditional rendering means displaying different UI based on a condition.

---

## Q2. What is the ternary operator?

### Answer

It is a short way to choose between two results.

```jsx
condition ? trueResult : falseResult
```

---

## Q3. When would you use `&&`?

### Answer

When you want to render something only when a condition is true.

```jsx
{isAdmin && <AdminPanel />}
```

---

## Q4. What is the difference between `&&` and ternary?

### Answer

`&&` is commonly used for:

```text
true → show something
false → show nothing
```

Ternary is commonly used for:

```text
true → show A
false → show B
```

---

## Q5. What is `||` useful for?

### Answer

It can provide a fallback when the first value is falsy.

```jsx
username || "Guest"
```

---

## Q6. Can we directly use an `if` statement inside JSX?

### Answer

No. A normal `if` statement cannot be placed directly inside JSX markup.

You can use `if` before the `return`, or use expressions such as ternary and `&&`.

---

## Q7. What is a loading state?

### Answer

A loading state represents the period when the application is waiting for data or another operation to complete.

Example:

```text
⏳ Loading...
```

---

## Q8. What is an empty state?

### Answer

An empty state is UI shown when the operation succeeds but there is no data to display.

Example:

```text
📭 No products found
```

---

## Q9. Why do we need error UI?

### Answer

It tells the user that something went wrong instead of leaving them with a blank or confusing screen.

---

## Q10. What are common UI states for an API?

### Answer

A useful model is:

```text
⏳ Loading
📦 Success
📭 Empty
❌ Error
```

---

# 📝 4️⃣7️⃣ 10-Question Quiz

## Q1. What does conditional rendering mean?

### ✅ Answer

Showing different UI based on a condition.

---

## Q2. Which operator is commonly used for two alternatives?

### ✅ Answer

The ternary operator:

```jsx
condition ? A : B
```

---

## Q3. Which operator can show UI only when a condition is true?

### ✅ Answer

```jsx
&&
```

---

## Q4. What does this mean?

```jsx
{isLoggedIn && <Dashboard />}
```

### ✅ Answer

Show `Dashboard` when `isLoggedIn` is truthy; otherwise render nothing from this expression.

---

## Q5. What does this mean?

```jsx
{isLoggedIn ? <Dashboard /> : <Login />}
```

### ✅ Answer

Show `Dashboard` when logged in; otherwise show `Login`.

---

## Q6. What is `||` commonly used for?

### ✅ Answer

Providing a fallback value.

Example:

```jsx
username || "Guest"
```

---

## Q7. Which approach is better for many conditions?

### ✅ Answer

Usually `if / else if / else` or a clear component/function structure.

---

## Q8. What should you show while an API request is running?

### ✅ Answer

A loading UI such as:

```text
⏳ Loading...
```

---

## Q9. What should you show when the API succeeds but returns no records?

### ✅ Answer

An empty-state UI such as:

```text
📭 No products found
```

---

## Q10. What are four useful API UI states?

### ✅ Answer

```text
⏳ Loading
📦 Success
📭 Empty
❌ Error
```

---

# 🧠 4️⃣8️⃣ Final Memory Trick

Remember these three:

```text
? :
↓
YES or NO

&&
↓
YES or NOTHING

||
↓
VALUE or FALLBACK
```

And for real applications:

```text
⏳ Loading
    ↓
📦 Success
    ↓
📭 Empty
    ↓
❌ Error
```

---

# 📚 4️⃣9️⃣ Quick Revision

```text
if / else
    ↓
Useful for normal JavaScript conditions

? :
    ↓
Two UI choices

&&
    ↓
Show UI only when true

||
    ↓
Fallback value

Conditional Component
    ↓
Separate UI into reusable components

Loading UI
    ↓
⏳ Waiting for data

Error UI
    ↓
❌ Something went wrong

Empty UI
    ↓
📭 No data available
```

---

# ✅ Chapter 8 Checklist

Before moving to the next chapter, make sure you can explain:

- [ ] What is conditional rendering?
- [ ] `if / else`
- [ ] Ternary operator
- [ ] `&&`
- [ ] `||`
- [ ] Multiple conditions
- [ ] Conditional components
- [ ] Loading UI
- [ ] Error UI
- [ ] Empty-state UI
- [ ] Difference between `&&` and ternary
- [ ] When to use `if / else`
- [ ] How conditional rendering works in real applications

---

# 🚀 NEXT CHAPTER

## 🔥 CHAPTER 9 — `useState`

This is a **very important React chapter**.

We will learn:

```text
🧠 What is State?
       ↓
⚛️ useState
       ↓
📦 State Variable
       ↓
🔄 Updating State
       ↓
🔁 Re-rendering
       ↓
➕ Counter
       ↓
📝 Input State
       ↓
🔀 Multiple State Values
```

And most importantly, we will understand **why React needs state** before writing complicated code.

🛑 **STOP HERE.**

Type **`NEXT`** when you are ready for **Chapter 9 — useState**.