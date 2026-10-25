# 🎯 Chapter Goal

By the end of this chapter, you should understand:

```text
📦 Array
   ↓
🔁 map()
   ↓
🧩 React Component
   ↓
📋 List UI
   ↓
🔑 key
```

We will learn:

- Arrays in React
- `map()`
- Rendering multiple elements
- Rendering components from arrays
- `key`
- Why keys are required
- Correct key usage
- Common key mistakes
- Lists of objects
- Product cards
- Student lists
- Tables
- Dynamic UI

---

# 1️⃣ What Is a List?

A list means displaying multiple items.

For example:

```text
Students

1. Ramesh
2. Ravi
3. Suresh
4. Kiran
```

Or products:

```text
Products

Laptop
Mobile
Keyboard
Mouse
```

Or employees:

```text
Employees

Ramesh
Ravi
Suresh
Anil
```

In React, these items are usually stored in an **array**.

---

# 2️⃣ What Is an Array?

An array stores multiple values in one variable.

Example:

```jsx
const students = [
    "Ramesh",
    "Ravi",
    "Suresh",
    "Kiran"
];
```

Think of it like a box:

```text
students
   │
   ├── "Ramesh"
   ├── "Ravi"
   ├── "Suresh"
   └── "Kiran"
```

---

# 3️⃣ Why Do We Need Lists in React?

Imagine an e-commerce website.

There may be:

```text
100 products
```

Would you write:

```jsx
<h2>Laptop</h2>
<h2>Mobile</h2>
<h2>Keyboard</h2>
<h2>Mouse</h2>
...
```

for every product?

❌ No.

Instead:

```text
Products Array
      ↓
    map()
      ↓
Product Component
      ↓
100 Product Cards
```

This is much easier.

---

# 4️⃣ The `map()` Method ⭐⭐⭐

JavaScript provides a method called:

```jsx
map()
```

It is used to create a new array by processing each item.

Simple example:

```jsx
const numbers = [1, 2, 3, 4];

const result = numbers.map((number) => {
    return number * 2;
});

console.log(result);
```

Output:

```text
[2, 4, 6, 8]
```

---

# 5️⃣ Understand `map()` Step by Step

Suppose:

```jsx
const numbers = [1, 2, 3];
```

Then:

```jsx
numbers.map(...)
```

visits every item.

```text
1 → process
2 → process
3 → process
```

If we multiply each by 2:

```text
1 × 2 = 2
2 × 2 = 4
3 × 2 = 6
```

Result:

```text
[2, 4, 6]
```

---

# 6️⃣ `map()` in React

Now let's use `map()` to create HTML.

```jsx
function App() {

    const students = [
        "Ramesh",
        "Ravi",
        "Suresh"
    ];

    return (
        <div>

            {students.map((student) => (
                <h2>{student}</h2>
            ))}

        </div>
    );
}

export default App;
```

The browser displays:

```text
Ramesh
Ravi
Suresh
```

---

# 7️⃣ Why `{}`?

Inside JSX, JavaScript expressions go inside:

```jsx
{}
```

So:

```jsx
{students.map(...)}
```

means:

> Run JavaScript `map()` here and put the result into the UI.

---

# 8️⃣ Dry Run 🧠

Array:

```jsx
const students = [
    "Ramesh",
    "Ravi",
    "Suresh"
];
```

React executes:

```jsx
students.map(...)
```

First:

```text
student = "Ramesh"
```

React creates:

```jsx
<h2>Ramesh</h2>
```

Second:

```text
student = "Ravi"
```

React creates:

```jsx
<h2>Ravi</h2>
```

Third:

```text
student = "Suresh"
```

React creates:

```jsx
<h2>Suresh</h2>
```

Final UI:

```text
Ramesh
Ravi
Suresh
```

---

# 9️⃣ Important: `map()` Returns a New Array

Suppose:

```jsx
const numbers = [1, 2, 3];
```

and:

```jsx
const result = numbers.map((number) => number * 10);
```

Then:

```text
numbers
[1, 2, 3]

       ↓ map()

result
[10, 20, 30]
```

In React, the returned values can be JSX elements.

---

# 🔟 List With `<li>`

A common way to display lists is with:

```jsx
<ul>
    <li>...</li>
</ul>
```

Example:

```jsx
function App() {

    const cities = [
        "Hyderabad",
        "Warangal",
        "Vijayawada"
    ];

    return (
        <ul>

            {cities.map((city) => (
                <li>{city}</li>
            ))}

        </ul>
    );
}

export default App;
```

Output:

```text
• Hyderabad
• Warangal
• Vijayawada
```

---

# 1️⃣1️⃣ The Key Problem ⚠️

When you render a list like this:

```jsx
{students.map((student) => (
    <h2>{student}</h2>
))}
```

React will warn about something like:

```text
Each child in a list should have a unique "key" prop.
```

Why?

Because React needs to identify each item.

This is where **keys** come in.

---

# 🔑 1️⃣2️⃣ What Is a Key?

A `key` is a special React attribute used to identify items in a list.

Example:

```jsx
{students.map((student) => (
    <h2 key={student}>
        {student}
    </h2>
))}
```

Now each item has an identity.

Think:

```text
Ramesh  → key="Ramesh"
Ravi    → key="Ravi"
Suresh  → key="Suresh"
```

---

# 1️⃣3️⃣ Why Does React Need Keys?

Suppose the list is:

```text
Ramesh
Ravi
Suresh
```

Then a new student is added:

```text
Anil
Ramesh
Ravi
Suresh
```

React needs to understand:

```text
Which item is new?
Which items already exist?
Which item changed?
Which item was removed?
```

Keys help React identify individual list items.

---

# 1️⃣4️⃣ Real-World Analogy 🏷️

Imagine a classroom.

There are four students:

```text
Ramesh
Ravi
Suresh
Kiran
```

The teacher needs to identify them.

Student ID:

```text
101 → Ramesh
102 → Ravi
103 → Suresh
104 → Kiran
```

The ID uniquely identifies each student.

Similarly, React uses:

```jsx
key
```

to identify list items.

---

# 1️⃣5️⃣ Best Key — Unique ID ⭐

Suppose our data is:

```jsx
const students = [
    {
        id: 101,
        name: "Ramesh"
    },
    {
        id: 102,
        name: "Ravi"
    },
    {
        id: 103,
        name: "Suresh"
    }
];
```

Use:

```jsx
{students.map((student) => (
    <h2 key={student.id}>
        {student.name}
    </h2>
))}
```

This is a very good pattern.

---

# 1️⃣6️⃣ Why `id` Is Better

Suppose:

```text
Ramesh
Ramesh
Ravi
```

Names are not necessarily unique.

But IDs can be:

```text
101 → Ramesh
102 → Ramesh
103 → Ravi
```

Therefore:

```jsx
key={student.id}
```

is safer.

---

# 1️⃣7️⃣ Complete Student List

```jsx
function App() {

    const students = [
        {
            id: 101,
            name: "Ramesh",
            age: 25
        },
        {
            id: 102,
            name: "Ravi",
            age: 24
        },
        {
            id: 103,
            name: "Suresh",
            age: 26
        }
    ];

    return (
        <div>

            <h1>Students</h1>

            {students.map((student) => (
                <div key={student.id}>

                    <h2>{student.name}</h2>

                    <p>Age: {student.age}</p>

                </div>
            ))}

        </div>
    );
}

export default App;
```

---

# 1️⃣8️⃣ Dry Run

First object:

```text
id: 101
name: Ramesh
age: 25
```

React creates:

```jsx
<div key={101}>
    <h2>Ramesh</h2>
    <p>Age: 25</p>
</div>
```

Second:

```text
id: 102
name: Ravi
age: 24
```

React creates:

```jsx
<div key={102}>
    <h2>Ravi</h2>
    <p>Age: 24</p>
</div>
```

And so on.

---

# 1️⃣9️⃣ Rendering Components From Arrays ⭐

This is extremely useful.

Instead of creating HTML directly:

```jsx
{products.map((product) => (
    <div>
        {product.name}
    </div>
))}
```

we can create a reusable component.

Example:

```jsx
function ProductCard({ product }) {

    return (
        <div>

            <h2>{product.name}</h2>

            <p>Price: ₹{product.price}</p>

        </div>
    );
}
```

Then:

```jsx
function App() {

    const products = [
        {
            id: 1,
            name: "Laptop",
            price: 50000
        },
        {
            id: 2,
            name: "Mobile",
            price: 20000
        },
        {
            id: 3,
            name: "Keyboard",
            price: 1500
        }
    ];

    return (
        <div>

            {products.map((product) => (
                <ProductCard
                    key={product.id}
                    product={product}
                />
            ))}

        </div>
    );
}

export default App;
```

---

# 2️⃣0️⃣ Understand the Flow

```text
products array
      ↓
    map()
      ↓
each product
      ↓
ProductCard
      ↓
props
      ↓
UI
```

Example:

```text
Product 1
   ↓
ProductCard

Product 2
   ↓
ProductCard

Product 3
   ↓
ProductCard
```

---

# 2️⃣1️⃣ Where Should `key` Go?

This is important.

Correct:

```jsx
{products.map((product) => (
    <ProductCard
        key={product.id}
        product={product}
    />
))}
```

The `key` should be on the **outermost element returned directly from `map()`**.

---

# 2️⃣2️⃣ `key` Is Not Normal Props

This is important.

You cannot normally do:

```jsx
function ProductCard(props) {
    console.log(props.key);
}
```

`key` is a special React attribute.

If the component needs the ID, pass it separately:

```jsx
<ProductCard
    key={product.id}
    productId={product.id}
/>
```

Then:

```jsx
function ProductCard({ productId }) {

    console.log(productId);

}
```

Remember:

```text
key
→ React uses it

productId
→ Your component can use it
```

---

# 2️⃣3️⃣ Using Index as Key

You may see:

```jsx
{students.map((student, index) => (
    <h2 key={index}>
        {student}
    </h2>
))}
```

This can work for simple static lists.

But using array index as a key can cause problems when the list is:

- reordered
- inserted into
- deleted from
- dynamically changed

So, when you have a stable unique ID, prefer:

```jsx
key={student.id}
```

---

# 2️⃣4️⃣ Why Index Can Be a Problem

Suppose:

```text
Index 0 → Ramesh
Index 1 → Ravi
Index 2 → Suresh
```

Then we add a new person at the beginning:

```text
Index 0 → Anil
Index 1 → Ramesh
Index 2 → Ravi
Index 3 → Suresh
```

The indexes changed.

But student IDs remain stable:

```text
101 → Ramesh
102 → Ravi
103 → Suresh
104 → Anil
```

Therefore stable IDs are generally better.

---

# 2️⃣5️⃣ Product List 🛒

Let's create a realistic product list.

```jsx
function App() {

    const products = [
        {
            id: 1,
            name: "Laptop",
            price: 55000,
            category: "Electronics"
        },
        {
            id: 2,
            name: "Mobile",
            price: 25000,
            category: "Electronics"
        },
        {
            id: 3,
            name: "Keyboard",
            price: 1500,
            category: "Accessories"
        }
    ];

    return (
        <div>

            <h1>Products</h1>

            {products.map((product) => (

                <div key={product.id}>

                    <h2>{product.name}</h2>

                    <p>
                        Price: ₹{product.price}
                    </p>

                    <p>
                        Category: {product.category}
                    </p>

                </div>

            ))}

        </div>
    );
}

export default App;
```

---

# 2️⃣6️⃣ Result

The UI will look conceptually like:

```text
Products

Laptop
Price: ₹55000
Category: Electronics

Mobile
Price: ₹25000
Category: Electronics

Keyboard
Price: ₹1500
Category: Accessories
```

---

# 2️⃣7️⃣ Lists + Components = Reusable UI 🚀

A better architecture:

```text
App
 │
 ├── ProductCard
 │
 ├── ProductCard
 │
 └── ProductCard
```

Data:

```text
products[]
```

Flow:

```text
products[]
    ↓
map()
    ↓
ProductCard
    ↓
props
    ↓
UI
```

This pattern is used everywhere in real React applications.

---

# 2️⃣8️⃣ Rendering a Table

Lists are also useful for tables.

```jsx
function App() {

    const employees = [
        {
            id: 1,
            name: "Ramesh",
            role: "Developer"
        },
        {
            id: 2,
            name: "Ravi",
            role: "Tester"
        },
        {
            id: 3,
            name: "Suresh",
            role: "Manager"
        }
    ];

    return (
        <table>

            <thead>
                <tr>
                    <th>Name</th>
                    <th>Role</th>
                </tr>
            </thead>

            <tbody>

                {employees.map((employee) => (
                    <tr key={employee.id}>

                        <td>{employee.name}</td>

                        <td>{employee.role}</td>

                    </tr>
                ))}

            </tbody>

        </table>
    );
}

export default App;
```

---

# 2️⃣9️⃣ Nested Data

Sometimes objects contain arrays.

Example:

```jsx
const company = {
    name: "ABC Technologies",

    employees: [
        "Ramesh",
        "Ravi",
        "Suresh"
    ]
};
```

We can render:

```jsx
<h1>{company.name}</h1>

{company.employees.map((employee) => (
    <p key={employee}>
        {employee}
    </p>
))}
```

---

# 3️⃣0️⃣ Conditional Rendering + Lists

Lists are often combined with conditional rendering.

Example:

```jsx
function App() {

    const products = [];

    return (
        <div>

            <h1>Products</h1>

            {products.length === 0 ? (
                <p>No products found.</p>
            ) : (
                products.map((product) => (
                    <p key={product.id}>
                        {product.name}
                    </p>
                ))
            )}

        </div>
    );
}

export default App;
```

If the array is empty:

```text
No products found.
```

If products exist:

```text
Laptop
Mobile
Keyboard
```

---

# 3️⃣1️⃣ List + API Data 🌐

Later you will get data from an API.

For example:

```text
GET /products
```

Backend may return:

```json
[
    {
        "id": 1,
        "name": "Laptop"
    },
    {
        "id": 2,
        "name": "Mobile"
    }
]
```

React can then do:

```jsx
products.map(...)
```

and create the UI dynamically.

This is one of the most common patterns in real React applications.

---

# 3️⃣2️⃣ Real-World Example

Imagine your project has:

```text
GET /api/customers
```

API response:

```text
Customer 1
Customer 2
Customer 3
...
```

React:

```text
API
 ↓
Array
 ↓
map()
 ↓
CustomerCard
 ↓
UI
```

For your future React + FastAPI projects, this pattern will be very important.

---

# ❌ 3️⃣3️⃣ Common Mistake — No `key`

Wrong:

```jsx
{students.map((student) => (
    <h2>{student}</h2>
))}
```

Better:

```jsx
{students.map((student) => (
    <h2 key={student}>
        {student}
    </h2>
))}
```

Best when IDs exist:

```jsx
{students.map((student) => (
    <h2 key={student.id}>
        {student.name}
    </h2>
))}
```

---

# ❌ 3️⃣4️⃣ Common Mistake — Duplicate Keys

Bad:

```jsx
key="student"
```

for every item.

That means:

```text
student
student
student
student
```

Keys should be unique among the relevant list items.

---

# ❌ 3️⃣5️⃣ Common Mistake — Using Random Values

Avoid:

```jsx
key={Math.random()}
```

Keys should be stable.

A random value changes between renders and can cause unnecessary problems.

---

# ❌ 3️⃣6️⃣ Common Mistake — Using Array Index Automatically

This:

```jsx
key={index}
```

is not always wrong.

But don't automatically choose it when your data already has a stable ID.

Prefer:

```jsx
key={item.id}
```

when available.

---

# ❌ 3️⃣7️⃣ Common Mistake — Forgetting `return`

With curly braces:

```jsx
students.map((student) => {
    <h2>{student}</h2>
})
```

This does not return the JSX.

You need:

```jsx
students.map((student) => {
    return <h2>{student}</h2>;
})
```

Or use parentheses with an implicit return:

```jsx
students.map((student) => (
    <h2>{student}</h2>
))
```

The second style is very common in React.

---

# 3️⃣8️⃣ Common Mistake — Using `forEach()` Instead of `map()`

For rendering a list, use:

```jsx
map()
```

because `map()` returns a new array.

`forEach()` does not return the transformed array in the same way.

Remember:

```text
map()
→ transform / create new array

forEach()
→ perform an action for each item
```

For React rendering, `map()` is usually what you want.

---

# ⭐ 3️⃣9️⃣ Best Practices

### ✅ Use stable unique IDs

```jsx
key={item.id}
```

---

### ✅ Create reusable components

Instead of putting everything inside `App.jsx`:

```text
App
 ↓
ProductList
 ↓
ProductCard
```

---

### ✅ Keep data separate from UI when possible

Example:

```jsx
const products = [...]
```

Then:

```jsx
products.map(...)
```

---

### ✅ Use meaningful names

Good:

```jsx
products
students
employees
orders
```

Avoid:

```jsx
x
data1
abc
```

---

### ✅ Use `map()` for rendering arrays

This is one of the most important React patterns.

---

# 🎤 4️⃣0️⃣ Interview Questions

## Q1. How do you render a list in React?

Use JavaScript's `map()` method.

Example:

```jsx
{students.map((student) => (
    <p key={student.id}>
        {student.name}
    </p>
))}
```

---

## Q2. Why are keys required in React lists?

Keys help React identify individual list items and understand which items have changed, been added, removed, or reordered.

---

## Q3. What is a good key?

A stable, unique identifier from the data.

Example:

```jsx
key={product.id}
```

---

## Q4. Can we use array index as a key?

Yes, in some simple/static cases, but it can cause problems when the list changes order or items are inserted or removed.

A stable ID is generally preferred.

---

## Q5. What is `map()`?

`map()` creates a new array by applying a function to each item of an existing array.

---

## Q6. Can we render components using `map()`?

Yes.

Example:

```jsx
{products.map((product) => (
    <ProductCard
        key={product.id}
        product={product}
    />
))}
```

---

## Q7. Is `key` available inside component props?

No. `key` is a special React attribute.

If your component needs the ID, pass it separately.

---

## Q8. What happens if keys are duplicated?

React can have difficulty correctly tracking the list items, and React will warn about duplicate keys.

---

## Q9. Why is `Math.random()` a bad key?

Because the key changes between renders, so it is not stable.

---

## Q10. What is the difference between `map()` and `forEach()`?

`map()` returns a new array, while `forEach()` is generally used to perform an action for each item without creating a transformed array for you.

---

# 🧪 4️⃣1️⃣ Practice Tasks

### Task 1 — Names

Create:

```jsx
const names = [
    "Ramesh",
    "Ravi",
    "Suresh",
    "Kiran"
];
```

Render them using `map()`.

---

### Task 2 — Cities

Create:

```jsx
const cities = [
    "Hyderabad",
    "Warangal",
    "Vijayawada",
    "Visakhapatnam"
];
```

Display them as `<li>` elements.

---

### Task 3 — Students

Create objects:

```text
id
name
age
city
```

Display them as cards.

---

### Task 4 — Products

Create five products with:

```text
id
name
price
category
```

Render them.

---

### Task 5 — Employee Table

Create an employee table with:

```text
ID
Name
Role
Salary
```

Render rows using `map()`.

---

# 🏆 4️⃣2️⃣ Mini Challenge

Build a **Product List**.

Your data:

```jsx
const products = [
    {
        id: 1,
        name: "Laptop",
        price: 55000,
        category: "Electronics"
    },
    {
        id: 2,
        name: "Mobile",
        price: 25000,
        category: "Electronics"
    },
    {
        id: 3,
        name: "Keyboard",
        price: 1500,
        category: "Accessories"
    },
    {
        id: 4,
        name: "Mouse",
        price: 800,
        category: "Accessories"
    }
];
```

Create a reusable:

```text
ProductCard
```

component.

Expected structure:

```text
App
 │
 └── ProductCard
       ├── Laptop
       ├── Mobile
       ├── Keyboard
       └── Mouse
```

Each card should display:

```text
Product Name
Price
Category
```

And every card must have:

```jsx
key={product.id}
```

---

# 📝 4️⃣3️⃣ 10-Question Quiz

## Q1. Which JavaScript method is commonly used to render arrays in React?

### ✅ Answer

```jsx
map()
```

---

## Q2. What does `map()` return?

### ✅ Answer

A new array containing the result of processing each item.

---

## Q3. Why does React need a `key`?

### ✅ Answer

To identify individual list items.

---

## Q4. Which is usually better?

```jsx
key={index}
```

or:

```jsx
key={student.id}
```

### ✅ Answer

When a stable ID exists:

```jsx
key={student.id}
```

is usually better.

---

## Q5. Should keys be unique?

### ✅ Answer

Yes. Keys should be unique among the relevant sibling list items.

---

## Q6. Is this a good key?

```jsx
key={Math.random()}
```

### ❌ Answer

No.

Keys should be stable between renders.

---

## Q7. Can we render components with `map()`?

### ✅ Answer

Yes.

```jsx
products.map((product) => (
    <ProductCard
        key={product.id}
        product={product}
    />
))
```

---

## Q8. Is `key` normal props?

### ❌ Answer

No.

`key` is a special React attribute.

---

## Q9. What should we use to render a list of objects?

### ✅ Answer

Usually:

```jsx
array.map(...)
```

and access object properties inside the returned JSX.

---

## Q10. Why is `map()` useful in React?

### ✅ Answer

It allows us to transform an array of data into a collection of React elements/components.

---

# 📚 4️⃣4️⃣ Quick Revision

| Concept | Remember |
|---|---|
| 📦 Array | Stores multiple values |
| 🔁 `map()` | Creates output for each item |
| 📋 List | Multiple UI items |
| 🔑 `key` | Identifies list items |
| 🆔 ID | Best common key source |
| 🧩 Component | Reusable UI |
| 📊 Object array | Common API data structure |
| 🚫 Random key | Avoid |
| ⚠️ Index key | Use carefully |
| 🔄 Dynamic UI | Array → `map()` → UI |

---

# 🧠 4️⃣5️⃣ Final Memory Trick

Remember this one pattern:

```text
DATA
 ↓
ARRAY
 ↓
map()
 ↓
KEY
 ↓
COMPONENT
 ↓
UI
```

Example:

```jsx
products.map((product) => (
    <ProductCard
        key={product.id}
        product={product}
    />
))
```

This single pattern is used constantly in real React applications.

---

# 🎯 Chapter 11 Checklist

Before moving ahead, make sure you understand:

- [ ] What is an array?
- [ ] What is a list?
- [ ] Why do we use `map()`?
- [ ] How to render strings?
- [ ] How to render objects?
- [ ] What is a `key`?
- [ ] Why are keys needed?
- [ ] Why are stable IDs preferred?
- [ ] Problems with index keys
- [ ] Problems with random keys
- [ ] Rendering components using `map()`
- [ ] Rendering tables
- [ ] Rendering API-style data
- [ ] Empty-list handling
- [ ] Common list mistakes

---

# 🚀 NEXT CHAPTER

## ⚡ CHAPTER 12 — `useEffect`

In the next chapter, we will learn one of the most important React Hooks:

```text
⚛️ useEffect
   ↓
⏱️ Side Effects
   ↓
🌐 API Calls
   ↓
📦 Fetch Data
   ↓
🔄 Component Lifecycle
   ↓
🧹 Cleanup
```

You will learn:

- What is `useEffect`?
- Why do we need it?
- Basic syntax
- Dependency array
- Empty dependency array
- Effect with dependencies
- Effect without dependency array
- API calls
- Cleanup function
- Common mistakes
- Infinite loops
- Real-world examples

🛑 **Stop here.**

Type **`NEXT`** when you are ready for **Chapter 12 — `useEffect`**.