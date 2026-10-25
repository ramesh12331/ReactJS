# 📝 REACTJS — CHAPTER 10
# 📝 FORMS

---

# 🎯 Chapter Goal

In this chapter, you will learn how React handles user input.

We will start from very simple examples and slowly build a complete form.

You will learn:

```text
📝 Form
   ↓
⌨️ Input
   ↓
⚛️ State
   ↓
🔄 onChange
   ↓
📨 Submit
   ↓
✅ Validation
   ↓
🔄 Reset
```

---

# 1️⃣ What Is a Form?

A **form** is a group of input fields used to collect information from a user.

For example, a registration form:

```text
┌─────────────────────────────┐
│       Registration          │
│                             │
│ Name:  [____________]       │
│ Email: [____________]       │
│ Age:   [____________]       │
│                             │
│ Gender:                     │
│ ○ Male  ○ Female            │
│                             │
│ City: [ Hyderabad ▼ ]       │
│                             │
│ ☑ I agree                   │
│                             │
│        [ Register ]          │
└─────────────────────────────┘
```

---

# 2️⃣ Real-World Examples 🌍

You use forms everywhere.

### 🔐 Login

```text
Email
Password
[ Login ]
```

### 👤 Registration

```text
Name
Email
Password
Phone
[ Register ]
```

### 🛒 Product Form

```text
Product Name
Price
Category
Quantity
[ Add Product ]
```

### 📞 Contact Form

```text
Name
Email
Message
[ Send ]
```

### 🔎 Search

```text
Search Product
[____________]
```

So learning React forms is extremely important.

---

# 3️⃣ Normal HTML Form

Before React, you may have written:

```html
<form>
    <input type="text">
    <button>Submit</button>
</form>
```

React uses the same HTML elements:

```jsx
<form>
    <input />
    <button>Submit</button>
</form>
```

But React adds something very important:

```text
React State
```

---

# 4️⃣ What Is a Controlled Component? ⭐

This is the first major concept of this chapter.

A **controlled component** is an input whose value is controlled by React state.

Simple flow:

```text
User types
    ↓
Input event
    ↓
React state changes
    ↓
Input displays updated state
```

So:

```text
Input
  ↕
React State
```

---

# 5️⃣ Why Do We Need Controlled Components?

Suppose the user types:

```text
Ramesh
```

React should know:

```text
name = "Ramesh"
```

Then React can use that value for:

- validation
- API requests
- displaying data
- submitting forms
- clearing forms
- editing records

This is why form state is important.

---

# 6️⃣ Basic Text Input

Let's start with a simple input.

```jsx
function App() {

    return (
        <div>

            <input type="text" />

        </div>
    );
}

export default App;
```

This creates:

```text
[________________]
```

But React does not yet store what the user types.

Let's connect it to state.

---

# 7️⃣ Text Input + State

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    return (
        <div>

            <input
                type="text"
                value={name}
                onChange={(event) => setName(event.target.value)}
            />

            <h2>Name: {name}</h2>

        </div>
    );
}

export default App;
```

---

# 8️⃣ What Is Happening Here? 🧠

We have:

```jsx
const [name, setName] = useState("");
```

Initially:

```text
name = ""
```

Then:

```jsx
value={name}
```

means:

> The input value comes from React state.

And:

```jsx
onChange={(event) => setName(event.target.value)}
```

means:

> When the user types, update the state.

---

# 9️⃣ Important Flow ⭐⭐⭐

Remember this:

```text
User types "R"
       ↓
onChange
       ↓
event.target.value
       ↓
setName("R")
       ↓
name = "R"
       ↓
React renders
       ↓
Input shows "R"
```

Then user types:

```text
a
```

Flow:

```text
"R"
 ↓
"Ra"
```

Then:

```text
"Ram"
```

Then:

```text
"Rame"
```

And so on.

---

# 🔟 What Is `event.target.value`?

This is very important.

Suppose the user types:

```text
Ramesh
```

The event contains information about the input.

```jsx
event.target
```

means:

> The input element that caused the event.

And:

```jsx
event.target.value
```

means:

> The current value inside that input.

So:

```jsx
setName(event.target.value)
```

stores the typed value in state.

---

# 1️⃣1️⃣ Dry Run

Initial:

```text
name = ""
```

Input:

```text
[________]
```

User types:

```text
R
```

Then:

```text
event.target.value
        ↓
       "R"
        ↓
setName("R")
        ↓
name = "R"
```

User types:

```text
a
```

Now:

```text
event.target.value
        ↓
       "Ra"
        ↓
setName("Ra")
```

State:

```text
name = "Ra"
```

---

# 1️⃣2️⃣ Showing the Value

We can display the state:

```jsx
<h2>Name: {name}</h2>
```

If user types:

```text
Ramesh
```

UI becomes:

```text
[ Ramesh ]

Name: Ramesh
```

This is the power of state.

---

# 1️⃣3️⃣ Controlled Component Flow

The complete concept:

```text
        USER
          │
          │ types
          ↓
       INPUT
          │
          │ onChange
          ↓
   event.target.value
          │
          ↓
      setName()
          │
          ↓
       STATE
          │
          ↓
     RE-RENDER
          │
          ↓
       INPUT
```

This cycle happens continuously while the user types.

---

# 1️⃣4️⃣ Text Input Example

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    return (
        <div>

            <h1>Student Form</h1>

            <label>Name:</label>

            <input
                type="text"
                value={name}
                onChange={(event) => setName(event.target.value)}
            />

            <p>You entered: {name}</p>

        </div>
    );
}

export default App;
```

---

# 1️⃣5️⃣ Number Input 🔢

We can create:

```jsx
<input type="number" />
```

Example:

```jsx
import { useState } from "react";

function App() {

    const [age, setAge] = useState("");

    return (
        <div>

            <label>Age:</label>

            <input
                type="number"
                value={age}
                onChange={(event) => setAge(event.target.value)}
            />

            <p>Age: {age}</p>

        </div>
    );
}

export default App;
```

---

# ⚠️ Important: Number Input Value

Even though we use:

```jsx
type="number"
```

the value obtained from:

```jsx
event.target.value
```

is commonly handled as a string.

For example:

```text
"25"
```

not necessarily:

```text
25
```

If you need a number for calculations, you can convert it.

Example:

```jsx
const numberAge = Number(event.target.value);
```

For now, understand the basic form flow first.

---

# 1️⃣6️⃣ Checkbox ☑️

A checkbox is different.

Example:

```text
☐ I agree to the terms
```

We usually use:

```jsx
checked
```

instead of `value` for the checked state.

Example:

```jsx
import { useState } from "react";

function App() {

    const [agree, setAgree] = useState(false);

    return (
        <div>

            <label>

                <input
                    type="checkbox"
                    checked={agree}
                    onChange={(event) =>
                        setAgree(event.target.checked)
                    }
                />

                I agree to the terms

            </label>

            <p>
                Agreement: {agree ? "Yes" : "No"}
            </p>

        </div>
    );
}

export default App;
```

---

# 1️⃣7️⃣ `event.target.checked`

For a checkbox:

```jsx
event.target.checked
```

tells us whether it is:

```text
true
```

or:

```text
false
```

Example:

```text
☐
 ↓
checked = false
```

Click:

```text
☑
 ↓
checked = true
```

---

# 1️⃣8️⃣ Checkbox Flow

```text
☐
 ↓ click
☑
 ↓
event.target.checked
 ↓
true
 ↓
setAgree(true)
```

Click again:

```text
☑
 ↓ click
☐
 ↓
event.target.checked
 ↓
false
```

---

# 1️⃣9️⃣ Radio Buttons 🔘

Radio buttons are useful when the user should choose **one option**.

Example:

```text
Gender:

○ Male

○ Female
```

React example:

```jsx
import { useState } from "react";

function App() {

    const [gender, setGender] = useState("");

    return (
        <div>

            <h2>Gender</h2>

            <label>
                <input
                    type="radio"
                    name="gender"
                    value="Male"
                    checked={gender === "Male"}
                    onChange={(event) =>
                        setGender(event.target.value)
                    }
                />

                Male
            </label>

            <label>
                <input
                    type="radio"
                    name="gender"
                    value="Female"
                    checked={gender === "Female"}
                    onChange={(event) =>
                        setGender(event.target.value)
                    }
                />

                Female
            </label>

            <p>Selected: {gender}</p>

        </div>
    );
}

export default App;
```

---

# 2️⃣0️⃣ Radio Button Flow

Suppose the user clicks:

```text
○ Male
```

Then:

```text
event.target.value
       ↓
"Male"
       ↓
setGender("Male")
       ↓
gender = "Male"
```

Now:

```text
Selected: Male
```

---

# 2️⃣1️⃣ Why `name="gender"`?

Radio buttons with the same `name` belong to the same group.

Example:

```jsx
name="gender"
```

Both buttons use:

```text
gender
```

Therefore the user chooses one option from the group.

---

# 2️⃣2️⃣ Select Dropdown 🔽

A select input allows the user to choose from a list.

Example:

```text
City:

[ Hyderabad ▼ ]
```

React:

```jsx
import { useState } from "react";

function App() {

    const [city, setCity] = useState("");

    return (
        <div>

            <label>City:</label>

            <select
                value={city}
                onChange={(event) =>
                    setCity(event.target.value)
                }
            >

                <option value="">
                    Select City
                </option>

                <option value="Hyderabad">
                    Hyderabad
                </option>

                <option value="Warangal">
                    Warangal
                </option>

                <option value="Vijayawada">
                    Vijayawada
                </option>

            </select>

            <p>Selected City: {city}</p>

        </div>
    );
}

export default App;
```

---

# 2️⃣3️⃣ Select Flow

User selects:

```text
Hyderabad
```

Then:

```text
event.target.value
       ↓
"Hyderabad"
       ↓
setCity("Hyderabad")
       ↓
city = "Hyderabad"
```

UI:

```text
Selected City: Hyderabad
```

---

# 2️⃣4️⃣ Textarea 📝

Textarea is used for larger text.

Examples:

```text
Message
┌──────────────────────────┐
│                          │
│                          │
│                          │
└──────────────────────────┘
```

React:

```jsx
import { useState } from "react";

function App() {

    const [message, setMessage] = useState("");

    return (
        <div>

            <textarea
                value={message}
                onChange={(event) =>
                    setMessage(event.target.value)
                }
            />

            <p>{message}</p>

        </div>
    );
}

export default App;
```

---

# 2️⃣5️⃣ Multiple Inputs ⭐

A real form usually has many fields.

For example:

```text
Name
Email
Age
City
Password
```

We could create separate state variables:

```jsx
const [name, setName] = useState("");
const [email, setEmail] = useState("");
const [age, setAge] = useState("");
const [city, setCity] = useState("");
```

This is valid.

But a form with many fields can become difficult to manage.

Another approach is to keep them inside one object.

---

# 2️⃣6️⃣ Multiple Inputs With Object State

Example:

```jsx
import { useState } from "react";

function App() {

    const [formData, setFormData] = useState({
        name: "",
        email: "",
        age: "",
        city: ""
    });

    return (
        <div>

            <input
                type="text"
                placeholder="Name"
                value={formData.name}
                onChange={(event) =>
                    setFormData({
                        ...formData,
                        name: event.target.value
                    })
                }
            />

            <input
                type="email"
                placeholder="Email"
                value={formData.email}
                onChange={(event) =>
                    setFormData({
                        ...formData,
                        email: event.target.value
                    })
                }
            />

        </div>
    );
}

export default App;
```

---

# 2️⃣7️⃣ Why `...formData`?

Suppose current state is:

```text
name  = Ramesh
email = ramesh@gmail.com
age   = 25
city  = Hyderabad
```

We want to change only:

```text
name
```

We use:

```jsx
setFormData({
    ...formData,
    name: event.target.value
});
```

Meaning:

```text
Copy existing form data
        +
Change name
```

The other values stay unchanged.

---

# 2️⃣8️⃣ Better Pattern — One Handler

When many inputs have the same pattern, we can use one handler.

```jsx
import { useState } from "react";

function App() {

    const [formData, setFormData] = useState({
        name: "",
        email: "",
        age: ""
    });

    function handleChange(event) {

        const { name, value } = event.target;

        setFormData({
            ...formData,
            [name]: value
        });
    }

    return (
        <div>

            <input
                name="name"
                value={formData.name}
                onChange={handleChange}
                placeholder="Name"
            />

            <input
                name="email"
                value={formData.email}
                onChange={handleChange}
                placeholder="Email"
            />

            <input
                name="age"
                value={formData.age}
                onChange={handleChange}
                placeholder="Age"
            />

        </div>
    );
}

export default App;
```

---

# 2️⃣9️⃣ Understand `[name]: value` 🧠

This line may look confusing:

```jsx
[name]: value
```

Suppose:

```text
name = "email"
value = "ramesh@gmail.com"
```

Then:

```jsx
[name]: value
```

becomes conceptually:

```jsx
email: "ramesh@gmail.com"
```

If:

```text
name = "age"
value = "25"
```

then:

```jsx
age: "25"
```

This allows one handler to manage multiple fields.

---

# 3️⃣0️⃣ Form Submission

Now we need a submit button.

Basic HTML:

```jsx
<form>
    <input />
    <button type="submit">
        Submit
    </button>
</form>
```

In React, we can handle submission using:

```jsx
onSubmit
```

---

# 3️⃣1️⃣ Basic `onSubmit`

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        console.log(name);

    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="text"
                value={name}
                onChange={(event) =>
                    setName(event.target.value)
                }
            />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}

export default App;
```

---

# 3️⃣2️⃣ What Is `event.preventDefault()`?

Normally, submitting an HTML form can cause the browser to perform its default form submission behavior.

In a React application, we usually want to handle the submission ourselves.

So:

```jsx
event.preventDefault();
```

means:

> Stop the browser's default form submission behavior.

Then React can process the form.

---

# 3️⃣3️⃣ Form Submission Flow ⭐

```text
User enters data
       ↓
Input state updated
       ↓
User clicks Submit
       ↓
onSubmit runs
       ↓
preventDefault()
       ↓
Validate data
       ↓
Send data / process data
```

Later, when we learn APIs:

```text
Form
 ↓
Validation
 ↓
POST API
 ↓
Backend
 ↓
Database
```

---

# 3️⃣4️⃣ Complete Simple Form

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");
    const [email, setEmail] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        console.log("Name:", name);
        console.log("Email:", email);

    }

    return (
        <div>

            <h1>Student Form</h1>

            <form onSubmit={handleSubmit}>

                <div>
                    <label>Name:</label>

                    <input
                        type="text"
                        value={name}
                        onChange={(event) =>
                            setName(event.target.value)
                        }
                    />
                </div>

                <br />

                <div>
                    <label>Email:</label>

                    <input
                        type="email"
                        value={email}
                        onChange={(event) =>
                            setEmail(event.target.value)
                        }
                    />
                </div>

                <br />

                <button type="submit">
                    Submit
                </button>

            </form>

        </div>
    );
}

export default App;
```

---

# 3️⃣5️⃣ Form Dry Run

User enters:

```text
Name:
Ramesh
```

State:

```text
name = "Ramesh"
```

User enters:

```text
Email:
ramesh@gmail.com
```

State:

```text
email = "ramesh@gmail.com"
```

User clicks:

```text
Submit
```

Then:

```text
onSubmit
   ↓
preventDefault()
   ↓
handleSubmit()
   ↓
Read state
   ↓
Name: Ramesh
Email: ramesh@gmail.com
```

---

# 3️⃣6️⃣ Form Validation ✅

Validation means checking whether the user's input is correct.

For example:

```text
Name is required
Email is required
Age must be valid
Password is required
```

---

# 3️⃣7️⃣ Simple Validation

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        if (name === "") {
            alert("Name is required");
            return;
        }

        alert("Form submitted");

    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="text"
                value={name}
                onChange={(event) =>
                    setName(event.target.value)
                }
            />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}

export default App;
```

---

# 3️⃣8️⃣ Validation Flow

```text
Submit
  ↓
Is name empty?
  ↓
YES ──→ ❌ Show error
  │
  NO
  ↓
Continue
  ↓
✅ Submit
```

---

# 3️⃣9️⃣ Better Validation Message

Instead of only using `alert`, we can keep an error in state.

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");
    const [error, setError] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        if (name.trim() === "") {
            setError("Name is required");
            return;
        }

        setError("");

        console.log("Form submitted");

    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                type="text"
                value={name}
                onChange={(event) =>
                    setName(event.target.value)
                }
            />

            {error && (
                <p>{error}</p>
            )}

            <button type="submit">
                Submit
            </button>

        </form>
    );
}

export default App;
```

---

# 4️⃣0️⃣ Why `trim()`?

Suppose the user enters:

```text
"     "
```

Technically, it contains spaces.

But we usually don't want spaces to count as a valid name.

```jsx
name.trim()
```

removes spaces from the beginning and end for the purpose of checking.

So:

```text
"     "
 ↓ trim()
""
```

Therefore:

```jsx
name.trim() === ""
```

is true.

---

# 4️⃣1️⃣ Reset Form 🔄

After submitting, we may want to clear the form.

For example:

```text
Before:

Name: Ramesh
Email: ramesh@gmail.com

After Reset:

Name:
Email:
```

---

# 4️⃣2️⃣ Reset With State

If using separate states:

```jsx
setName("");
setEmail("");
```

Example:

```jsx
function resetForm() {

    setName("");
    setEmail("");

}
```

---

# 4️⃣3️⃣ Complete Form With Reset

```jsx
import { useState } from "react";

function App() {

    const [name, setName] = useState("");
    const [email, setEmail] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        console.log({
            name,
            email
        });

        resetForm();
    }

    function resetForm() {

        setName("");
        setEmail("");

    }

    return (
        <div>

            <h1>Student Form</h1>

            <form onSubmit={handleSubmit}>

                <input
                    type="text"
                    placeholder="Name"
                    value={name}
                    onChange={(event) =>
                        setName(event.target.value)
                    }
                />

                <input
                    type="email"
                    placeholder="Email"
                    value={email}
                    onChange={(event) =>
                        setEmail(event.target.value)
                    }
                />

                <button type="submit">
                    Submit
                </button>

                <button
                    type="button"
                    onClick={resetForm}
                >
                    Reset
                </button>

            </form>

        </div>
    );
}

export default App;
```

---

# ⚠️ 4️⃣4️⃣ Important: Submit vs Button

Inside a form:

```jsx
<button type="submit">
    Submit
</button>
```

causes form submission.

But:

```jsx
<button type="button">
    Reset
</button>
```

is a normal button.

This is important.

For a reset button handled by your own function, use:

```jsx
type="button"
```

---

# 4️⃣5️⃣ Common Mistake ❌

## Mistake 1 — Forgetting `value`

Controlled input:

```jsx
<input
    value={name}
    onChange={...}
/>
```

The `value` connects the input to state.

---

# 4️⃣6️⃣ Common Mistake ❌

## Mistake 2 — Forgetting `onChange`

This:

```jsx
<input value={name} />
```

without a proper way to update the value can make the input effectively read-only.

For a controlled input, normally use:

```jsx
value={name}
onChange={...}
```

---

# 4️⃣7️⃣ Common Mistake ❌

## Mistake 3 — Forgetting `preventDefault()`

If you want React to handle the form submission:

```jsx
function handleSubmit(event) {

    event.preventDefault();

}
```

Remember it.

---

# 4️⃣8️⃣ Common Mistake ❌

## Mistake 4 — Checkbox Using `value`

For checkbox state, you normally care about:

```jsx
event.target.checked
```

not just:

```jsx
event.target.value
```

Remember:

```text
Text input
→ value

Checkbox
→ checked
```

---

# 4️⃣9️⃣ Common Mistake ❌

## Mistake 5 — Forgetting `name`

When using the multiple-input pattern:

```jsx
const { name, value } = event.target;
```

the input should have a matching `name`:

```jsx
<input name="email" />
```

Otherwise the handler won't know which field should be updated.

---

# 5️⃣0️⃣ Common Mistake ❌

## Mistake 6 — Forgetting Spread Operator

Wrong:

```jsx
setFormData({
    name: event.target.value
});
```

This replaces the object with only `name`.

Better:

```jsx
setFormData({
    ...formData,
    name: event.target.value
});
```

This preserves the other fields.

---

# ⭐ 5️⃣1️⃣ Best Practices

### ✅ 1. Use controlled inputs

Keep important form values in React state.

---

### ✅ 2. Use meaningful state names

Good:

```jsx
const [email, setEmail] = useState("");
```

Avoid unclear names like:

```jsx
const [x, setX] = useState("");
```

---

### ✅ 3. Validate before submitting

Think:

```text
Input
 ↓
Validation
 ↓
Submit
```

---

### ✅ 4. Show useful error messages

Instead of:

```text
Error
```

prefer:

```text
Email is required
```

---

### ✅ 5. Reset after successful submission when appropriate

For example:

```text
Submit successful
       ↓
Clear form
```

---

# 5️⃣2️⃣ Real-World Form Architecture 🏗️

A professional form often follows:

```text
                FORM
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
      INPUTS             BUTTONS
        │                   │
        ↓                   ↓
      STATE              SUBMIT
        │                   │
        └─────────┬─────────┘
                  ↓
              VALIDATION
                  ↓
             API REQUEST
                  ↓
             BACKEND
                  ↓
              DATABASE
```

Later in this React course, you will connect these forms to APIs.

---

# 5️⃣3️⃣ React Form + FastAPI 🔥

Because you are also learning FastAPI, eventually your flow will look like:

```text
React Form
    ↓
User enters data
    ↓
React State
    ↓
Validation
    ↓
POST request
    ↓
FastAPI
    ↓
Pydantic validation
    ↓
PostgreSQL
```

For example:

```text
React
Name: Ramesh
Age: 25
City: Hyderabad
        ↓
      API
        ↓
FastAPI
        ↓
PostgreSQL
```

We will learn API integration later in the React course.

---

# 🧠 5️⃣4️⃣ Most Important Form Pattern

For a text input, remember:

```jsx
<input
    value={value}
    onChange={(event) =>
        setValue(event.target.value)
    }
/>
```

This pattern will appear again and again in React applications.

---

# 🎤 5️⃣5️⃣ Interview Questions

## Q1. What is a controlled component?

### Answer

A controlled component is a form input whose value is controlled by React state.

---

## Q2. How do you handle a text input?

### Answer

Use state with `value` and update the state using `onChange`.

```jsx
value={name}

onChange={(event) =>
    setName(event.target.value)
}
```

---

## Q3. What is `event.target.value`?

### Answer

It gives the current value of the input that triggered the event.

---

## Q4. How do you handle a checkbox?

### Answer

Use `checked` and read:

```jsx
event.target.checked
```

---

## Q5. What is `onSubmit`?

### Answer

`onSubmit` is an event handler used to handle form submission.

---

## Q6. Why do we use `event.preventDefault()`?

### Answer

It prevents the browser's default form submission behavior so React can handle the submission itself.

---

## Q7. How do you reset a form?

### Answer

Set the state values back to their initial values.

Example:

```jsx
setName("");
setEmail("");
```

---

## Q8. How do you manage multiple inputs?

### Answer

You can use separate state variables or keep the form fields in one object state.

---

## Q9. Why do we use the spread operator with object form state?

### Answer

To preserve existing fields while updating one field.

```jsx
setFormData({
    ...formData,
    name: event.target.value
});
```

---

## Q10. What is form validation?

### Answer

Form validation checks whether user input satisfies the required rules before the form is submitted.

---

# 🧪 5️⃣6️⃣ Practice Questions

## Practice 1 — Name

Create:

```text
Name: [____________]
```

Display:

```text
Hello Ramesh
```

when the user types Ramesh.

---

## Practice 2 — Age

Create:

```text
Age: [____________]
```

Display:

```text
Your age is 25
```

---

## Practice 3 — Checkbox

Create:

```text
☐ I agree
```

Display:

```text
Agreed
```

when checked.

Otherwise:

```text
Not Agreed
```

---

## Practice 4 — Gender

Create:

```text
○ Male
○ Female
```

Display the selected gender.

---

## Practice 5 — City

Create:

```text
[ Select City ▼ ]
```

Options:

```text
Hyderabad
Warangal
Vijayawada
```

Display the selected city.

---

## Practice 6 — Message

Create a textarea and display the message below it.

---

## Practice 7 — Registration Form

Create:

```text
Name
Email
Age
City
Gender
☐ Terms

[ Register ]
[ Reset ]
```

Requirements:

```text
Name required
Email required
Terms required
```

---

# 🏆 5️⃣7️⃣ Mini Challenge

Build a **Student Registration Form**.

Your form should contain:

```text
┌──────────────────────────────┐
│    Student Registration      │
│                              │
│ Name:  [____________]        │
│ Email: [____________]        │
│ Age:   [____________]        │
│                              │
│ Gender:                      │
│ ○ Male   ○ Female            │
│                              │
│ City: [ Hyderabad ▼ ]        │
│                              │
│ ☑ I agree                    │
│                              │
│ [ Register ] [ Reset ]       │
└──────────────────────────────┘
```

### Validation rules

```text
Name     → Required
Email    → Required
Age      → Required
Gender   → Required
City     → Required
Terms    → Must be checked
```

### Think Before Coding 🧠

Ask:

```text
1. What fields do I need?
        ↓
2. What should be state?
        ↓
3. What happens onChange?
        ↓
4. What happens onSubmit?
        ↓
5. What validation is required?
        ↓
6. What happens after successful submission?
        ↓
7. How will I reset the form?
```

This thinking pattern will help you build real CRUD forms later.

---

# 📝 5️⃣8️⃣ 10-Question Quiz

## Q1. What is a controlled component?

### ✅ Answer

A form input whose value is controlled by React state.

---

## Q2. Which event is commonly used for text input changes?

### ✅ Answer

```jsx
onChange
```

---

## Q3. How do you get the value of a text input?

### ✅ Answer

```jsx
event.target.value
```

---

## Q4. How do you get the checked state of a checkbox?

### ✅ Answer

```jsx
event.target.checked
```

---

## Q5. Which event handles form submission?

### ✅ Answer

```jsx
onSubmit
```

---

## Q6. Why do we use `preventDefault()`?

### ✅ Answer

To prevent the browser's default form submission behavior.

---

## Q7. What should be used for a checkbox: `value` or `checked`?

### ✅ Answer

For its checked state, use:

```jsx
checked
```

and:

```jsx
event.target.checked
```

---

## Q8. Why use `...formData`?

### ✅ Answer

To keep the existing fields while updating one field.

---

## Q9. What is form validation?

### ✅ Answer

Checking user input before accepting or submitting it.

---

## Q10. How can you reset controlled inputs?

### ✅ Answer

Set their state back to their initial values.

Example:

```jsx
setName("");
setEmail("");
```

---

# 📚 5️⃣9️⃣ Quick Revision

| Concept | Remember |
|---|---|
| 📝 Form | Collects user information |
| ⚛️ Controlled component | Input controlled by state |
| ⌨️ Text input | `value` + `onChange` |
| 🔢 Number input | `type="number"` |
| ☑️ Checkbox | `checked` |
| 🔘 Radio | `value` + `checked` |
| 🔽 Select | `value` + `onChange` |
| 📝 Textarea | `value` + `onChange` |
| 📨 Submit | `onSubmit` |
| 🛑 Prevent default | `event.preventDefault()` |
| ✅ Validation | Check input |
| 🔄 Reset | Restore initial state |

---

# 🧠 6️⃣0️⃣ Final Memory Trick

For a **text input**, remember:

```text
USER TYPES
    ↓
onChange
    ↓
event.target.value
    ↓
setState()
    ↓
STATE
    ↓
UI
```

For a **form**:

```text
INPUT
 ↓
STATE
 ↓
VALIDATION
 ↓
SUBMIT
 ↓
API
 ↓
DATABASE
```

And remember:

```text
Text Input
→ value

Checkbox
→ checked

Form
→ onSubmit

Submit
→ preventDefault()
```

---

# ✅ Chapter 10 Checklist

Before moving forward, make sure you understand:

- [ ] What is a form?
- [ ] Controlled components
- [ ] Input state
- [ ] Text input
- [ ] Number input
- [ ] Checkbox
- [ ] Radio buttons
- [ ] Select
- [ ] Textarea
- [ ] Multiple inputs
- [ ] Object form state
- [ ] `event.target.value`
- [ ] `event.target.checked`
- [ ] `onSubmit`
- [ ] `preventDefault()`
- [ ] Form validation
- [ ] Reset forms
- [ ] Common form mistakes

---

# 🚀 NEXT CHAPTER

## 📋 CHAPTER 11 — LISTS AND KEYS

We will learn:

```text
📋 Arrays
   ↓
🔁 map()
   ↓
🧩 Dynamic Components
   ↓
🔑 Keys
   ↓
❓ Why Keys Are Required
   ↓
✅ Correct Key Usage
   ↓
❌ Common Key Mistakes
   ↓
📦 Cards
   ↓
📊 Tables
   ↓
📋 Lists
```

These are the topics specified for Chapter 11 in your course source.

🛑 **STOP HERE.**

Type **`NEXT`** when you are ready for **Chapter 11 — Lists and Keys**.