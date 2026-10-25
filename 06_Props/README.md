# 📦 REACTJS — CHAPTER 6
# 🔄 PROPS

---

# 🎯 Chapter Goal

After this chapter, you should understand:

- 📦 What are props?
- 🤔 Why do we need props?
- 📤 Passing props
- 📥 Receiving props
- 🔢 Multiple props
- ✂️ Props destructuring
- 🛡️ Default values
- 📦 Passing objects
- 📋 Passing arrays
- 🔧 Passing functions
- ⚖️ Props vs normal variables
- 🔒 Why props are read-only

---

# 1️⃣ What Are Props?

## 🧠 Simple Definition

**Props are data passed from a parent component to a child component.**

The word **props** comes from **properties**.

Think of props like information given to a component.

For example:

```text
Parent
  |
  | name = "Ramesh"
  ↓
Child
```

The child receives:

```text
name = "Ramesh"
```

and can display it.

---

# 2️⃣ Why Do We Need Props?

In Chapter 5, we created reusable components.

For example:

```jsx
<ProductCard />
<ProductCard />
<ProductCard />
```

But there is a problem.

What if we want:

```text
Product 1 → Laptop → ₹50,000
Product 2 → Mobile → ₹25,000
Product 3 → Keyboard → ₹2,000
```

We don't want to create three different components.

Instead, we create **one reusable component**:

```jsx
<ProductCard />
```

and send different data using props.

```jsx
<ProductCard name="Laptop" price={50000} />
<ProductCard name="Mobile" price={25000} />
<ProductCard name="Keyboard" price={2000} />
```

Now the same component can display different data.

### 🧠 Main Idea

```text
One Component
      +
Different Props
      ↓
Different UI
```

---

# 3️⃣ Real-World Analogy 📦

Imagine you have a delivery box.

The box is the:

```text
📦 Component
```

The information written on the box is:

```text
Name
Address
Phone
Product
```

That information is like:

```text
Props
```

So:

```text
📦 Component
      ↑
      |
   Props/Data
```

The component receives the information and uses it.

---

# 4️⃣ First Props Example

Let's create a child component.

### `User.jsx`

```jsx
function User(props) {

    return (
        <h1>Hello {props.name}</h1>
    );
}

export default User;
```

Now use it from `App.jsx`.

```jsx
import User from "./User";

function App() {

    return (
        <div>
            <User name="Ramesh" />
        </div>
    );
}

export default App;
```

### Output

```text
Hello Ramesh
```

---

# 5️⃣ How Does This Work?

Look at:

```jsx
<User name="Ramesh" />
```

We are sending:

```text
name = "Ramesh"
```

to `User`.

Then:

```jsx
function User(props) {
```

The child receives the props object.

Inside:

```jsx
props.name
```

means:

> Get the `name` value from the props object.

So:

```text
props
│
└── name
      ↓
   "Ramesh"
```

---

# 6️⃣ Props Flow ⭐

This is one of the most important concepts.

```text
        PARENT
          |
          | props
          ↓
        CHILD
```

Example:

```text
App
 |
 | name="Ramesh"
 ↓
User
```

### 🧠 Remember

> **Props normally flow from Parent → Child.**

---

# 7️⃣ Passing a String Prop

Example:

```jsx
<User name="Ramesh" />
```

Here:

```text
name
```

is the prop name.

```text
"Ramesh"
```

is the prop value.

Think:

```text
name = "Ramesh"
```

---

# 8️⃣ Receiving Props

The child component receives the props through its function parameter.

```jsx
function User(props) {

    return (
        <h1>{props.name}</h1>
    );
}
```

Here:

```text
props
```

is an object.

Conceptually:

```javascript
props = {
    name: "Ramesh"
}
```

Therefore:

```jsx
props.name
```

returns:

```text
Ramesh
```

---

# 9️⃣ Passing Multiple Props

We can send multiple values.

```jsx
<User
    name="Ramesh"
    age={25}
    city="Hyderabad"
/>
```

Now the child can access:

```jsx
props.name
props.age
props.city
```

Example:

```jsx
function User(props) {

    return (
        <div>
            <h2>{props.name}</h2>
            <p>Age: {props.age}</p>
            <p>City: {props.city}</p>
        </div>
    );
}

export default User;
```

### Output

```text
Ramesh
Age: 25
City: Hyderabad
```

---

# 🔟 Why Do We Use `{}` for Numbers?

Look at:

```jsx
<User age={25} />
```

We use:

```jsx
{25}
```

because we are passing a JavaScript value.

Compare:

```jsx
age="25"
```

and:

```jsx
age={25}
```

The first is a string.

The second is a JavaScript number.

### 🧠 Important

```text
"25" → String

{25} → Number
```

---

# 1️⃣1️⃣ Passing Boolean Props

We can pass boolean values.

```jsx
<User isActive={true} />
```

The child can access:

```jsx
props.isActive
```

Example:

```jsx
function User(props) {

    return (
        <p>Active: {props.isActive ? "Yes" : "No"}</p>
    );
}
```

Output:

```text
Active: Yes
```

---

# 1️⃣2️⃣ Props Destructuring ⭐

This is very common in React.

Instead of:

```jsx
function User(props) {

    return (
        <div>
            <h1>{props.name}</h1>
            <p>{props.age}</p>
            <p>{props.city}</p>
        </div>
    );
}
```

we can use destructuring.

```jsx
function User({ name, age, city }) {

    return (
        <div>
            <h1>{name}</h1>
            <p>{age}</p>
            <p>{city}</p>
        </div>
    );
}
```

Much cleaner.

---

# 1️⃣3️⃣ Understanding Destructuring

Suppose:

```javascript
const props = {
    name: "Ramesh",
    age: 25,
    city: "Hyderabad"
};
```

Without destructuring:

```javascript
props.name
props.age
props.city
```

With destructuring:

```javascript
const { name, age, city } = props;
```

Now we can simply use:

```javascript
name
age
city
```

---

# 1️⃣4️⃣ Complete Destructuring Example

### Parent

```jsx
function App() {

    return (
        <User
            name="Ramesh"
            age={25}
            city="Hyderabad"
        />
    );
}

export default App;
```

### Child

```jsx
function User({ name, age, city }) {

    return (
        <div>
            <h1>{name}</h1>
            <p>Age: {age}</p>
            <p>City: {city}</p>
        </div>
    );
}

export default User;
```

### Output

```text
Ramesh
Age: 25
City: Hyderabad
```

---

# 1️⃣5️⃣ Props Default Values

What happens if a prop is not provided?

Example:

```jsx
<User name="Ramesh" />
```

But the component expects:

```text
name
age
city
```

The missing values will be `undefined` unless you provide a default.

We can give a default value using destructuring.

```jsx
function User({
    name,
    age = 18,
    city = "Hyderabad"
}) {

    return (
        <div>
            <h1>{name}</h1>
            <p>Age: {age}</p>
            <p>City: {city}</p>
        </div>
    );
}
```

Now if we only pass:

```jsx
<User name="Ramesh" />
```

the result is:

```text
Ramesh
Age: 18
City: Hyderabad
```

---

# 1️⃣6️⃣ Passing Objects

Props can contain objects.

Parent:

```jsx
function App() {

    const user = {
        name: "Ramesh",
        age: 25,
        city: "Hyderabad"
    };

    return (
        <User user={user} />
    );
}

export default App;
```

Child:

```jsx
function User({ user }) {

    return (
        <div>
            <h1>{user.name}</h1>
            <p>Age: {user.age}</p>
            <p>City: {user.city}</p>
        </div>
    );
}

export default User;
```

### Output

```text
Ramesh
Age: 25
City: Hyderabad
```

---

# 1️⃣7️⃣ Why Are There Two `{}`?

Look carefully:

```jsx
<User user={user} />
```

The first:

```text
user
```

is the prop name.

The second:

```jsx
{user}
```

means:

> Use the JavaScript variable called `user`.

So:

```text
user={user}
 ↑     ↑
 |     |
prop   JS variable
name
```

This is very important.

---

# 1️⃣8️⃣ Passing Arrays

Props can also contain arrays.

Parent:

```jsx
function App() {

    const skills = [
        "React",
        "Python",
        "SQL"
    ];

    return (
        <Skills skills={skills} />
    );
}

export default App;
```

Child:

```jsx
function Skills({ skills }) {

    return (
        <div>
            <h2>Skills</h2>

            <p>{skills[0]}</p>
            <p>{skills[1]}</p>
            <p>{skills[2]}</p>
        </div>
    );
}

export default Skills;
```

Output:

```text
Skills

React
Python
SQL
```

Later, when we learn **Lists and Keys**, we will learn how to display arrays using `map()`.

---

# 1️⃣9️⃣ Passing Functions ⭐

This is a very important concept.

A parent can pass a function to a child.

Parent:

```jsx
function App() {

    function showMessage() {
        alert("Hello from Parent");
    }

    return (
        <Button onClick={showMessage} />
    );
}

export default App;
```

Child:

```jsx
function Button({ onClick }) {

    return (
        <button onClick={onClick}>
            Click Me
        </button>
    );
}

export default Button;
```

Now:

```text
Parent
  |
  | function
  ↓
Child
```

The child can call the function.

---

# 2️⃣0️⃣ Why Pass Functions?

This becomes very useful for:

```text
👨 Parent
    |
    | callback function
    ↓
👶 Child
```

The child can notify the parent that something happened.

For example:

```text
Child Button clicked
        ↓
Function runs
        ↓
Parent knows about it
```

We will study this more deeply in the **Events** and **Component Communication** chapters.

---

# 2️⃣1️⃣ Props vs Normal Variables

This is very important.

### Normal variable

```jsx
function App() {

    const name = "Ramesh";

    return <h1>{name}</h1>;
}
```

`name` is a normal local variable.

It belongs to the `App` function.

---

### Prop

```jsx
<User name="Ramesh" />
```

Here:

```text
name
```

is data passed from another component.

---

# 2️⃣2️⃣ Simple Difference

| Normal Variable | Prop |
|---|---|
| Created inside component | Passed to component |
| Local data | Data received from parent |
| Example: `const name = "Ramesh"` | Example: `<User name="Ramesh" />` |
| Not automatically shared | Used for parent → child data |

### 🧠 Memory Trick

```text
Variable
    ↓
I create it

Prop
    ↓
Parent gives it to me
```

---

# 2️⃣3️⃣ Props Are Read-Only 🔒

This is one of the most important React rules.

A child should **not modify its props**.

For example:

```jsx
function User({ name }) {

    name = "John";

    return <h1>{name}</h1>;
}
```

❌ Don't do this.

Props should be treated as read-only.

If the value needs to change because of user interaction, we will normally use **state**.

We will learn state later in:

```text
🧠 Chapter 9 — useState
```

---

# 2️⃣4️⃣ Why Are Props Read-Only?

Think about this:

```text
Parent
  |
  | name="Ramesh"
  ↓
Child
```

The parent owns the data being passed.

The child receives the data.

So the child should not directly change the parent's data through the prop.

Think:

```text
Parent owns data
       ↓
Child receives data
       ↓
Child reads data
```

---

# 2️⃣5️⃣ Complete Example — Student Card

Let's build a practical example.

### `StudentCard.jsx`

```jsx
function StudentCard({
    name,
    age,
    course,
    city
}) {

    return (
        <div>
            <h2>{name}</h2>

            <p>Age: {age}</p>

            <p>Course: {course}</p>

            <p>City: {city}</p>
        </div>
    );
}

export default StudentCard;
```

### `App.jsx`

```jsx
import StudentCard from "./StudentCard";

function App() {

    return (
        <div>
            <StudentCard
                name="Ramesh"
                age={25}
                course="ReactJS"
                city="Hyderabad"
            />
        </div>
    );
}

export default App;
```

Output:

```text
Ramesh
Age: 25
Course: ReactJS
City: Hyderabad
```

---

# 2️⃣6️⃣ Reusing the Same Component

Now the real power of props.

We can create multiple students:

```jsx
function App() {

    return (
        <div>

            <StudentCard
                name="Ramesh"
                age={25}
                course="ReactJS"
                city="Hyderabad"
            />

            <StudentCard
                name="Suresh"
                age={24}
                course="Python"
                city="Warangal"
            />

            <StudentCard
                name="Anil"
                age={26}
                course="SQL"
                city="Vijayawada"
            />

        </div>
    );
}
```

Same component:

```text
StudentCard
```

Different data:

```text
Ramesh
Suresh
Anil
```

This is the main benefit of props.

---

# 2️⃣7️⃣ Component Reusability Flow

```text
              StudentCard
                  ↑
        ┌─────────┼─────────┐
        │         │         │
        │         │         │
     Ramesh     Suresh     Anil
        │         │         │
      React      Python     SQL
```

One component.

Different props.

Different output.

---

# 2️⃣8️⃣ Practical Example — Product Card 🛒

Imagine an e-commerce website.

Parent:

```jsx
function App() {

    return (
        <div>

            <ProductCard
                name="Laptop"
                price={50000}
            />

            <ProductCard
                name="Mobile"
                price={25000}
            />

            <ProductCard
                name="Keyboard"
                price={2000}
            />

        </div>
    );
}
```

Child:

```jsx
function ProductCard({ name, price }) {

    return (
        <div>
            <h2>{name}</h2>

            <p>Price: ₹{price}</p>

            <button>
                Add to Cart
            </button>
        </div>
    );
}
```

The output will contain three different products.

---

# 2️⃣9️⃣ Dry Run 🧠

For:

```jsx
<ProductCard
    name="Laptop"
    price={50000}
/>
```

React passes:

```javascript
{
    name: "Laptop",
    price: 50000
}
```

The component receives:

```jsx
function ProductCard({ name, price }) {
```

So:

```text
name  → Laptop
price → 50000
```

Then:

```jsx
<h2>{name}</h2>
```

becomes:

```text
Laptop
```

And:

```jsx
<p>Price: ₹{price}</p>
```

becomes:

```text
Price: ₹50000
```

---

# 3️⃣0️⃣ Props Flow Diagram ⭐

Remember this diagram:

```text
                 PARENT
                   │
                   │
              Props/Data
                   │
                   ↓
                 CHILD
                   │
                   ↓
              Display UI
```

Example:

```text
App
 │
 │ name="Ramesh"
 │ age={25}
 ↓
User
 │
 ├── name
 └── age
```

---

# 3️⃣1️⃣ Common Beginner Mistakes ❌

## Mistake 1 — Forgetting `{}` for JavaScript values

If you have:

```javascript
const age = 25;
```

Use:

```jsx
<User age={age} />
```

not:

```jsx
<User age="age" />
```

The second passes the literal string `"age"`.

---

# 3️⃣2️⃣ Mistake 2 — Confusing Prop Name and Variable

Look at:

```jsx
<User name={userName} />
```

Here:

```text
name
```

is the prop name.

```text
userName
```

is the JavaScript variable.

---

# 3️⃣3️⃣ Mistake 3 — Wrong Destructuring

Suppose parent sends:

```jsx
<User name="Ramesh" />
```

Child should use:

```jsx
function User({ name }) {
```

not:

```jsx
function User({ username }) {
```

unless the parent actually sends:

```jsx
<User username="Ramesh" />
```

The names must match.

---

# 3️⃣4️⃣ Mistake 4 — Trying to Modify Props

Avoid:

```jsx
function User({ name }) {

    name = "John";

    return <h1>{name}</h1>;
}
```

Props should be treated as read-only.

---

# 3️⃣5️⃣ Mistake 5 — Passing a Function Incorrectly

Suppose:

```jsx
function showMessage() {
    alert("Hello");
}
```

Correct:

```jsx
<Button onClick={showMessage} />
```

Be careful with:

```jsx
<Button onClick={showMessage()} />
```

This calls the function immediately while rendering instead of passing the function itself.

We will study event handling more deeply in Chapter 7.

---

# 3️⃣6️⃣ Best Practices ⭐

### ✅ 1. Use meaningful prop names

Good:

```jsx
<User name="Ramesh" />
```

Avoid unclear names:

```jsx
<User x="Ramesh" />
```

---

### ✅ 2. Destructure when there are several props

Instead of:

```jsx
function User(props) {
    return (
        <h1>{props.name}</h1>
    );
}
```

you can write:

```jsx
function User({ name }) {
    return (
        <h1>{name}</h1>
    );
}
```

---

### ✅ 3. Keep props simple

Pass only the data the child needs.

---

### ✅ 4. Remember the direction

```text
Parent
  ↓
Child
```

---

### ✅ 5. Don't modify props

Treat props as read-only.

---

# 🎤 3️⃣7️⃣ Interview Questions

## Q1. What are props in React?

### Answer

Props are data passed from a parent component to a child component.

---

## Q2. Why do we use props?

### Answer

Props allow components to receive dynamic data and make components reusable.

---

## Q3. How do you pass props?

### Answer

Props are passed as attributes:

```jsx
<User name="Ramesh" age={25} />
```

---

## Q4. How do you receive props?

### Answer

Using the component function parameter:

```jsx
function User(props) {
    return <h1>{props.name}</h1>;
}
```

or destructuring:

```jsx
function User({ name }) {
    return <h1>{name}</h1>;
}
```

---

## Q5. What is props destructuring?

### Answer

Props destructuring extracts values from the props object.

Example:

```jsx
function User({ name, age }) {
```

instead of:

```jsx
function User(props) {
```

and then using:

```jsx
props.name
props.age
```

---

## Q6. Can we pass objects through props?

### Answer

Yes.

```jsx
<User user={user} />
```

---

## Q7. Can we pass arrays through props?

### Answer

Yes.

```jsx
<Skills skills={skills} />
```

---

## Q8. Can we pass functions through props?

### Answer

Yes.

Functions can be passed from parent to child as props.

```jsx
<Button onClick={showMessage} />
```

---

## Q9. Can a child modify props?

### Answer

Props should be treated as read-only. A child should not directly modify the props it receives.

---

## Q10. What is the difference between props and state?

### Answer

At a beginner level:

```text
Props
 ↓
Data received from parent

State
 ↓
Data managed inside a component
```

We will study state in detail later.

---

# 🧪 3️⃣8️⃣ Practice

Try these yourself.

## Practice 1 — Greeting

Create:

```jsx
<Greeting name="Ramesh" />
```

Display:

```text
Hello Ramesh
```

---

## Practice 2 — User

Pass:

```text
name
age
city
```

and display all three values.

---

## Practice 3 — Product

Create:

```jsx
<Product
    name="Laptop"
    price={50000}
/>
```

Display:

```text
Laptop
₹50000
```

---

## Practice 4 — Object

Create:

```javascript
const user = {
    name: "Ramesh",
    city: "Hyderabad"
};
```

Pass it to a child component.

---

## Practice 5 — Array

Create:

```javascript
const skills = [
    "React",
    "Python",
    "SQL"
];
```

Pass the array as a prop.

---

# 🏆 3️⃣9️⃣ Mini Challenge

Build a reusable:

```text
StudentCard
```

component.

### Parent should pass:

```text
name
age
course
city
```

Example:

```jsx
<StudentCard
    name="Ramesh"
    age={25}
    course="ReactJS"
    city="Hyderabad"
/>
```

Create **three students** using the same component.

Expected idea:

```text
┌─────────────────────┐
│ Ramesh              │
│ Age: 25             │
│ Course: ReactJS     │
│ City: Hyderabad     │
└─────────────────────┘

┌─────────────────────┐
│ Suresh              │
│ Age: 24             │
│ Course: Python      │
│ City: Warangal      │
└─────────────────────┘

┌─────────────────────┐
│ Anil                │
│ Age: 26             │
│ Course: SQL         │
│ City: Vijayawada    │
└─────────────────────┘
```

### 🧠 Your thinking process

```text
1. Create StudentCard
        ↓
2. Decide required props
        ↓
3. Pass props from App
        ↓
4. Receive props in StudentCard
        ↓
5. Display values
```

---

# 📝 4️⃣0️⃣ 10-Question Quiz

## Q1. What does Props mean?

### ✅ Answer

Props means **properties**. They are data passed from one component, normally a parent, to a child.

---

## Q2. What is the direction of normal props flow?

### ✅ Answer

```text
Parent → Child
```

---

## Q3. What does this pass?

```jsx
<User name="Ramesh" />
```

### ✅ Answer

It passes a prop called:

```text
name
```

with value:

```text
Ramesh
```

---

## Q4. What does this pass?

```jsx
<User age={25} />
```

### ✅ Answer

It passes:

```text
age = 25
```

as a JavaScript number.

---

## Q5. What is the difference?

```jsx
age="25"
```

and:

```jsx
age={25}
```

### ✅ Answer

```text
"25" → String
{25} → Number
```

---

## Q6. What does this mean?

```jsx
function User({ name }) {
```

### ✅ Answer

It destructures the `name` property from the props object.

---

## Q7. Can we pass an object through props?

### ✅ Answer

Yes.

```jsx
<User user={user} />
```

---

## Q8. Can we pass an array?

### ✅ Answer

Yes.

```jsx
<Skills skills={skills} />
```

---

## Q9. Can we pass a function?

### ✅ Answer

Yes.

```jsx
<Button onClick={showMessage} />
```

---

## Q10. Can a child directly modify props?

### ✅ Answer

No. Props should be treated as read-only.

---

# 📚 4️⃣1️⃣ Chapter Summary

| Concept | Simple Meaning |
|---|---|
| 📦 Props | Data passed to a component |
| 📤 Passing Props | Parent sends data |
| 📥 Receiving Props | Child receives data |
| 🔢 Multiple Props | Send several values |
| ✂️ Destructuring | Extract prop values |
| 🛡️ Default Value | Fallback value |
| 📦 Object Props | Pass an object |
| 📋 Array Props | Pass an array |
| 🔧 Function Props | Pass a function |
| 🔒 Read-Only | Don't modify props |

---

# 🧠 4️⃣2️⃣ Final Memory Trick

Remember this simple sentence:

> **Props = Parent gives data → Child receives data.**

Diagram:

```text
              PARENT
                 │
                 │
          name="Ramesh"
          age={25}
                 │
                 ↓
               CHILD
                 │
          ┌──────┴──────┐
          ↓             ↓
        name            age
          ↓             ↓
       Ramesh           25
```

### ⭐ One more important trick

```text
Props
  ↓
Passed from Parent
  ↓
Received by Child
  ↓
Used to display dynamic data
  ↓
Read-only
```

---

# ✅ Chapter 6 Checklist

Before moving ahead, make sure you can explain:

- [ ] What are props?
- [ ] Why do we need props?
- [ ] How to pass props?
- [ ] How to receive props?
- [ ] Multiple props
- [ ] String props
- [ ] Number props
- [ ] Boolean props
- [ ] Props destructuring
- [ ] Default values
- [ ] Object props
- [ ] Array props
- [ ] Function props
- [ ] Props vs normal variables
- [ ] Props are read-only
- [ ] Parent → Child data flow
- [ ] Reusable components using props

---

# 🚀 NEXT CHAPTER

## 🖱️ CHAPTER 7 — EVENTS

We will learn:

```text
🖱️ onClick
      ↓
⌨️ onChange
      ↓
📝 onSubmit
      ↓
🖱️ onMouseEnter
      ↓
🖱️ onMouseLeave
      ↓
⌨️ Keyboard Events
      ↓
📦 Event Object
      ↓
⚙️ Event Handlers
      ↓
🎯 Passing Arguments
      ↓
❌ Common Event Mistakes
```

These are exactly the event topics listed in your uploaded syllabus.

🛑 **STOP HERE.**

Type **`NEXT`** when you are ready for **Chapter 7 — Events**.