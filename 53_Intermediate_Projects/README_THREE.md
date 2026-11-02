# 💰 CHAPTER 53 — INTERMEDIATE PROJECTS
# PART 3 — EXPENSE TRACKER

We have already built:

```text
✅ Weather App
✅ Movie Search App
```

Now we will build:

# 💰 EXPENSE TRACKER

This project is very important because it combines **forms + arrays + state + calculations + filtering + sorting + local storage**.

---

# 🎯 1. WHAT ARE WE BUILDING?

Our Expense Tracker will allow the user to:

```text
➕ Add expense
📝 Enter description
💰 Enter amount
🏷️ Select category
📅 Select date
📋 Display expenses
🗑️ Delete expense
🔎 Filter expenses
↕️ Sort expenses
💵 Calculate total
💾 Save data in Local Storage
```

Example:

```text
┌──────────────────────────────────────┐
│          💰 Expense Tracker          │
├──────────────────────────────────────┤
│                                      │
│ Description: [ Grocery          ]    │
│ Amount:      [ 1200             ]    │
│ Category:    [ Food        ▼    ]    │
│ Date:        [ 2026-11-02      ]    │
│                                      │
│              [ ➕ Add Expense ]       │
│                                      │
├──────────────────────────────────────┤
│ Total: ₹4,850                        │
├──────────────────────────────────────┤
│ Grocery       Food       ₹1200 🗑️    │
│ Petrol        Travel     ₹1500 🗑️    │
│ Movie         Entertainment ₹500 🗑️ │
└──────────────────────────────────────┘
```

---

# 🧠 2. WHY IS THIS AN INTERMEDIATE PROJECT?

The application is not just displaying information.

The user can **change data**.

The flow becomes:

```text
User
 ↓
Form
 ↓
React State
 ↓
Array
 ↓
Calculate
 ↓
Filter / Sort
 ↓
Display
 ↓
Local Storage
```

This is very close to real-world React development.

---

# 🧩 3. WHAT REACT CONCEPTS WILL WE USE?

```text
useState
Forms
Events
Controlled inputs
Arrays
map()
filter()
sort()
reduce()
Conditional rendering
Props
Callbacks
Component communication
Local Storage
Derived state
```

---

# 🧠 4. THINK ABOUT THE DATA FIRST

Before writing components, decide what one expense looks like.

Example:

```javascript
const expense = {
    id: 1,
    description: "Grocery",
    amount: 1200,
    category: "Food",
    date: "2026-11-02"
};
```

One expense is an object.

Multiple expenses are an array:

```javascript
const expenses = [
    {
        id: 1,
        description: "Grocery",
        amount: 1200,
        category: "Food",
        date: "2026-11-02"
    },
    {
        id: 2,
        description: "Petrol",
        amount: 1500,
        category: "Travel",
        date: "2026-11-01"
    }
];
```

---

# 🧠 5. DATA STRUCTURE

Think:

```text
Expenses
   │
   ├── Expense 1
   │      ├── id
   │      ├── description
   │      ├── amount
   │      ├── category
   │      └── date
   │
   ├── Expense 2
   │      ├── id
   │      ├── description
   │      ├── amount
   │      ├── category
   │      └── date
   │
   └── Expense 3
```

This structure will make our application easier to understand.

---

# 📁 6. PROJECT STRUCTURE

A clean structure:

```text
expense-tracker/
│
├── src/
│   │
│   ├── components/
│   │   ├── ExpenseForm.jsx
│   │   ├── ExpenseList.jsx
│   │   ├── ExpenseItem.jsx
│   │   ├── ExpenseSummary.jsx
│   │   └── ExpenseFilter.jsx
│   │
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
│
├── package.json
└── vite.config.js
```

---

# 🏠 7. APP ARCHITECTURE

```text
App
│
├── ExpenseForm
│
├── ExpenseSummary
│
├── ExpenseFilter
│
└── ExpenseList
      │
      ├── ExpenseItem
      ├── ExpenseItem
      └── ExpenseItem
```

---

# 🧠 8. WHERE SHOULD THE MAIN STATE LIVE?

We need multiple components to access the expenses.

Therefore, keep the main state in `App`.

```jsx
const [expenses, setExpenses] =
    useState([]);
```

Why?

Because:

```text
ExpenseForm
     ↓
needs to ADD

ExpenseList
     ↓
needs to DISPLAY

ExpenseSummary
     ↓
needs to CALCULATE

ExpenseFilter
     ↓
needs to FILTER
```

So `App` becomes the common source of truth.

---

# 🔄 9. STATE FLOW

```text
                  App
                   │
              expenses
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
 ExpenseForm   Summary      ExpenseList
       │
       │ addExpense()
       ↓
      App
```

This is another example of **lifting state up**.

---

# 📝 10. EXPENSE FORM STATE

The form needs:

```jsx
const [description, setDescription] =
    useState("");

const [amount, setAmount] =
    useState("");

const [category, setCategory] =
    useState("Food");

const [date, setDate] =
    useState("");
```

Each input is controlled by React.

---

# 🎯 11. CONTROLLED INPUT

Example:

```jsx
<input
    value={description}
    onChange={event =>
        setDescription(
            event.target.value
        )
    }
/>
```

The flow:

```text
User types
    ↓
onChange
    ↓
setDescription()
    ↓
React state
    ↓
input value
```

---

# 🧩 12. EXPENSE FORM COMPONENT

Create:

```text
src/components/ExpenseForm.jsx
```

```jsx
import { useState } from "react";


function ExpenseForm({
    onAddExpense
}) {

    const [description, setDescription] =
        useState("");

    const [amount, setAmount] =
        useState("");

    const [category, setCategory] =
        useState("Food");

    const [date, setDate] =
        useState("");


    function handleSubmit(event) {

        event.preventDefault();


        if (
            description.trim() === "" ||
            amount === "" ||
            date === ""
        ) {
            return;
        }


        const newExpense = {

            id: Date.now(),

            description:
                description.trim(),

            amount:
                Number(amount),

            category,

            date

        };


        onAddExpense(
            newExpense
        );


        setDescription("");

        setAmount("");

        setCategory("Food");

        setDate("");

    }


    return (

        <form
            onSubmit={handleSubmit}
        >

            <input
                type="text"
                placeholder="Description"
                value={description}
                onChange={event =>
                    setDescription(
                        event.target.value
                    )
                }
            />


            <input
                type="number"
                placeholder="Amount"
                value={amount}
                onChange={event =>
                    setAmount(
                        event.target.value
                    )
                }
            />


            <select
                value={category}
                onChange={event =>
                    setCategory(
                        event.target.value
                    )
                }
            >

                <option value="Food">
                    Food
                </option>

                <option value="Travel">
                    Travel
                </option>

                <option value="Shopping">
                    Shopping
                </option>

                <option value="Bills">
                    Bills
                </option>

                <option value="Entertainment">
                    Entertainment
                </option>

            </select>


            <input
                type="date"
                value={date}
                onChange={event =>
                    setDate(
                        event.target.value
                    )
                }
            />


            <button type="submit">
                ➕ Add Expense
            </button>

        </form>
    );
}


export default ExpenseForm;
```

---

# 🔍 13. DRY RUN — ADD EXPENSE

Suppose user enters:

```text
Description = Grocery
Amount      = 1200
Category    = Food
Date        = 2026-11-02
```

When the user clicks:

```text
➕ Add Expense
```

we create:

```javascript
const newExpense = {
    id: Date.now(),
    description: "Grocery",
    amount: 1200,
    category: "Food",
    date: "2026-11-02"
};
```

Then:

```text
onAddExpense(newExpense)
```

sends the object to the parent.

---

# 📤 14. CHILD → PARENT

This is the same concept from Chapter 19.

```text
ExpenseForm
     │
     │ onAddExpense()
     ↓
    App
```

The parent gives the child a function.

The child calls the function.

---

# 🏠 15. APP COMPONENT

Now:

```jsx
import { useState } from "react";

import ExpenseForm
    from "./components/ExpenseForm";


function App() {

    const [expenses, setExpenses] =
        useState([]);


    function handleAddExpense(
        newExpense
    ) {

        setExpenses(
            previousExpenses => [
                ...previousExpenses,
                newExpense
            ]
        );

    }


    return (

        <div>

            <h1>
                💰 Expense Tracker
            </h1>


            <ExpenseForm
                onAddExpense={
                    handleAddExpense
                }
            />

        </div>
    );
}


export default App;
```

---

# 🧠 16. WHY USE SPREAD OPERATOR?

We have:

```javascript
previousExpenses
```

Suppose:

```javascript
[
    expense1,
    expense2
]
```

We want:

```text
expense1
expense2
expense3
```

So:

```javascript
[
    ...previousExpenses,
    newExpense
]
```

means:

```text
copy old expenses
+
add new expense
```

---

# ⚠️ 17. DON'T MUTATE STATE

Bad:

```javascript
expenses.push(newExpense);

setExpenses(expenses);
```

Avoid directly changing the existing state array.

Better:

```javascript
setExpenses(
    previousExpenses => [
        ...previousExpenses,
        newExpense
    ]
);
```

This creates a new array.

---

# 📋 18. EXPENSE LIST

Create:

```text
src/components/ExpenseList.jsx
```

```jsx
import ExpenseItem
    from "./ExpenseItem";


function ExpenseList({
    expenses,
    onDeleteExpense
}) {

    if (expenses.length === 0) {

        return (
            <p>
                📭 No expenses found.
            </p>
        );

    }


    return (

        <div>

            {expenses.map(expense => (

                <ExpenseItem
                    key={expense.id}
                    expense={expense}
                    onDeleteExpense={
                        onDeleteExpense
                    }
                />

            ))}

        </div>
    );
}


export default ExpenseList;
```

---

# 🧩 19. EXPENSE ITEM

Create:

```text
src/components/ExpenseItem.jsx
```

```jsx
function ExpenseItem({
    expense,
    onDeleteExpense
}) {

    return (

        <div>

            <h3>
                {expense.description}
            </h3>

            <p>
                💰 ₹{expense.amount}
            </p>

            <p>
                🏷️ {expense.category}
            </p>

            <p>
                📅 {expense.date}
            </p>

            <button
                onClick={() =>
                    onDeleteExpense(
                        expense.id
                    )
                }
            >
                🗑️ Delete
            </button>

        </div>
    );
}


export default ExpenseItem;
```

---

# 🗑️ 20. DELETE EXPENSE

In `App.jsx`:

```javascript
function handleDeleteExpense(
    expenseId
) {

    setExpenses(
        previousExpenses =>
            previousExpenses.filter(
                expense =>
                    expense.id !== expenseId
            )
    );

}
```

---

# 🧠 21. UNDERSTAND `filter()`

Suppose:

```text
expenses:

1 Grocery
2 Petrol
3 Movie
```

We want to delete:

```text
2 Petrol
```

Condition:

```javascript
expense.id !== 2
```

Result:

```text
1 Grocery
3 Movie
```

So:

```javascript
filter()
```

creates a new array without the matching expense.

---

# 🔄 22. DELETE FLOW

```text
User clicks Delete
        ↓
ExpenseItem
        ↓
onDeleteExpense(id)
        ↓
App
        ↓
filter()
        ↓
setExpenses()
        ↓
React re-renders
```

---

# 💵 23. CALCULATING TOTAL EXPENSE

Now comes an important JavaScript method:

```text
reduce()
```

Suppose:

```javascript
const expenses = [
    { amount: 1200 },
    { amount: 1500 },
    { amount: 500 }
];
```

We want:

```text
1200 + 1500 + 500
```

Use:

```javascript
const total = expenses.reduce(
    (sum, expense) =>
        sum + expense.amount,
    0
);
```

Result:

```text
3200
```

---

# 🧠 24. HOW `reduce()` WORKS

Start:

```text
sum = 0
```

First:

```text
0 + 1200 = 1200
```

Second:

```text
1200 + 1500 = 2700
```

Third:

```text
2700 + 500 = 3200
```

Final:

```text
3200
```

---

# 📊 25. EXPENSE SUMMARY

Create:

```text
src/components/ExpenseSummary.jsx
```

```jsx
function ExpenseSummary({
    expenses
}) {

    const total = expenses.reduce(
        (sum, expense) =>
            sum + expense.amount,
        0
    );


    return (

        <div>

            <h2>
                Total Expenses
            </h2>

            <p>
                💰 ₹{total}
            </p>

        </div>
    );
}


export default ExpenseSummary;
```

---

# 🔎 26. FILTERING EXPENSES

Suppose we have:

```text
Food
Travel
Bills
Shopping
```

The user selects:

```text
Food
```

We only display Food expenses.

We can use:

```javascript
const filteredExpenses =
    expenses.filter(
        expense =>
            expense.category ===
            selectedCategory
    );
```

---

# 🧠 27. FILTER STATE

In `App`:

```jsx
const [selectedCategory, setSelectedCategory] =
    useState("All");
```

Now:

```text
All
Food
Travel
Shopping
Bills
Entertainment
```

---

# 🔎 28. FILTER LOGIC

```javascript
const filteredExpenses =
    selectedCategory === "All"
        ? expenses
        : expenses.filter(
            expense =>
                expense.category ===
                selectedCategory
        );
```

This means:

```text
If All
 ↓
show everything

Otherwise
 ↓
show matching category
```

---

# 🧩 29. FILTER COMPONENT

```jsx
function ExpenseFilter({
    selectedCategory,
    onCategoryChange
}) {

    return (

        <select
            value={selectedCategory}
            onChange={event =>
                onCategoryChange(
                    event.target.value
                )
            }
        >

            <option value="All">
                All Categories
            </option>

            <option value="Food">
                Food
            </option>

            <option value="Travel">
                Travel
            </option>

            <option value="Shopping">
                Shopping
            </option>

            <option value="Bills">
                Bills
            </option>

            <option value="Entertainment">
                Entertainment
            </option>

        </select>
    );
}


export default ExpenseFilter;
```

---

# ↕️ 30. SORTING EXPENSES

We may want:

```text
Highest amount
Lowest amount
Newest
Oldest
```

Example:

```javascript
const sortedExpenses =
    [...filteredExpenses].sort(
        (a, b) =>
            b.amount - a.amount
    );
```

This sorts from:

```text
High → Low
```

---

# ⚠️ 31. IMPORTANT — DON'T DIRECTLY SORT STATE

Avoid:

```javascript
expenses.sort(...)
```

because `sort()` mutates the array.

Better:

```javascript
[...expenses].sort(...)
```

The spread operator creates a copy first.

---

# 🧠 32. DERIVED DATA

This is an important concept.

We don't necessarily need state for:

```text
total
filteredExpenses
sortedExpenses
```

because they can be calculated from:

```text
expenses
```

For example:

```javascript
const total = expenses.reduce(...);
```

This is called **derived data**.

---

# ⭐ 33. IMPORTANT RULE

Don't store duplicate information unnecessarily.

Bad:

```text
expenses
+
totalExpenses
```

when `totalExpenses` can be calculated from `expenses`.

Better:

```text
expenses
   ↓
calculate total
```

This reduces the chance of inconsistent state.

---

# 💾 34. LOCAL STORAGE

If we refresh the browser:

```text
expenses
   ↓
React memory
   ↓
Refresh
   ↓
Gone ❌
```

We want:

```text
expenses
   ↓
localStorage
   ↓
Refresh
   ↓
Restore data
```

---

# 🧠 35. WHAT IS LOCAL STORAGE?

`localStorage` is browser storage that allows us to save data as strings.

Example:

```javascript
localStorage.setItem(
    "name",
    "Ramesh"
);
```

Read:

```javascript
const name =
    localStorage.getItem("name");
```

Remove:

```javascript
localStorage.removeItem("name");
```

---

# 📦 36. SAVING AN ARRAY

JavaScript array:

```javascript
[
    {
        description: "Grocery",
        amount: 1200
    }
]
```

But localStorage stores strings.

Therefore:

```javascript
JSON.stringify(expenses)
```

converts the array to a string.

---

# 💾 37. SAVE EXPENSES

We can use `useEffect`:

```jsx
import {
    useEffect,
    useState
} from "react";
```

Then:

```jsx
useEffect(() => {

    localStorage.setItem(
        "expenses",
        JSON.stringify(expenses)
    );

}, [expenses]);
```

Meaning:

```text
expenses changes
       ↓
useEffect runs
       ↓
JSON.stringify()
       ↓
localStorage
```

---

# 📥 38. LOAD EXPENSES

When the application starts:

```javascript
const savedExpenses =
    localStorage.getItem(
        "expenses"
    );
```

Then:

```javascript
const initialExpenses =
    savedExpenses
        ? JSON.parse(savedExpenses)
        : [];
```

Now the saved string becomes a JavaScript array again.

---

# 🔄 39. LOCAL STORAGE FLOW

```text
React State
    ↓
JSON.stringify()
    ↓
localStorage
```

When application starts:

```text
localStorage
    ↓
JSON.parse()
    ↓
React State
```

Remember:

```text
stringify → save
parse → read
```

---

# 🧠 40. COMPLETE APP FLOW

```text
                    App
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
    ExpenseForm   Summary    Filter
          │                     │
          │                     │
          ↓                     ↓
      addExpense           selectedCategory
          │                     │
          └──────────┬──────────┘
                     ↓
                 expenses
                     │
              ┌──────┴──────┐
              ↓             ↓
           filter          total
              ↓
            sort
              ↓
        ExpenseList
              ↓
        ExpenseItem
```

---

# 🧪 41. COMPLETE DRY RUN

Start:

```text
expenses = []
```

User adds:

```text
Grocery
₹1200
Food
```

State:

```text
expenses = [
    {
        id: 1,
        description: "Grocery",
        amount: 1200,
        category: "Food"
    }
]
```

User adds:

```text
Petrol
₹1500
Travel
```

Now:

```text
expenses = [
    Grocery ₹1200,
    Petrol  ₹1500
]
```

Total:

```text
1200 + 1500
= 2700
```

UI:

```text
Total: ₹2700
```

User selects:

```text
Food
```

Filter:

```text
Grocery ₹1200
```

User deletes Grocery.

Now:

```text
expenses = [
    Petrol ₹1500
]
```

Total:

```text
₹1500
```

---

# 🧠 42. IMPORTANT JAVASCRIPT METHODS

This project gives us excellent practice with:

### `map()`

Used for displaying items.

```javascript
expenses.map(...)
```

### `filter()`

Used for filtering/deleting.

```javascript
expenses.filter(...)
```

### `reduce()`

Used for calculations.

```javascript
expenses.reduce(...)
```

### `sort()`

Used for ordering.

```javascript
[...expenses].sort(...)
```

Remember:

```text
map    → display
filter → select/remove
reduce → calculate
sort   → order
```

---

# 🚨 43. COMMON MISTAKES

## Mistake 1 — Mutating state

Bad:

```javascript
expenses.push(newExpense);
```

Better:

```javascript
setExpenses(
    previous => [
        ...previous,
        newExpense
    ]
);
```

---

## Mistake 2 — Mutating with sort

Bad:

```javascript
expenses.sort(...)
```

Better:

```javascript
[...expenses].sort(...)
```

---

## Mistake 3 — Forgetting `Number()`

Input values are strings.

For example:

```text
"1200"
```

not:

```text
1200
```

So:

```javascript
amount: Number(amount)
```

is useful when we want numeric calculations.

---

## Mistake 4 — Forgetting `JSON.stringify()`

Bad:

```javascript
localStorage.setItem(
    "expenses",
    expenses
);
```

Better:

```javascript
localStorage.setItem(
    "expenses",
    JSON.stringify(expenses)
);
```

---

## Mistake 5 — Forgetting `JSON.parse()`

Bad:

```javascript
const expenses =
    localStorage.getItem(
        "expenses"
    );
```

This gives a string.

Better:

```javascript
const expenses =
    JSON.parse(
        localStorage.getItem(
            "expenses"
        )
    );
```

Use a fallback when the key doesn't exist.

---

# 💼 44. INTERVIEW QUESTIONS

### Q1. Why should we not mutate React state directly?

**Answer:**  
React state should be updated using the state setter with a new value/reference so React can properly detect the update.

---

### Q2. Why do we use `reduce()` in an Expense Tracker?

**Answer:**  
To calculate values such as the total amount from an array of expenses.

---

### Q3. Why use `filter()`?

**Answer:**  
To create a new array containing only items that satisfy a condition. It can be used for filtering and deleting items.

---

### Q4. Why use `[...expenses].sort()`?

**Answer:**  
Because `sort()` mutates the array. Copying it first avoids directly mutating the original state array.

---

### Q5. Why use `JSON.stringify()` with localStorage?

**Answer:**  
Because localStorage stores string values, so JavaScript objects and arrays must be converted to strings before saving.

---

### Q6. Why use `JSON.parse()`?

**Answer:**  
To convert stored JSON strings back into JavaScript objects or arrays.

---

### Q7. What is derived data?

**Answer:**  
Data that can be calculated from existing state instead of being stored separately.

Example:

```text
expenses
   ↓
reduce()
   ↓
total
```

---

# 🧪 45. MINI CHALLENGE

Build the Expense Tracker yourself.

### Basic requirements

```text
☐ Add expense
☐ Description
☐ Amount
☐ Category
☐ Date
☐ Display expenses
☐ Delete expense
☐ Calculate total
```

### Intermediate requirements

```text
☐ Filter by category
☐ Sort by amount
☐ Sort by date
☐ Local Storage
☐ Empty state
☐ Form validation
```

### Bonus

Add:

```text
📊 Category totals
📈 Monthly spending
💰 Highest expense
💵 Lowest expense
🔍 Search expenses
```

---

# 📝 10-QUESTION QUIZ

### Q1. Which Hook stores the expenses?

**Answer:** `useState`

### Q2. Which array method is useful for displaying every expense?

**Answer:** `map()`

### Q3. Which method is useful for deleting an expense based on its ID?

**Answer:** `filter()`

### Q4. Which method calculates the total?

**Answer:** `reduce()`

### Q5. Which method sorts an array?

**Answer:** `sort()`

### Q6. Why use `[...expenses]` before sorting?

**Answer:** To create a copy and avoid mutating the original state array.

### Q7. What converts an array/object into a JSON string?

**Answer:** `JSON.stringify()`

### Q8. What converts JSON text back into a JavaScript value?

**Answer:** `JSON.parse()`

### Q9. Where should the main `expenses` state live when several components need it?

**Answer:** In their common parent, `App`.

### Q10. What are the four important array methods we practiced?

**Answer:**

```text
map()
filter()
reduce()
sort()
```

---

# 🧠 FINAL MEMORY TRICK

Remember the Expense Tracker like this:

```text
➕ ADD
 ↓
expenses[]
 ↓
 ┌───────────────┐
 │               │
 ↓               ↓
FILTER          TOTAL
 │               │
 ↓               ↓
SORT          reduce()
 │
 ↓
LIST
 │
 ↓
DELETE
```

And:

```text
map()
   ↓
Display

filter()
   ↓
Filter / Delete

reduce()
   ↓
Calculate

sort()
   ↓
Order
```

For persistence:

```text
React State
    ↓
JSON.stringify()
    ↓
localStorage

localStorage
    ↓
JSON.parse()
    ↓
React State
```

---

# ✅ CHAPTER 53 — PART 3 COMPLETE

We have completed:

```text
🌤️ Part 1 — Weather App
🎬 Part 2 — Movie Search App
💰 Part 3 — Expense Tracker
```

You have now practiced an important real-world pattern:

```text
FORM
 ↓
STATE
 ↓
ARRAY
 ↓
CRUD
 ↓
FILTER
 ↓
SORT
 ↓
CALCULATE
 ↓
LOCAL STORAGE
```

# 🚀 NEXT

## CHAPTER 53 — PART 4
### 🧠 QUIZ APP

We will build a real Quiz App with:

```text
❓ Questions
🔘 Answer options
✅ Correct answer
❌ Wrong answer
📊 Score
⏭️ Next question
🔄 Restart
📈 Result screen
```

Type **NEXT** to continue.