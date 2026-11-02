# 🚀 CHAPTER 52 — BEGINNER REACT PROJECTS
## PART 2 — Calculator, Digital Clock, Random Quote & Notes App

In Part 1, we built the foundation with:

```text
🔢 Counter App
📝 Todo App
```

Now we continue with:

```text
🧮 Project 3 → Calculator
🕐 Project 4 → Digital Clock
💬 Project 5 → Random Quote App
📝 Project 6 → Notes App
```

---

# 🧮 PROJECT 3 — CALCULATOR APP

## 🎯 What Will We Build?

Our calculator will support:

```text
➕ Addition
➖ Subtraction
✖️ Multiplication
➗ Division
🟰 Result
🗑️ Clear
```

Example:

```text
┌─────────────────┐
│             25  │
├─────────────────┤
│  7 │ 8 │ 9 │ ÷  │
│  4 │ 5 │ 6 │ ×  │
│  1 │ 2 │ 3 │ -  │
│  0 │ C │ = │ +  │
└─────────────────┘
```

---

# 🧠 Step 1 — Think Before Coding

Ask:

### What changes?

The calculator display.

Therefore:

```text
display
```

can be state.

```jsx
const [display, setDisplay] =
    useState("");
```

We also need to know what operation the user selected.

For a simple calculator we can keep:

```text
firstNumber
operator
```

---

# 🧠 Calculator Flow

```text
User clicks number
       ↓
Add number to display
       ↓
User clicks operator
       ↓
Save first number
       ↓
Save operator
       ↓
User enters second number
       ↓
Click =
       ↓
Calculate result
```

---

# 1️⃣ Number Button Logic

Suppose display is:

```text
5
```

User clicks:

```text
2
```

New display:

```text
52
```

Logic:

```jsx
function handleNumber(number) {

    setDisplay(previousDisplay =>
        previousDisplay + number
    );

}
```

---

# 2️⃣ Clear Logic

```jsx
function handleClear() {

    setDisplay("");

}
```

Flow:

```text
52
 ↓
C
 ↓
""
```

---

# 3️⃣ Basic Calculator Example

```jsx
import { useState } from "react";


function Calculator() {

    const [display, setDisplay] =
        useState("");


    function handleNumber(number) {

        setDisplay(previousDisplay =>
            previousDisplay + number
        );

    }


    function handleClear() {

        setDisplay("");

    }


    function calculate() {

        try {

            const result =
                eval(display);

            setDisplay(String(result));

        } catch {

            setDisplay("Error");

        }

    }


    return (
        <div>

            <h1>Calculator</h1>

            <input
                value={display}
                readOnly
            />

            <div>

                <button
                    onClick={() =>
                        handleNumber("7")
                    }
                >
                    7
                </button>

                <button
                    onClick={() =>
                        handleNumber("8")
                    }
                >
                    8
                </button>

                <button
                    onClick={() =>
                        handleNumber("9")
                    }
                >
                    9
                </button>

                <button
                    onClick={() =>
                        handleNumber("+")
                    }
                >
                    +
                </button>

            </div>


            <div>

                <button
                    onClick={() =>
                        handleNumber("4")
                    }
                >
                    4
                </button>

                <button
                    onClick={() =>
                        handleNumber("5")
                    }
                >
                    5
                </button>

                <button
                    onClick={() =>
                        handleNumber("6")
                    }
                >
                    6
                </button>

                <button
                    onClick={() =>
                        handleNumber("-")
                    }
                >
                    -
                </button>

            </div>


            <div>

                <button
                    onClick={() =>
                        handleNumber("1")
                    }
                >
                    1
                </button>

                <button
                    onClick={() =>
                        handleNumber("2")
                    }
                >
                    2
                </button>

                <button
                    onClick={() =>
                        handleNumber("3")
                    }
                >
                    3
                </button>

                <button
                    onClick={() =>
                        handleNumber("*")
                    }
                >
                    ×
                </button>

            </div>


            <div>

                <button
                    onClick={() =>
                        handleNumber("0")
                    }
                >
                    0
                </button>

                <button
                    onClick={handleClear}
                >
                    C
                </button>

                <button
                    onClick={calculate}
                >
                    =
                </button>

                <button
                    onClick={() =>
                        handleNumber("/")
                    }
                >
                    ÷
                </button>

            </div>

        </div>
    );
}


export default Calculator;
```

---

# ⚠️ Important Note About `eval()`

You may see:

```javascript
eval(display)
```

in simple calculator tutorials.

For learning a tiny calculator, it demonstrates the basic idea, but **do not use `eval()` for untrusted/user-controlled expressions in production applications**.

A production calculator should parse and calculate operations explicitly.

For example:

```text
firstNumber
operator
secondNumber
```

and then:

```text
+ → addition
- → subtraction
* → multiplication
/ → division
```

---

# 🧠 Calculator Dry Run

Suppose:

```text
7 + 3
```

User clicks:

```text
7
```

Display:

```text
7
```

User clicks:

```text
+
```

Display:

```text
7+
```

User clicks:

```text
3
```

Display:

```text
7+3
```

User clicks:

```text
=
```

Result:

```text
10
```

---

# 🕐 PROJECT 4 — DIGITAL CLOCK

Now we use an important React concept:

```text
useEffect()
```

---

# 🎯 Requirement

Display the current time:

```text
Current Time

07:35:21 PM
```

The time should update automatically every second.

---

# 🧠 Think Before Coding

What changes?

```text
currentTime
```

Therefore:

```jsx
const [currentTime, setCurrentTime] =
    useState(new Date());
```

What should happen repeatedly?

```text
Every 1 second:
    get current time
    update state
```

What React feature can run this setup?

```text
useEffect()
```

---

# 🔄 Digital Clock Flow

```text
Component renders
       ↓
useEffect()
       ↓
Start interval
       ↓
Every 1 second
       ↓
new Date()
       ↓
setCurrentTime()
       ↓
React re-renders
```

---

# 💻 Digital Clock

```jsx
import {
    useEffect,
    useState
} from "react";


function DigitalClock() {

    const [currentTime, setCurrentTime] =
        useState(new Date());


    useEffect(() => {

        const timer =
            setInterval(() => {

                setCurrentTime(
                    new Date()
                );

            }, 1000);


        return () => {

            clearInterval(timer);

        };

    }, []);


    return (
        <div>

            <h1>
                Digital Clock
            </h1>

            <h2>
                {currentTime.toLocaleTimeString()}
            </h2>

        </div>
    );
}


export default DigitalClock;
```

---

# 🔍 Important Part

```jsx
const timer =
    setInterval(() => {

        setCurrentTime(
            new Date()
        );

    }, 1000);
```

means:

```text
Wait 1000 milliseconds
       ↓
Update time
       ↓
Wait 1000 milliseconds
       ↓
Update time
       ↓
Repeat
```

---

# 🧹 Why `clearInterval()`?

Very important.

We create an interval:

```jsx
setInterval(...)
```

When the component is removed, we should clean it up.

Therefore:

```jsx
return () => {
    clearInterval(timer);
};
```

This is called **cleanup**.

---

# 🧠 Memory Trick

```text
setInterval()
     ↓
Do something repeatedly

clearInterval()
     ↓
Stop it
```

---

# 💬 PROJECT 5 — RANDOM QUOTE APP

Now we introduce API-style thinking.

## 🎯 Requirement

Display:

```text
"The only way to do great work
is to love what you do."

— Steve Jobs

[ New Quote ]
```

When the user clicks:

```text
New Quote
```

another quote appears.

---

# 🧠 Step 1 — Identify State

We need:

```text
quote
author
```

For example:

```jsx
const [quote, setQuote] =
    useState(null);
```

---

# 🧠 Step 2 — Data

For learning, start with local data.

```jsx
const quotes = [
    {
        text: "Stay hungry, stay foolish.",
        author: "Steve Jobs"
    },
    {
        text: "The future depends on what you do today.",
        author: "Mahatma Gandhi"
    },
    {
        text: "Success is not final.",
        author: "Winston Churchill"
    }
];
```

---

# 🎲 Random Number

We can select a random quote using:

```javascript
Math.random()
```

and:

```javascript
Math.floor()
```

Example:

```javascript
const index =
    Math.floor(
        Math.random() * quotes.length
    );
```

If there are 3 quotes:

```text
Math.random()
      ↓
0 to less than 1
      ↓
× 3
      ↓
0 to less than 3
      ↓
Math.floor()
      ↓
0, 1, or 2
```

---

# 💻 Random Quote App

```jsx
import { useState } from "react";


const quotes = [
    {
        text:
            "Stay hungry, stay foolish.",
        author: "Steve Jobs"
    },
    {
        text:
            "The future depends on what you do today.",
        author: "Mahatma Gandhi"
    },
    {
        text:
            "Success is not final.",
        author: "Winston Churchill"
    }
];


function RandomQuote() {

    const [quote, setQuote] =
        useState(quotes[0]);


    function generateQuote() {

        const randomIndex =
            Math.floor(
                Math.random() *
                quotes.length
            );


        setQuote(
            quotes[randomIndex]
        );

    }


    return (
        <div>

            <h1>
                Random Quote
            </h1>

            <blockquote>
                "{quote.text}"
            </blockquote>

            <p>
                — {quote.author}
            </p>

            <button
                onClick={generateQuote}
            >
                New Quote
            </button>

        </div>
    );
}


export default RandomQuote;
```

---

# 🔍 Dry Run

Suppose:

```text
quotes.length = 3
```

Random index:

```text
2
```

Then:

```text
quotes[2]
```

might be:

```text
Success is not final.
```

State changes:

```text
quote = quotes[2]
```

React re-renders.

---

# 🌐 API Version — Concept

Later, we can fetch quotes from an API.

The flow becomes:

```text
Button
  ↓
fetch()
  ↓
API
  ↓
JSON response
  ↓
setQuote()
  ↓
UI
```

This connects directly to the API integration concepts you learned earlier.

---

# 📝 PROJECT 6 — NOTES APP

Now we build our final beginner project.

This is a useful project because it combines:

```text
useState
forms
arrays
objects
map()
filter()
events
components
```

---

# 🎯 Requirements

Our Notes App:

```text
Notes App

Title
[ Learn React            ]

Content
[ React is a JavaScript
  library...             ]

[ Add Note ]

--------------------------

Learn React
React is a JavaScript library...

[ Delete ]
```

---

# 🧠 Identify State

We need:

```text
title
content
notes
```

Therefore:

```jsx
const [title, setTitle] =
    useState("");

const [content, setContent] =
    useState("");

const [notes, setNotes] =
    useState([]);
```

---

# 🧠 Note Object

Each note can look like:

```javascript
{
    id: 1,
    title: "Learn React",
    content: "Study components"
}
```

---

# 🔄 Add Note Flow

```text
User enters title
       ↓
title state

User enters content
       ↓
content state

Click Add Note
       ↓
Validate
       ↓
Create note object
       ↓
Add to notes
       ↓
Clear form
```

---

# 💻 Notes App

```jsx
import { useState } from "react";


function NotesApp() {

    const [title, setTitle] =
        useState("");


    const [content, setContent] =
        useState("");


    const [notes, setNotes] =
        useState([]);


    function addNote() {

        if (
            title.trim() === "" ||
            content.trim() === ""
        ) {
            return;
        }


        const newNote = {

            id: Date.now(),

            title: title,

            content: content

        };


        setNotes(previousNotes => [
            ...previousNotes,
            newNote
        ]);


        setTitle("");

        setContent("");

    }


    function deleteNote(noteId) {

        setNotes(previousNotes =>
            previousNotes.filter(
                note =>
                    note.id !== noteId
            )
        );

    }


    return (
        <div>

            <h1>
                Notes App
            </h1>


            <input
                type="text"
                value={title}
                onChange={event =>
                    setTitle(
                        event.target.value
                    )
                }
                placeholder="Title"
            />


            <textarea
                value={content}
                onChange={event =>
                    setContent(
                        event.target.value
                    )
                }
                placeholder="Write your note..."
            />


            <button
                onClick={addNote}
            >
                Add Note
            </button>


            <div>

                {notes.map(note => (

                    <div
                        key={note.id}
                    >

                        <h2>
                            {note.title}
                        </h2>

                        <p>
                            {note.content}
                        </p>

                        <button
                            onClick={() =>
                                deleteNote(
                                    note.id
                                )
                            }
                        >
                            Delete
                        </button>

                    </div>

                ))}

            </div>

        </div>
    );
}


export default NotesApp;
```

---

# 🔍 Notes App Dry Run

Initial:

```text
title = ""
content = ""
notes = []
```

User enters:

```text
Title:
Learn React
```

and:

```text
Content:
Learn components and props.
```

State:

```text
title = "Learn React"

content =
"Learn components and props."
```

Click:

```text
Add Note
```

New object:

```javascript
{
    id: 123,
    title: "Learn React",
    content: "Learn components and props."
}
```

Then:

```text
notes = [
    {
        id: 123,
        title: "Learn React",
        content: "Learn components and props."
    }
]
```

The form is cleared.

---

# 🗑️ Delete Note

Suppose:

```text
notes:

1 → Learn React
2 → Learn JavaScript
3 → Learn SQL
```

Delete ID:

```text
2
```

Use:

```jsx
setNotes(previousNotes =>
    previousNotes.filter(
        note => note.id !== 2
    )
);
```

Result:

```text
1 → Learn React
3 → Learn SQL
```

---

# 🧠 What Did We Learn From These 6 Projects?

## 🔢 Counter

```text
useState
events
functional updates
```

## 📝 Todo

```text
arrays
objects
map
filter
state updates
```

## 🧮 Calculator

```text
event handling
conditional logic
calculations
```

## 🕐 Digital Clock

```text
useEffect
setInterval
cleanup
```

## 💬 Random Quote

```text
random values
state
data selection
API concept
```

## 📝 Notes

```text
forms
arrays
objects
CRUD-style operations
```

---

# 🧠 PROJECT SKILL MAP

```text
                 REACT BEGINNER PROJECTS

                         │
          ┌──────────────┼──────────────┐
          ↓              ↓              ↓
       STATE          EVENTS          JSX
          │              │              │
          └──────────────┼──────────────┘
                         ↓
                      PROJECTS
                         │
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
    Counter            Todo            Calculator
       │                 │                 │
       ↓                 ↓                 ↓
   useState        map/filter        calculations
                         │
       ┌─────────────────┼─────────────────┐
       ↓                 ↓                 ↓
  Digital Clock    Random Quote        Notes
       │                 │                 │
       ↓                 ↓                 ↓
   useEffect          random            forms
   interval            data             arrays
```

---

# 🚨 COMMON BEGINNER MISTAKES

## 1. Mutating arrays

❌ Don't:

```javascript
notes.push(newNote);
```

✅ Do:

```javascript
setNotes(previous => [
    ...previous,
    newNote
]);
```

---

## 2. Missing keys

❌

```jsx
notes.map(note => (
    <div>
        {note.title}
    </div>
))
```

✅

```jsx
notes.map(note => (
    <div key={note.id}>
        {note.title}
    </div>
))
```

---

## 3. Calling functions during render

❌

```jsx
onClick={deleteNote(note.id)}
```

✅

```jsx
onClick={() =>
    deleteNote(note.id)
}
```

---

## 4. Forgetting interval cleanup

❌

```jsx
useEffect(() => {

    setInterval(...);

}, []);
```

Better:

```jsx
useEffect(() => {

    const timer =
        setInterval(...);

    return () => {
        clearInterval(timer);
    };

}, []);
```

---

# 🏆 BEGINNER PROJECT CHALLENGE

Now try building these **without copying the solution**.

## Challenge 1 — Counter

Requirements:

```text
Count
+
-
Reset
+5
-5
Even/Odd
Minimum = 0
Maximum = 100
```

---

## Challenge 2 — Todo

Requirements:

```text
Add
Delete
Complete
Total count
Completed count
Pending count
```

---

## Challenge 3 — Calculator

Requirements:

```text
0–9
+
-
*
/
=
C
```

---

## Challenge 4 — Digital Clock

Requirements:

```text
Current time
Updates every second
Cleanup interval
```

---

## Challenge 5 — Random Quote

Requirements:

```text
Quote
Author
New Quote button
Random selection
```

---

## Challenge 6 — Notes

Requirements:

```text
Add note
Delete note
Title
Content
Empty validation
```

---

# 🎤 INTERVIEW QUESTIONS

### Q1. Why do we use `useEffect` in a digital clock?

**Answer:**  
To perform the interval setup and clean it up when the component unmounts.

---

### Q2. Why should an interval be cleared?

**Answer:**  
To prevent the interval from continuing after the component is no longer needed and to avoid unnecessary work or memory/resource leaks.

---

### Q3. How do you add an item to an array in React state?

**Answer:**

```jsx
setItems(previousItems => [
    ...previousItems,
    newItem
]);
```

---

### Q4. How do you delete an item?

**Answer:**

```jsx
setItems(previousItems =>
    previousItems.filter(
        item => item.id !== id
    )
);
```

---

### Q5. How do you update one item in an array?

**Answer:**

Use `map()` and replace the matching item.

---

### Q6. What React concepts are used in a Notes App?

**Answer:**

```text
useState
forms
events
arrays
objects
map()
filter()
```

---

# 📝 10-QUESTION QUIZ

### Q1. Which Hook is commonly used for Counter?

**Answer:** `useState`

### Q2. Which method renders a Todo list?

**Answer:** `map()`

### Q3. Which method removes matching items?

**Answer:** `filter()`

### Q4. Which Hook is commonly used for a timer?

**Answer:** `useEffect`

### Q5. What JavaScript function creates a repeating timer?

**Answer:** `setInterval()`

### Q6. How do you stop an interval?

**Answer:** `clearInterval()`

### Q7. Which function can generate a random decimal?

**Answer:** `Math.random()`

### Q8. Why do we use `Date.now()` in these examples?

**Answer:**  
It provides a convenient numeric value that can be used as an identifier in a simple learning project.

### Q9. Why should React state arrays not be mutated directly?

**Answer:**  
Because React state should be updated using immutable update patterns through the state setter.

### Q10. What are the six beginner projects?

**Answer:**

```text
1. Counter
2. Todo
3. Calculator
4. Digital Clock
5. Random Quote
6. Notes App
```

---

# 🧠 FINAL MEMORY TRICK

```text
🔢 Counter
   ↓
STATE

📝 Todo
   ↓
ARRAYS

🧮 Calculator
   ↓
LOGIC

🕐 Clock
   ↓
EFFECT

💬 Quote
   ↓
DATA / RANDOM

📝 Notes
   ↓
FORMS + CRUD
```

And the three most important array patterns:

```jsx
// ADD
setItems(previous => [
    ...previous,
    newItem
]);

// DELETE
setItems(previous =>
    previous.filter(
        item => item.id !== id
    )
);

// UPDATE
setItems(previous =>
    previous.map(item =>
        item.id === id
            ? { ...item, completed: !item.completed }
            : item
    )
);
```

---

# ✅ CHAPTER 52 COMPLETE

You have now covered the complete **Beginner Projects** section:

```text
✅ Counter App
✅ Todo App
✅ Calculator
✅ Digital Clock
✅ Random Quote App
✅ Notes App
```

The next level is where the projects become much closer to **portfolio/job-ready applications**.

# 🚀 NEXT CHAPTER — CHAPTER 53

## 🟡 INTERMEDIATE REACT PROJECTS

We will build projects involving:

```text
🌤️ Weather App
🛒 E-commerce App
📰 Blog App
📊 Dashboard
👤 User Management
🔐 Authentication
🔄 API Integration
🧩 Reusable Components
📱 Responsive UI
```

Type **NEXT** to start **Chapter 53 — Intermediate Projects**.