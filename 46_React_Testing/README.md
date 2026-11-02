# 🧪 CHAPTER 46 — REACT TESTING

## 🎯 Chapter Goal

In this chapter, you will learn how to test React applications.

We will start from the absolute basics:

```text
What is testing?
       ↓
Why do we test?
       ↓
Unit Testing
       ↓
Component Testing
       ↓
Integration Testing
       ↓
Vitest
       ↓
React Testing Library
       ↓
User Interactions
       ↓
Mocking API Calls
```

---

# 1. 🧠 What Is Testing?

**Testing** means checking whether our application behaves as expected.

For example, suppose we have:

```jsx
function add(a, b) {
    return a + b;
}
```

We expect:

```text
add(2, 3)
   ↓
5
```

A test can automatically verify this.

```text
Expected → 5
Actual   → 5
           ↓
          ✅ PASS
```

If we get:

```text
Expected → 5
Actual   → 6
           ↓
          ❌ FAIL
```

---

# 2. 🤔 Why Do We Need Testing?

Imagine you build a large e-commerce application.

It has:

```text
Login
Products
Cart
Checkout
Payment
Orders
Profile
Admin
```

You change the login code.

How do you know you didn't break something else?

You could manually test everything:

```text
Login
Products
Cart
Checkout
Payment
Orders
...
```

This takes a lot of time.

Automated tests help.

```text
Code change
    ↓
Run tests
    ↓
Tests automatically check behavior
    ↓
PASS / FAIL
```

---

# 3. 🧠 Real-World Example

Think about a car.

Before selling a car, manufacturers test:

```text
🚗 Brakes
🚗 Engine
🚗 Lights
🚗 Doors
🚗 Safety
```

Similarly, we test software:

```text
⚛️ Components
⚛️ Functions
⚛️ Forms
⚛️ Buttons
⚛️ API behavior
```

---

# 4. 🧪 Types of Testing

For React applications, you should understand:

```text
Testing
│
├── Unit Testing
│
├── Component Testing
│
└── Integration Testing
```

---

# 5. 🔹 Unit Testing

Unit testing tests a **small individual piece of code**.

For example:

```javascript
function add(a, b) {
    return a + b;
}
```

Test:

```text
add(2, 3)
```

Expected:

```text
5
```

The function is the unit.

```text
Function
   ↓
Input
   ↓
Output
   ↓
Check
```

---

# 6. 🔹 Component Testing

Component testing checks whether a React component behaves correctly.

Example:

```jsx
function Welcome() {
    return <h1>Welcome Ramesh</h1>;
}
```

We can test:

```text
Does the component render?
        ↓
Does "Welcome Ramesh" appear?
```

---

# 7. 🔹 Integration Testing

Integration testing checks whether multiple parts work together.

For example:

```text
Login Form
    ↓
Submit
    ↓
API
    ↓
Response
    ↓
UI Update
```

We can test the complete interaction.

---

# 8. 🧠 Easy Difference

Remember:

```text
Unit
 ↓
One small piece

Component
 ↓
One React component

Integration
 ↓
Multiple pieces working together
```

Example:

```text
🧩 Unit
add()

⚛️ Component
LoginForm

🔗 Integration
LoginForm + API + UI
```

---

# 9. 🛠️ What Is Vitest?

**Vitest** is a testing framework commonly used with modern Vite projects.

It provides tools for writing and running tests.

For example:

```javascript
import { describe, it, expect } from "vitest";
```

Then:

```javascript
describe("addition", () => {

    it("adds two numbers", () => {

        expect(
            2 + 3
        ).toBe(5);

    });

});
```

---

# 10. 🧠 Understand `describe()`

`describe()` groups related tests.

```javascript
describe("Math functions", () => {

    // tests here

});
```

Think:

```text
describe()
    ↓
Test group
```

---

# 11. 🧠 Understand `it()`

`it()` defines one test.

```javascript
it(
    "adds two numbers",
    () => {

        // test
    }
);
```

You can also use:

```javascript
test()
```

depending on the testing framework/API style.

---

# 12. 🧠 Understand `expect()`

`expect()` checks the result.

Example:

```javascript
expect(2 + 3).toBe(5);
```

Meaning:

```text
Actual result
     ↓
2 + 3
     ↓
5

Expected
     ↓
5

Same?
 ↓
YES → PASS
```

---

# 13. 📦 Installing Testing Tools

For a Vite React project, install the testing packages.

```bash
npm install -D vitest jsdom
```

For React Testing Library:

```bash
npm install -D @testing-library/react @testing-library/jest-dom
```

For user interactions:

```bash
npm install -D @testing-library/user-event
```

---

# 14. 📁 Test File Structure

A simple project can look like:

```text
src/
│
├── components/
│   └── Button.jsx
│
├── tests/
│   └── Button.test.jsx
│
├── App.jsx
└── main.jsx
```

Another common approach is placing the test beside the component:

```text
src/
│
├── components/
│   ├── Button.jsx
│   └── Button.test.jsx
```

Both approaches are possible.

---

# 15. 🧪 First Vitest Test

Create:

```text
src/math.js
```

```javascript
export function add(a, b) {

    return a + b;
}
```

Now:

```text
src/math.test.js
```

```javascript
import {
    describe,
    it,
    expect
} from "vitest";

import {
    add
} from "./math";


describe("add()", () => {

    it(
        "should add two numbers",
        () => {

            expect(
                add(2, 3)
            ).toBe(5);

        }
    );

});
```

---

# 16. 🔍 Dry Run

The test:

```javascript
add(2, 3)
```

calls:

```javascript
return 2 + 3;
```

Result:

```text
5
```

Then:

```javascript
expect(5).toBe(5);
```

Therefore:

```text
Expected = 5
Actual   = 5

        ↓

      ✅ PASS
```

---

# 17. ▶️ Running Tests

In `package.json`, you can define a script such as:

```json
{
    "scripts": {
        "test": "vitest"
    }
}
```

Then:

```bash
npm run test
```

Vitest starts the test runner.

---

# 18. 🧪 Test Failure

Suppose:

```javascript
expect(
    add(2, 3)
).toBe(6);
```

Actual:

```text
5
```

Expected:

```text
6
```

Result:

```text
❌ FAIL
```

This is useful because the test tells you that your expected behavior and actual behavior don't match.

---

# 19. 🧠 What Is React Testing Library?

**React Testing Library** provides utilities for testing React components from the user's perspective.

Instead of asking:

> "Did React create this exact internal object?"

we generally ask:

> "Can the user see and interact with the expected UI?"

For example:

```text
User sees:
"Login"

User clicks:
"Login"

User enters:
email

User sees:
"Welcome"
```

This makes tests more focused on behavior.

---

# 20. ⚛️ First Component

Create:

```text
src/components/Greeting.jsx
```

```jsx
function Greeting() {

    return (
        <h1>
            Hello Ramesh
        </h1>
    );
}


export default Greeting;
```

---

# 21. 🧪 Component Test

Create:

```text
Greeting.test.jsx
```

```jsx
import {
    render,
    screen
} from "@testing-library/react";

import {
    describe,
    it,
    expect
} from "vitest";

import Greeting from "./Greeting";


describe(
    "Greeting",
    () => {

        it(
            "renders greeting",
            () => {

                render(
                    <Greeting />
                );


                expect(
                    screen.getByText(
                        "Hello Ramesh"
                    )
                ).toBeInTheDocument();

            }
        );

    }
);
```

---

# 22. 🔍 Understand `render()`

```javascript
render(
    <Greeting />
);
```

This renders the React component in the test environment.

Think:

```text
Greeting component
       ↓
render()
       ↓
Test DOM
```

---

# 23. 🔎 Understand `screen`

`screen` provides queries for finding elements in the rendered UI.

Example:

```javascript
screen.getByText(
    "Hello Ramesh"
);
```

Meaning:

```text
Search the rendered page
       ↓
Find this text
       ↓
"Hello Ramesh"
```

---

# 24. 🧠 Understand `getByText()`

```javascript
screen.getByText(
    "Hello Ramesh"
);
```

It looks for visible text.

If found:

```text
✅ Test can continue
```

If not found:

```text
❌ Test fails
```

---

# 25. 🧠 Understand `toBeInTheDocument()`

```javascript
expect(
    screen.getByText(
        "Hello Ramesh"
    )
).toBeInTheDocument();
```

Meaning:

```text
Does this element exist
in the rendered document?
```

Expected:

```text
YES → ✅ PASS
NO  → ❌ FAIL
```

---

# 26. 🖱️ User Interactions

Testing isn't only about checking text.

We also need to test user actions.

Examples:

```text
Click
Type
Submit
Select
Upload
```

React Testing Library's `user-event` package helps simulate realistic user interactions.

---

# 27. 🧩 Button Example

```jsx
import {
    useState
} from "react";


function Counter() {

    const [count, setCount] =
        useState(0);


    return (

        <div>

            <h1>
                {count}
            </h1>


            <button
                onClick={() =>
                    setCount(
                        count + 1
                    )
                }
            >
                Increment
            </button>

        </div>
    );
}


export default Counter;
```

Initial UI:

```text
0

[ Increment ]
```

---

# 28. 🧪 Testing Button Click

```jsx
import {
    render,
    screen
} from "@testing-library/react";

import userEvent from
    "@testing-library/user-event";

import {
    describe,
    it,
    expect
} from "vitest";

import Counter from "./Counter";


describe(
    "Counter",
    () => {

        it(
            "increments count",
            async () => {

                const user =
                    userEvent.setup();


                render(
                    <Counter />
                );


                const button =
                    screen.getByRole(
                        "button",
                        {
                            name: "Increment"
                        }
                    );


                await user.click(
                    button
                );


                expect(
                    screen.getByText("1")
                ).toBeInTheDocument();

            }
        );

    }
);
```

---

# 29. 🔍 Dry Run

Initially:

```text
count = 0
```

Test renders component:

```text
0
Increment
```

Then:

```javascript
await user.click(button);
```

User clicks:

```text
Increment
```

React executes:

```javascript
setCount(count + 1);
```

State becomes:

```text
1
```

UI becomes:

```text
1
Increment
```

Test checks:

```javascript
screen.getByText("1")
```

Result:

```text
✅ PASS
```

---

# 30. 🧠 Why `userEvent`?

You could encounter older examples using lower-level event helpers.

For user-focused tests, `user-event` is generally preferred because it models user interactions more realistically.

Think:

```text
user.click()
user.type()
user.clear()
user.selectOptions()
```

---

# 31. 📝 Testing Forms

Suppose:

```jsx
function LoginForm() {

    return (

        <form>

            <label>
                Email

                <input
                    type="email"
                />
            </label>


            <button>
                Login
            </button>

        </form>
    );
}
```

A test should verify that users can find and interact with the form.

---

# 32. 🧪 Form Test

```jsx
render(
    <LoginForm />
);


const email =
    screen.getByRole(
        "textbox",
        {
            name: "Email"
        }
    );


await user.type(
    email,
    "ramesh@gmail.com"
);
```

Now the test has simulated a user typing into the email field.

---

# 33. 🔎 Why `getByRole()`?

Example:

```javascript
screen.getByRole(
    "button",
    {
        name: "Login"
    }
);
```

This asks for a button that a user can identify as:

```text
Login
```

Using accessible roles is usually a strong testing approach.

Examples:

```text
button
textbox
heading
link
checkbox
radio
```

---

# 34. 🧠 Testing Philosophy

Try to write tests around what the user can observe.

Prefer:

```text
What does the user see?
What can the user click?
What can the user type?
What happens afterward?
```

Instead of depending heavily on implementation details.

---

# 35. 🌐 Mocking API Calls

Now let's discuss an important real-world problem.

Suppose React calls:

```text
GET /products
```

During testing, we don't necessarily want every test to call the real backend.

Why?

```text
Backend may be offline
Database may be unavailable
Network may be slow
Test data may change
Tests become less predictable
```

Instead, we can **mock** the API behavior.

---

# 36. 🧠 What Does Mocking Mean?

Mocking means replacing a real dependency with a controlled fake version for testing.

Real:

```text
React
 ↓
Real API
 ↓
Database
```

Test:

```text
React
 ↓
Mock API
 ↓
Fake Response
```

Example fake response:

```json
[
    {
        "id": 1,
        "name": "Laptop"
    },
    {
        "id": 2,
        "name": "Mouse"
    }
]
```

---

# 37. 🧪 Simple Fetch Mock

For a simple example, we can mock `fetch`.

```javascript
globalThis.fetch =
    vi.fn();
```

Then provide a response:

```javascript
fetch.mockResolvedValue({

    ok: true,

    json: async () => [

        {
            id: 1,
            name: "Laptop"
        }

    ]

});
```

Now your component can behave as if the API returned that data.

---

# 38. 🧠 Why Mock API Calls?

Suppose your component does:

```text
fetch("/products")
```

Without mocking:

```text
Test
 ↓
Internet/server
 ↓
API
 ↓
Database
```

With mocking:

```text
Test
 ↓
Mock response
 ↓
Component
```

This makes the test:

```text
Fast
Predictable
Independent
```

---

# 39. 🔄 Testing Loading State

A good API component usually has:

```text
Loading
   ↓
Success

or

Loading
   ↓
Error
```

Example:

```jsx
if (loading) {
    return <p>Loading...</p>;
}
```

Test:

```javascript
expect(
    screen.getByText(
        "Loading..."
    )
).toBeInTheDocument();
```

---

# 40. ❌ Testing Error State

Suppose the API fails.

The component displays:

```text
Failed to load products
```

Test should verify:

```javascript
expect(
    screen.getByText(
        "Failed to load products"
    )
).toBeInTheDocument();
```

So we can test:

```text
✅ Loading
✅ Success
✅ Error
```

---

# 41. 🧪 Testing Complete User Flow

Imagine:

```text
Product Page
      ↓
Loading
      ↓
Products appear
      ↓
User clicks Add to Cart
      ↓
Cart count changes
```

This is a good integration-style scenario.

Test the behavior:

```text
Render
 ↓
Wait for products
 ↓
Find product
 ↓
Click button
 ↓
Check cart
```

---

# 42. ⏳ Async Testing

When something happens asynchronously, tests often need to wait.

For example:

```javascript
const product =
    await screen.findByText(
        "Laptop"
    );
```

`findBy...` queries are useful when the element appears after an asynchronous operation.

Remember:

```text
getBy
 ↓
Expected to exist now

findBy
 ↓
Expected to appear asynchronously
```

---

# 43. 🔎 `getBy` vs `findBy`

### `getBy`

```javascript
screen.getByText(
    "Hello"
);
```

Use when the element should already be present.

### `findBy`

```javascript
await screen.findByText(
    "Laptop"
);
```

Use when the element may appear after asynchronous work.

---

# 44. 🧠 Testing Query Types

You will commonly see:

```text
getBy...
queryBy...
findBy...
```

Simple idea:

```text
getBy
 ↓
Must exist now

queryBy
 ↓
Can check if it exists or doesn't

findBy
 ↓
Waits for async appearance
```

---

# 45. ❌ Example `queryBy`

Suppose an error message should **not** be displayed.

```javascript
expect(
    screen.queryByText(
        "Error"
    )
).not.toBeInTheDocument();
```

If it doesn't exist:

```text
✅ PASS
```

---

# 46. 🧪 Unit + Component + Integration

Let's compare.

| Type | What it tests | Example |
|---|---|---|
| Unit | Small function | `add()` |
| Component | One component | `Button` |
| Integration | Multiple pieces | Form + API + UI |

---

# 47. 🏗️ Real Project Testing Structure

For a larger project:

```text
src/
│
├── components/
│   ├── Button.jsx
│   ├── ProductCard.jsx
│   └── LoginForm.jsx
│
├── pages/
│   ├── Home.jsx
│   └── Products.jsx
│
├── services/
│   └── productService.js
│
├── tests/
│   ├── Button.test.jsx
│   ├── ProductCard.test.jsx
│   └── Products.test.jsx
│
└── App.jsx
```

---

# 48. 🧠 What Should We Test?

You don't need to test every implementation detail.

Focus on important behavior.

For example, an e-commerce application:

```text
☑️ Login works
☑️ Product appears
☑️ Search works
☑️ Add to cart works
☑️ Remove from cart works
☑️ Checkout validation works
☑️ API error appears correctly
```

---

# 49. ⚠️ COMMON TESTING MISTAKES

## ❌ Mistake 1 — Testing Implementation Details

Don't make tests depend unnecessarily on internal implementation.

Focus on user-visible behavior.

---

## ❌ Mistake 2 — Testing Only Happy Path

Don't test only:

```text
Success
```

Also test:

```text
Loading
Error
Empty state
Invalid input
```

---

## ❌ Mistake 3 — Using Real APIs Everywhere

Real APIs can make tests:

```text
Slow
Unstable
Dependent on backend
```

Mock external dependencies when appropriate.

---

## ❌ Mistake 4 — Not Testing User Interaction

A component may render correctly but its button may not work.

Test:

```text
Click
Type
Submit
```

---

## ❌ Mistake 5 — Ignoring Accessibility

Prefer queries that reflect how users interact with the application.

For example:

```javascript
getByRole(
    "button",
    {
        name: "Login"
    }
);
```

---

# 50. ✅ BEST PRACTICES

### 1. Write small tests

One test should have a clear purpose.

### 2. Test behavior

Think:

```text
What should the user experience?
```

### 3. Test important scenarios

```text
Success
Error
Loading
Empty
Interaction
```

### 4. Keep tests readable

Good:

```javascript
it(
    "shows error when login fails",
    () => {
        // ...
    }
);
```

### 5. Keep tests independent

One test should not depend on another test's state.

### 6. Use mocks carefully

Mock external dependencies where they improve isolation and reliability.

---

# 51. 🧪 PRACTICE 1 — Unit Test

Create:

```javascript
export function multiply(
    a,
    b
) {
    return a * b;
}
```

Write tests for:

```text
2 × 3 = 6
5 × 4 = 20
0 × 10 = 0
```

---

# 52. 🧪 PRACTICE 2 — Component Test

Create:

```jsx
function Welcome() {

    return (
        <h1>
            Welcome to React
        </h1>
    );
}
```

Test that:

```text
"Welcome to React"
```

appears.

---

# 53. 🧪 PRACTICE 3 — Button

Create:

```text
Counter
```

with:

```text
0
[ Increment ]
```

Test:

```text
Click button
     ↓
1
```

---

# 54. 🧪 PRACTICE 4 — Form

Create a login form:

```text
Email
Password

[ Login ]
```

Test:

```text
Type email
Type password
Click Login
```

---

# 55. 🧪 PRACTICE 5 — API

Create:

```text
ProductList
```

Test:

```text
Loading
Success
Error
```

Mock the API response.

---

# 56. 🧩 MINI PROJECT

## 🛒 Product Testing Project

Build a small product page.

```text
Products
───────────────

Laptop
₹50,000
[Add to Cart]

Mouse
₹1,000
[Add to Cart]
```

Write tests for:

### Test 1

Products render.

### Test 2

Product name appears.

### Test 3

Add to Cart works.

### Test 4

Cart count changes.

### Test 5

Loading state appears.

### Test 6

API error appears.

### Test 7

Mock API response.

---

# 57. 🧠 Complete Testing Flow

Remember:

```text
                 🧪 TEST
                    ↓
          ┌─────────┼─────────┐
          ↓         ↓         ↓
        Unit    Component  Integration
          ↓         ↓         ↓
       Function   UI       User Flow
                    ↓
              React Testing
                    ↓
           Vitest + RTL
```

---

# 58. 💼 INTERVIEW QUESTIONS

### Q1. What is testing?

Testing verifies that software behaves as expected.

---

### Q2. Why is testing important?

It helps catch bugs, prevent regressions and increase confidence when changing code.

---

### Q3. What is unit testing?

Testing a small isolated piece of functionality.

---

### Q4. What is component testing?

Testing a React component's behavior and rendered UI.

---

### Q5. What is integration testing?

Testing multiple parts of an application working together.

---

### Q6. What is Vitest?

Vitest is a test runner/framework commonly used with Vite-based projects.

---

### Q7. What is React Testing Library?

A library that provides utilities for testing React components based primarily on user-observable behavior.

---

### Q8. What does `render()` do?

It renders a React component into the test environment.

---

### Q9. What does `screen.getByRole()` do?

It finds an element using its accessible role and, optionally, accessible name.

---

### Q10. Why mock API calls?

To make tests more isolated, predictable and independent of real external services.

---

# 59. 🧠 INTERVIEW SCENARIO

### Question:

> Your React component fetches products from an API. How would you test it?

### Answer:

I would test the important states and behavior:

```text
1. Render component
2. Verify loading state
3. Mock successful API response
4. Verify products appear
5. Mock failed API response
6. Verify error message
7. Test user interactions
```

---

# 60. 📝 10-QUESTION QUIZ

### Q1. What does testing do?

**Answer:** It verifies that software behaves as expected.

---

### Q2. What is unit testing?

**Answer:** Testing a small isolated piece of functionality.

---

### Q3. What is component testing?

**Answer:** Testing the behavior and rendered UI of a React component.

---

### Q4. What is integration testing?

**Answer:** Testing multiple parts working together.

---

### Q5. What tool can run tests in a Vite React project?

**Answer:** Vitest.

---

### Q6. What library provides React-specific testing utilities?

**Answer:** React Testing Library.

---

### Q7. What does `render()` do?

**Answer:** It renders a React component into the test environment.

---

### Q8. What is `userEvent` used for?

**Answer:** Simulating realistic user interactions such as clicking and typing.

---

### Q9. Why mock an API?

**Answer:** To avoid depending on a real external API and make tests more predictable and isolated.

---

### Q10. What is the difference between `getBy` and `findBy`?

**Answer:**

```text
getBy
 ↓
Find something expected immediately

findBy
 ↓
Wait for something that appears asynchronously
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
🧪 React Testing

Vitest
   ↓
Runs tests

React Testing Library
   ↓
Tests React UI

render()
   ↓
Render component

screen
   ↓
Find UI elements

userEvent
   ↓
Simulate user actions

expect()
   ↓
Check result

Mock
   ↓
Fake external dependency
```

And:

```text
Unit
 ↓
Small function

Component
 ↓
React UI

Integration
 ↓
Multiple parts together
```

---

# ✅ CHAPTER 46 COMPLETE

You learned:

- 🧪 Why testing
- 🔹 Unit testing
- ⚛️ Component testing
- 🔗 Integration testing
- 🛠️ Vitest
- 🧪 React Testing Library
- `describe()`
- `it()`
- `expect()`
- `render()`
- `screen`
- `getByText()`
- `getByRole()`
- `getBy`
- `queryBy`
- `findBy`
- 🖱️ User interactions
- 📝 Form testing
- 🌐 API testing
- 🎭 Mocking API calls
- ⏳ Loading testing
- ❌ Error testing
- 💼 Interview questions
- 📝 Practice tasks
- 🧩 Mini project

# 🚀 NEXT CHAPTER — CHAPTER 47

## 🔐 Environment Variables

We will learn:

- `.env`
- `.env.local`
- Vite environment variables
- API URLs
- Development vs Production
- Environment-specific configuration
- What should **never** be stored in frontend environment variables
- Security rules
- React + FastAPI environment setup
- Common mistakes
- Interview questions
- Practical project

Type **NEXT** to continue.