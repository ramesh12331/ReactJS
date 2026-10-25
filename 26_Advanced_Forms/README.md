# 📝 CHAPTER 26 — ADVANCED FORMS

## 🎯 Goal of This Chapter

You already learned basic React forms.

Now we will learn how to build **real-world forms**.

For example:

```text
Registration Form
        ↓
Name
Email
Password
Confirm Password
        ↓
Validation
        ↓
Error Messages
        ↓
Submit
        ↓
API
```

This is very important for real React applications.

---

# 📚 CHAPTER TOPICS

We will learn:

1. What is form architecture?
2. Form state
3. Controlled forms
4. Uncontrolled forms
5. Validation
6. Client-side validation
7. Required fields
8. Email validation
9. Password validation
10. Error messages
11. Form submission
12. Form libraries
13. React Hook Form
14. Controlled vs uncontrolled
15. Practical registration form
16. Common mistakes
17. Best practices
18. Interview questions
19. Practice tasks
20. Mini challenge
21. Quiz
22. Summary

---

# 1️⃣ WHAT IS A FORM?

A form collects information from the user.

Examples:

```text
Login Form
Register Form
Contact Form
Search Form
Product Form
Employee Form
Checkout Form
```

Example:

```text
┌──────────────────────────────┐
│       Registration           │
│                              │
│ Name:     [____________]     │
│ Email:    [____________]     │
│ Password: [____________]     │
│                              │
│          [ Register ]         │
└──────────────────────────────┘
```

---

# 2️⃣ WHAT IS FORM ARCHITECTURE?

Form architecture means:

> How we organize form fields, state, validation, errors, and submission.

Think:

```text
                 FORM
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
     Fields    Validation   Submit
       │          │          │
       ↓          ↓          ↓
     State      Errors       API
```

A good form normally has:

```text
Input
 ↓
State
 ↓
Validation
 ↓
Error message
 ↓
Submit
 ↓
API
```

---

# 3️⃣ SIMPLE REACT FORM

Let's start with a basic form.

```jsx
import { useState } from "react";

function Register() {

    const [name, setName] = useState("");

    function handleSubmit(event) {

        event.preventDefault();

        console.log(name);
    }

    return (
        <form onSubmit={handleSubmit}>

            <label>Name</label>

            <input
                type="text"
                value={name}
                onChange={(event) =>
                    setName(event.target.value)
                }
            />

            <button type="submit">
                Register
            </button>

        </form>
    );
}

export default Register;
```

---

# 4️⃣ UNDERSTANDING THE FLOW 🧠

When the user types:

```text
R
```

React receives:

```text
event.target.value
```

Then:

```text
setName("R")
```

State becomes:

```text
name = "R"
```

Then user types:

```text
a
```

State becomes:

```text
name = "Ra"
```

Flow:

```text
User types
    ↓
onChange
    ↓
event.target.value
    ↓
setName()
    ↓
State changes
    ↓
Input updates
```

---

# 5️⃣ WHAT IS VALIDATION?

Validation means:

> Checking whether the user's input is correct before accepting or submitting it.

For example:

```text
Name
❌ Empty

Email
❌ Invalid

Password
❌ Too short
```

We can show:

```text
Name is required
Please enter a valid email
Password must contain at least 8 characters
```

---

# 6️⃣ WHY DO WE NEED VALIDATION?

Suppose a registration form accepts:

```text
Name: ""
Email: "hello"
Password: "123"
```

This is bad data.

Validation protects the application from incorrect input.

```text
User Input
    ↓
Validation
    ↓
Valid?
 ┌──┴──┐
YES    NO
 ↓      ↓
Submit  Error
```

---

# 7️⃣ CLIENT-SIDE VALIDATION

Client-side validation happens in the browser.

Example:

```text
React Application
       ↓
User enters data
       ↓
Browser / React validation
       ↓
Error or Submit
```

Example:

```jsx
if (name.trim() === "") {

    setError("Name is required");

}
```

---

# 8️⃣ CLIENT-SIDE vs SERVER-SIDE VALIDATION

## Client-side

Runs in the frontend.

```text
React
 ↓
Validation
```

Useful for:

- Better user experience
- Immediate feedback
- Avoiding unnecessary requests

---

## Server-side

Runs on the backend.

```text
React
 ↓
API
 ↓
Backend validation
```

Important because users can bypass frontend validation.

### 🧠 Important

> Frontend validation improves UX. Backend validation protects the application.

You normally want both.

---

# 9️⃣ REQUIRED FIELDS

A required field must contain a value.

Example:

```jsx
<input
    type="text"
    required
/>
```

The browser can prevent submission when the field is empty.

But in a real React application, you often also manage validation yourself so you can control the error messages and behavior.

---

# 🔟 MANUAL REQUIRED VALIDATION

Example:

```jsx
if (name.trim() === "") {

    setError("Name is required");

    return;
}
```

Why use:

```javascript
trim()
```

Because:

```text
"Ramesh"
```

contains meaningful text.

But:

```text
"     "
```

contains only spaces.

`trim()` removes leading and trailing whitespace.

---

# 1️⃣1️⃣ MULTIPLE FIELD VALIDATION

Suppose:

```text
Name
Email
Password
```

We can create an error object.

```jsx
const [errors, setErrors] = useState({});
```

Example:

```jsx
setErrors({
    name: "Name is required",
    email: "Email is required",
    password: "Password is required"
});
```

Now each field can have its own error.

---

# 1️⃣2️⃣ COMPLETE BASIC VALIDATION

```jsx
import { useState } from "react";

function Register() {

    const [formData, setFormData] = useState({
        name: "",
        email: "",
        password: ""
    });

    const [errors, setErrors] = useState({});

    function handleChange(event) {

        const { name, value } = event.target;

        setFormData({
            ...formData,
            [name]: value
        });
    }

    function handleSubmit(event) {

        event.preventDefault();

        const newErrors = {};

        if (!formData.name.trim()) {
            newErrors.name =
                "Name is required";
        }

        if (!formData.email.trim()) {
            newErrors.email =
                "Email is required";
        }

        if (!formData.password.trim()) {
            newErrors.password =
                "Password is required";
        }

        setErrors(newErrors);

        if (Object.keys(newErrors).length === 0) {

            console.log(
                "Form submitted",
                formData
            );
        }
    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                name="name"
                value={formData.name}
                onChange={handleChange}
                placeholder="Name"
            />

            {errors.name && (
                <p>{errors.name}</p>
            )}

            <input
                name="email"
                value={formData.email}
                onChange={handleChange}
                placeholder="Email"
            />

            {errors.email && (
                <p>{errors.email}</p>
            )}

            <input
                name="password"
                type="password"
                value={formData.password}
                onChange={handleChange}
                placeholder="Password"
            />

            {errors.password && (
                <p>{errors.password}</p>
            )}

            <button type="submit">
                Register
            </button>

        </form>
    );
}

export default Register;
```

---

# 1️⃣3️⃣ IMPORTANT PATTERN — ONE OBJECT FOR FORM DATA

Instead of:

```jsx
const [name, setName] = useState("");
const [email, setEmail] = useState("");
const [password, setPassword] = useState("");
```

we can use:

```jsx
const [formData, setFormData] = useState({
    name: "",
    email: "",
    password: ""
});
```

This becomes easier when the form has many fields.

---

# 1️⃣4️⃣ UNDERSTAND `handleChange`

```jsx
function handleChange(event) {

    const { name, value } = event.target;

    setFormData({
        ...formData,
        [name]: value
    });
}
```

Suppose:

```text
name = "email"
value = "ramesh@gmail.com"
```

Then:

```javascript
[name]: value
```

becomes:

```javascript
email: "ramesh@gmail.com"
```

So:

```text
formData
```

becomes:

```javascript
{
    name: "",
    email: "ramesh@gmail.com",
    password: ""
}
```

---

# 1️⃣5️⃣ WHY DO WE USE `[name]`?

This is a **computed property name**.

Example:

```javascript
const name = "email";

const value = "ramesh@gmail.com";

const data = {
    [name]: value
};
```

Result:

```javascript
{
    email: "ramesh@gmail.com"
}
```

So one `handleChange()` can manage multiple fields.

---

# 1️⃣6️⃣ EMAIL VALIDATION 📧

A simple validation can check whether the email has a reasonable format.

Example:

```javascript
function isValidEmail(email) {

    return /^[^\s@]+@[^\s@]+\.[^\s@]+$/
        .test(email);
}
```

Then:

```javascript
if (!isValidEmail(formData.email)) {

    newErrors.email =
        "Please enter a valid email";
}
```

---

# 1️⃣7️⃣ EMAIL EXAMPLES

Valid examples:

```text
ramesh@gmail.com
user@example.com
hello@test.in
```

Invalid examples:

```text
ramesh
ramesh@
@gmail.com
ramesh@gmail
```

The exact email rules can be more complicated in real systems, but this is enough for beginner React validation.

---

# 1️⃣8️⃣ PASSWORD VALIDATION 🔐

Suppose our rule is:

```text
Minimum 8 characters
```

We can write:

```javascript
if (formData.password.length < 8) {

    newErrors.password =
        "Password must be at least 8 characters";
}
```

Example:

```text
12345
```

❌ Invalid

```text
12345678
```

✅ Meets the minimum length rule

---

# 1️⃣9️⃣ STRONGER PASSWORD VALIDATION

We can require:

```text
8+ characters
uppercase
lowercase
number
special character
```

Example:

```javascript
const passwordPattern =
    /^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&]).{8,}$/;
```

Then:

```javascript
if (!passwordPattern.test(formData.password)) {

    newErrors.password =
        "Password must contain uppercase, lowercase, number and special character";
}
```

### ⚠️ Beginner advice

Don't memorize the regular expression.

Understand the idea first:

```text
Password
 ↓
Check rules
 ↓
Valid?
```

---

# 2️⃣0️⃣ CONFIRM PASSWORD

A registration form often contains:

```text
Password
Confirm Password
```

Validation:

```javascript
if (
    formData.password !==
    formData.confirmPassword
) {

    newErrors.confirmPassword =
        "Passwords do not match";
}
```

Flow:

```text
Password
    ↓
Confirm Password
    ↓
Compare
    ↓
Same?
 ┌──┴──┐
YES    NO
 ↓      ↓
OK     Error
```

---

# 2️⃣1️⃣ ERROR MESSAGES

Good form validation should tell the user **what went wrong**.

Bad:

```text
Invalid input
```

Better:

```text
Email is required
```

Even better:

```text
Please enter a valid email address
```

Example:

```jsx
{errors.email && (
    <p>{errors.email}</p>
)}
```

Meaning:

```text
Does errors.email exist?
       ↓
YES → show error
NO  → show nothing
```

---

# 2️⃣2️⃣ ERROR STATE

A common structure:

```jsx
const [errors, setErrors] = useState({});
```

Example:

```javascript
{
    name: "Name is required",
    email: "Invalid email"
}
```

If there are no errors:

```javascript
{}
```

---

# 2️⃣3️⃣ FORM SUBMISSION FLOW

Remember this:

```text
User clicks Submit
        ↓
onSubmit
        ↓
preventDefault()
        ↓
Validate
        ↓
Any errors?
   ┌────┴────┐
  YES       NO
   ↓         ↓
Show       API
errors     request
             ↓
          Success
```

---

# 2️⃣4️⃣ WHY `event.preventDefault()`?

Normally, browser forms can perform their default submission behavior.

In a React SPA, we commonly want React to handle the submission.

So:

```javascript
event.preventDefault();
```

prevents the browser's default form submission.

Then we can:

```text
Validate
 ↓
API
 ↓
Update state
 ↓
Show result
```

---

# 2️⃣5️⃣ CONTROLLED FORM 🎛️

A controlled input is an input whose value is controlled by React state.

Example:

```jsx
const [name, setName] =
    useState("");

<input
    value={name}
    onChange={(event) =>
        setName(event.target.value)
    }
/>
```

Flow:

```text
Input
 ↓
onChange
 ↓
React State
 ↓
value
 ↓
Input
```

React is the source of truth.

---

# 2️⃣6️⃣ WHY CONTROLLED FORMS?

Controlled forms make it easy to:

```text
Read values
Validate values
Show errors
Disable buttons
Transform input
Submit API data
```

Example:

```jsx
<button
    disabled={!formData.email}
>
    Submit
</button>
```

---

# 2️⃣7️⃣ UNCONTROLLED FORM

In an uncontrolled form, the DOM keeps the current input value instead of React state controlling every change.

A common React approach uses:

```jsx
useRef()
```

Example:

```jsx
import { useRef } from "react";

function Form() {

    const nameRef = useRef();

    function handleSubmit(event) {

        event.preventDefault();

        console.log(
            nameRef.current.value
        );
    }

    return (
        <form onSubmit={handleSubmit}>

            <input
                ref={nameRef}
                type="text"
            />

            <button type="submit">
                Submit
            </button>

        </form>
    );
}

export default Form;
```

Here React does not store the input value in state on every keystroke.

---

# 2️⃣8️⃣ CONTROLLED vs UNCONTROLLED

| Controlled | Uncontrolled |
|---|---|
| React state controls value | DOM controls value |
| Uses `useState` commonly | Uses `ref` commonly |
| Easy validation | Can be simpler for some cases |
| Easy conditional UI | Less state-driven |
| Common in React forms | Useful with refs/form libraries |

### 🧠 Easy memory trick

```text
Controlled
= React controls input

Uncontrolled
= DOM controls input
```

---

# 2️⃣9️⃣ WHAT IS A FORM LIBRARY?

Large forms can contain:

```text
20 fields
30 fields
Complex validation
Nested fields
Dynamic fields
Error handling
Submission logic
```

Managing everything manually can become difficult.

A form library helps manage this complexity.

Your syllabus specifically includes:

> React Hook Form.

---

# 3️⃣0️⃣ REACT HOOK FORM

React Hook Form is a popular library for managing React forms.

Install:

```bash
npm install react-hook-form
```

Then:

```jsx
import { useForm } from "react-hook-form";
```

---

# 3️⃣1️⃣ BASIC REACT HOOK FORM

```jsx
import { useForm } from "react-hook-form";

function Register() {

    const {
        register,
        handleSubmit
    } = useForm();

    function onSubmit(data) {

        console.log(data);
    }

    return (
        <form
            onSubmit={handleSubmit(onSubmit)}
        >

            <input
                {...register("name")}
                placeholder="Name"
            />

            <input
                {...register("email")}
                placeholder="Email"
            />

            <input
                {...register("password")}
                type="password"
                placeholder="Password"
            />

            <button type="submit">
                Register
            </button>

        </form>
    );
}

export default Register;
```

---

# 3️⃣2️⃣ UNDERSTAND `useForm()`

```jsx
const {
    register,
    handleSubmit
} = useForm();
```

Think of it as:

```text
useForm()
   │
   ├── register
   │
   └── handleSubmit
```

---

# 3️⃣3️⃣ WHAT DOES `register()` DO?

Example:

```jsx
<input
    {...register("name")}
/>
```

It tells React Hook Form:

> "This input is a field called `name`."

Another:

```jsx
<input
    {...register("email")}
/>
```

means:

```text
field name = email
```

---

# 3️⃣4️⃣ WHAT DOES `handleSubmit()` DO?

```jsx
<form
    onSubmit={handleSubmit(onSubmit)}
>
```

When the user submits:

```text
Submit
 ↓
handleSubmit()
 ↓
Validation
 ↓
onSubmit(data)
```

Then:

```javascript
function onSubmit(data) {

    console.log(data);
}
```

could receive:

```javascript
{
    name: "Ramesh",
    email: "ramesh@gmail.com",
    password: "12345678"
}
```

---

# 3️⃣5️⃣ REACT HOOK FORM VALIDATION

We can add rules.

Example:

```jsx
<input
    {...register("name", {
        required: "Name is required"
    })}
/>
```

Email:

```jsx
<input
    {...register("email", {
        required: "Email is required"
    })}
/>
```

---

# 3️⃣6️⃣ ACCESSING ERRORS

```jsx
const {
    register,
    handleSubmit,
    formState: {
        errors
    }
} = useForm();
```

Then:

```jsx
{errors.name && (
    <p>{errors.name.message}</p>
)}
```

Flow:

```text
Validation fails
      ↓
errors.name
      ↓
message
      ↓
Show error
```

---

# 3️⃣7️⃣ COMPLETE REACT HOOK FORM EXAMPLE

```jsx
import { useForm } from "react-hook-form";

function Register() {

    const {
        register,
        handleSubmit,
        formState: {
            errors
        }
    } = useForm();

    function onSubmit(data) {

        console.log(data);
    }

    return (
        <form
            onSubmit={
                handleSubmit(onSubmit)
            }
        >

            <h1>Register</h1>

            <input
                {...register("name", {
                    required:
                        "Name is required"
                })}
                placeholder="Name"
            />

            {errors.name && (
                <p>
                    {errors.name.message}
                </p>
            )}

            <input
                {...register("email", {
                    required:
                        "Email is required"
                })}
                placeholder="Email"
            />

            {errors.email && (
                <p>
                    {errors.email.message}
                </p>
            )}

            <input
                type="password"
                {...register("password", {
                    required:
                        "Password is required",
                    minLength: {
                        value: 8,
                        message:
                            "Password must be at least 8 characters"
                    }
                })}
                placeholder="Password"
            />

            {errors.password && (
                <p>
                    {errors.password.message}
                </p>
            )}

            <button type="submit">
                Register
            </button>

        </form>
    );
}

export default Register;
```

---

# 3️⃣8️⃣ DRY RUN — REACT HOOK FORM

User enters:

```text
Name:
Ramesh
```

Then:

```text
register("name")
        ↓
React Hook Form
        ↓
stores field value
```

User clicks:

```text
Register
```

Then:

```text
handleSubmit()
        ↓
Validation
        ↓
Valid?
   ┌────┴────┐
  YES       NO
   ↓         ↓
onSubmit   errors
   ↓         ↓
data       message
```

---

# 3️⃣9️⃣ EMAIL VALIDATION WITH REACT HOOK FORM

Example:

```jsx
<input
    {...register("email", {
        required:
            "Email is required",

        pattern: {
            value:
                /^[^\s@]+@[^\s@]+\.[^\s@]+$/,

            message:
                "Enter a valid email"
        }
    })}
/>
```

---

# 4️⃣0️⃣ PASSWORD VALIDATION WITH REACT HOOK FORM

```jsx
<input
    type="password"
    {...register("password", {

        required:
            "Password is required",

        minLength: {
            value: 8,

            message:
                "Minimum 8 characters"
        }

    })}
/>
```

---

# 4️⃣1️⃣ REQUIRED + EMAIL + PASSWORD

A practical form may look like:

```text
Name
 └── required

Email
 ├── required
 └── valid email

Password
 ├── required
 └── minimum 8 characters
```

This is a very common pattern.

---

# 4️⃣2️⃣ CONTROLLED FORM vs REACT HOOK FORM

Don't confuse these concepts.

### Controlled form

You manually manage state:

```jsx
const [email, setEmail] =
    useState("");
```

### React Hook Form

The library manages form state and field registration:

```jsx
register("email")
```

You don't have to write a separate `useState` for every simple field.

---

# 4️⃣3️⃣ WHEN SHOULD YOU USE CONTROLLED FORMS?

Use controlled forms when:

```text
Small form
Simple fields
Custom behavior
You want complete control
```

Example:

```text
Search
Login
Small contact form
```

---

# 4️⃣4️⃣ WHEN SHOULD YOU USE REACT HOOK FORM?

Useful when:

```text
Large form
Many fields
Complex validation
Many error messages
Dynamic fields
Form-heavy application
```

For example:

```text
Employee registration
Customer registration
Checkout
Large admin form
```

---

# 4️⃣5️⃣ COMMON MISTAKES ❌

## Mistake 1 — Forgetting `name`

If using a manual object-based form:

```jsx
<input
    name="email"
/>
```

is important so the generic change handler knows which field changed.

---

## Mistake 2 — Forgetting `preventDefault()`

```javascript
function handleSubmit(event) {

    event.preventDefault();

}
```

---

## Mistake 3 — Not showing validation errors

Validation without useful feedback creates a bad user experience.

---

## Mistake 4 — Only validating on frontend

Remember:

```text
Frontend validation
       +
Backend validation
```

---

## Mistake 5 — Putting too much logic inside JSX

Avoid huge expressions inside JSX.

Prefer:

```javascript
function validateForm() {
    // validation logic
}
```

---

# 4️⃣6️⃣ BEST PRACTICES ⭐

### ✅ 1. Give every field a clear name

```text
name
email
password
```

### ✅ 2. Give useful error messages

```text
Email is required
```

instead of:

```text
Error
```

### ✅ 3. Validate before API request

```text
Form
 ↓
Validation
 ↓
API
```

### ✅ 4. Disable submit when appropriate

For example, while submitting:

```text
Submitting...
```

instead of allowing multiple clicks.

### ✅ 5. Validate on backend too

Never trust only frontend validation.

### ✅ 6. Keep form logic organized

For larger forms, consider a form library.

---

# 4️⃣7️⃣ REAL-WORLD REGISTRATION ARCHITECTURE 🚀

```text
              REGISTER FORM
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
       Name        Email      Password
        │           │           │
        └───────────┼───────────┘
                    ↓
                Validation
                    ↓
              Any errors?
               /       \
             YES        NO
              ↓          ↓
        Show errors     API
                          ↓
                       Backend
                          ↓
                       Database
                          ↓
                      Response
                          ↓
                     Success UI
```

This is the foundation for Chapter 27 authentication.

---

# 4️⃣8️⃣ INTERVIEW QUESTIONS 🎤

### Q1. What is a controlled component?

**Answer:**  
A form element whose value is controlled by React state.

---

### Q2. What is an uncontrolled component?

**Answer:**  
A form element whose value is primarily managed by the DOM, commonly accessed through a ref.

---

### Q3. Why is validation important?

**Answer:**  
Validation ensures user input follows the required rules and provides useful feedback before data is processed.

---

### Q4. What is client-side validation?

**Answer:**  
Validation performed in the browser/frontend before data is sent to the server.

---

### Q5. Is frontend validation enough?

**Answer:**  
No. Backend validation is also required because frontend validation can be bypassed.

---

### Q6. What is React Hook Form?

**Answer:**  
React Hook Form is a library that helps manage React forms and their validation with less manual form-state code.

---

### Q7. What does `register()` do in React Hook Form?

**Answer:**  
It registers an input field with React Hook Form so the library can track and validate it.

---

### Q8. What does `handleSubmit()` do?

**Answer:**  
It handles form submission and runs validation before calling the submit callback when appropriate.

---

### Q9. How do you display an error in React Hook Form?

**Answer:**

```jsx
{errors.email && (
    <p>{errors.email.message}</p>
)}
```

---

### Q10. Controlled vs uncontrolled?

**Answer:**

```text
Controlled
→ React state controls the value

Uncontrolled
→ DOM/ref controls the value
```

---

# 4️⃣9️⃣ PRACTICE TASKS 📝

## Task 1 — Login Form

Create:

```text
Email
Password
Login button
```

Validation:

```text
Email required
Password required
```

---

## Task 2 — Registration Form

Create:

```text
Name
Email
Password
Confirm Password
Register
```

Validation:

```text
Name required
Email valid
Password minimum 8
Confirm password must match
```

---

## Task 3 — Error Messages

Display the error under the corresponding input.

Example:

```text
Email
[____________]

❌ Please enter a valid email
```

---

## Task 4 — Controlled Form

Build the registration form using:

```text
useState
```

---

## Task 5 — React Hook Form

Build the same registration form using:

```text
useForm()
register()
handleSubmit()
errors
```

Then compare both approaches.

---

# 5️⃣0️⃣ MINI CHALLENGE 💪

## 🚀 BUILD A STUDENT REGISTRATION FORM

Create:

```text
Student Registration

Name
Email
Phone
Course
Password
Confirm Password

[ Register ]
```

### Validation

```text
Name
→ Required

Email
→ Required
→ Valid email

Phone
→ Required

Course
→ Required

Password
→ Minimum 8 characters

Confirm Password
→ Must match password
```

### Required behavior

```text
User clicks Register
        ↓
Validate
        ↓
Errors?
 ┌──────┴──────┐
YES            NO
 ↓              ↓
Show errors    Submit
                 ↓
              console.log()
```

Try to build it yourself first.

Don't look for the solution immediately.

---

# 5️⃣1️⃣ QUIZ 🧠

### Q1. What is a form used for?

**Answer:**  
To collect information from the user.

---

### Q2. What is validation?

**Answer:**  
Checking whether user input follows required rules.

---

### Q3. What does client-side validation mean?

**Answer:**  
Validation performed in the frontend/browser.

---

### Q4. What is a controlled input?

**Answer:**  
An input whose value is controlled by React state.

---

### Q5. What is commonly used for uncontrolled inputs?

**Answer:**

```jsx
useRef()
```

---

### Q6. Why do we use `event.preventDefault()`?

**Answer:**  
To prevent the browser's default form submission behavior so React can handle the submission.

---

### Q7. What library are we learning for complex React forms?

**Answer:**

```text
React Hook Form
```

---

### Q8. What does `register("email")` mean?

**Answer:**  
It registers the `email` field with React Hook Form.

---

### Q9. Where should important validation ultimately happen?

**Answer:**  
On the backend/server as well as on the frontend.

---

### Q10. What is the basic form flow?

**Answer:**

```text
Input
 ↓
State / Form Library
 ↓
Validation
 ↓
Errors or Submit
 ↓
API
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
FORM
 ↓
INPUT
 ↓
STATE
 ↓
VALIDATION
 ↓
ERROR
 ↓
SUBMIT
 ↓
API
```

For React Hook Form:

```text
useForm()
   ↓
register()
   ↓
handleSubmit()
   ↓
validation
   ↓
errors
   ↓
onSubmit(data)
```

And remember:

```text
Controlled
= React controls

Uncontrolled
= DOM controls
```

---

# ✅ CHAPTER 26 CHECKLIST

Before moving forward, make sure you understand:

- [ ] Form architecture
- [ ] Form state
- [ ] Controlled forms
- [ ] Uncontrolled forms
- [ ] Validation
- [ ] Client-side validation
- [ ] Required fields
- [ ] Error messages
- [ ] Email validation
- [ ] Password validation
- [ ] Confirm password
- [ ] `preventDefault()`
- [ ] React Hook Form
- [ ] `useForm()`
- [ ] `register()`
- [ ] `handleSubmit()`
- [ ] `errors`
- [ ] Frontend + backend validation

# 🎉 CHAPTER 26 COMPLETE

You now have the foundation for building **professional React forms**.

## ⏭️ NEXT CHAPTER — CHAPTER 27: AUTHENTICATION 🔐

We will learn:

```text
Login
Register
Logout
Authentication Flow
JWT
Access Token
Refresh Token
LocalStorage
Cookies
Protected Routes
Auth Context
Role-Based Authentication
```

Type **`NEXT`** when you are ready.