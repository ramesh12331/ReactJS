# 🛠️ CHAPTER 28 — CRUD APPLICATION

## 🎯 Goal of This Chapter

This is one of the **most important practical React chapters**.

You have already learned:

```text
React
 ↓
Components
 ↓
Props
 ↓
State
 ↓
Forms
 ↓
API
 ↓
Authentication
```

Now we combine these skills to build a real application.

Our goal:

```text
             REACT CRUD APP
                   │
       ┌───────────┼───────────┐
       ↓           ↓           ↓
     CREATE       READ       UPDATE
       │           │           │
       └───────────┼───────────┘
                   ↓
                 DELETE
                   ↓
             SEARCH / FILTER
                   ↓
              PAGINATION
                   ↓
          LOADING / ERROR
                   ↓
                  API
```

---

# 📚 CHAPTER TOPICS

We will learn:

1. What is CRUD?
2. CRUD in real life
3. CRUD + HTTP methods
4. CRUD application architecture
5. Create
6. Read
7. Update
8. Delete
9. API integration
10. Loading state
11. Error state
12. Form validation
13. Search
14. Filter
15. Pagination
16. Complete CRUD flow
17. Folder structure
18. Common mistakes
19. Best practices
20. Interview questions
21. Practice tasks
22. Mini project challenge
23. Quiz
24. Final revision

---

# 1️⃣ WHAT IS CRUD?

CRUD represents four basic operations on data:

```text
C → Create
R → Read
U → Update
D → Delete
```

These operations are commonly mapped to HTTP methods like this:

```text
CREATE → POST
READ   → GET
UPDATE → PUT / PATCH
DELETE → DELETE
```

Your API notes use the same CRUD mapping.

---

# 2️⃣ REAL-WORLD EXAMPLE 🏪

Imagine an **Employee Management System**.

You need to:

```text
Add employee
View employees
Edit employee
Delete employee
Search employee
Filter employee
```

That's CRUD.

Example:

```text
Employee
────────────────────
ID: 101
Name: Ramesh
Email: ramesh@gmail.com
City: Hyderabad
Salary: 50000
```

Operations:

```text
➕ Add Employee       → CREATE

👀 View Employee     → READ

✏️ Edit Employee     → UPDATE

🗑️ Delete Employee  → DELETE
```

---

# 3️⃣ CRUD + HTTP METHODS

Remember this table:

| CRUD | HTTP Method | Purpose |
|---|---|---|
| Create | POST | Create new data |
| Read | GET | Retrieve data |
| Update | PUT/PATCH | Modify data |
| Delete | DELETE | Remove data |

For example:

```text
POST   /employees
GET    /employees
GET    /employees/101
PUT    /employees/101
DELETE /employees/101
```

Your API reference also describes POST as commonly used to create, GET to retrieve, PUT/PATCH to update, and DELETE to remove data.

---

# 4️⃣ CRUD APPLICATION ARCHITECTURE 🏗️

A typical React CRUD application looks like:

```text
                  React Frontend
                       │
              ┌────────┼────────┐
              ↓        ↓        ↓
            Form      List    Search
              │        │        │
              └────────┼────────┘
                       ↓
                    API Layer
                       ↓
                    Backend
                       ↓
                    Database
```

More specifically:

```text
React
  ↓
fetch() / Axios
  ↓
REST API
  ↓
Backend
  ↓
Database
```

This matches the API request flow in your API notes: frontend sends a request to the backend, the backend communicates with the database, and the response comes back to the frontend.

---

# 5️⃣ OUR CRUD PROJECT

Let's imagine we are building:

# 👨‍💼 Employee Management System

Features:

```text
┌─────────────────────────────────────┐
│       Employee Management           │
│                                     │
│ Search: [____________]              │
│                                     │
│ [Add Employee]                      │
│                                     │
│ ID   Name    Email       Actions    │
│ 1    Ramesh  r@gmail.com [Edit]     │
│                         [Delete]     │
└─────────────────────────────────────┘
```

---

# 6️⃣ CREATE ➕

Create means:

> Add new data.

Example:

```text
User fills form
      ↓
Name
Email
City
Salary
      ↓
Click Add
      ↓
POST request
      ↓
Backend
      ↓
Database
      ↓
New employee created
```

---

# 7️⃣ CREATE REQUEST

Example:

```javascript
async function createEmployee(employee) {

    const response = await fetch(
        "http://localhost:8000/employees",
        {
            method: "POST",

            headers: {
                "Content-Type":
                    "application/json"
            },

            body: JSON.stringify(employee)
        }
    );

    return response.json();
}
```

---

# 8️⃣ UNDERSTAND `JSON.stringify()`

Suppose we have:

```javascript
const employee = {
    name: "Ramesh",
    email: "ramesh@gmail.com"
};
```

`fetch()` request body commonly needs a JSON string.

So:

```javascript
JSON.stringify(employee)
```

produces JSON text representing that object.

Conceptually:

```text
JavaScript Object
       ↓
JSON.stringify()
       ↓
JSON data
       ↓
HTTP request
```

---

# 9️⃣ CREATE FORM

Example:

```jsx
import { useState } from "react";

function EmployeeForm() {

    const [formData, setFormData] = useState({
        name: "",
        email: "",
        city: ""
    });

    function handleChange(event) {

        const {
            name,
            value
        } = event.target;

        setFormData({
            ...formData,
            [name]: value
        });
    }

    async function handleSubmit(event) {

        event.preventDefault();

        console.log(formData);
    }

    return (
        <form onSubmit={handleSubmit}>

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
                name="city"
                value={formData.city}
                onChange={handleChange}
                placeholder="City"
            />

            <button type="submit">
                Add Employee
            </button>

        </form>
    );
}

export default EmployeeForm;
```

---

# 🔟 READ 👀

Read means:

> Retrieve existing data.

Example:

```text
GET /employees
```

Backend returns:

```javascript
[
    {
        id: 1,
        name: "Ramesh",
        email: "ramesh@gmail.com"
    },
    {
        id: 2,
        name: "John",
        email: "john@gmail.com"
    }
]
```

React displays it.

---

# 1️⃣1️⃣ FETCH DATA WITH `useEffect`

```jsx
import {
    useEffect,
    useState
} from "react";

function Employees() {

    const [employees, setEmployees] =
        useState([]);

    useEffect(() => {

        fetch(
            "http://localhost:8000/employees"
        )
            .then(response =>
                response.json()
            )
            .then(data => {

                setEmployees(data);

            });

    }, []);

    return (
        <div>

            <h1>Employees</h1>

            {employees.map(employee => (

                <div key={employee.id}>

                    <h3>
                        {employee.name}
                    </h3>

                    <p>
                        {employee.email}
                    </p>

                </div>

            ))}

        </div>
    );
}

export default Employees;
```

---

# 1️⃣2️⃣ DRY RUN — READ

When the component loads:

```text
Employees component
       ↓
useEffect()
       ↓
GET /employees
       ↓
Backend
       ↓
Database
       ↓
Response
       ↓
setEmployees(data)
       ↓
React re-renders
       ↓
Employee list displayed
```

---

# 1️⃣3️⃣ UPDATE ✏️

Update means:

> Modify existing data.

Example:

```text
Existing:

Ramesh
Hyderabad

        ↓ Edit

Ramesh
Bangalore
```

API:

```text
PUT /employees/101
```

or:

```text
PATCH /employees/101
```

---

# 1️⃣4️⃣ UPDATE REQUEST

Example:

```javascript
async function updateEmployee(
    id,
    employee
) {

    const response = await fetch(
        `http://localhost:8000/employees/${id}`,
        {
            method: "PUT",

            headers: {
                "Content-Type":
                    "application/json"
            },

            body: JSON.stringify(employee)
        }
    );

    return response.json();
}
```

---

# 1️⃣5️⃣ UPDATE FLOW

```text
Click Edit
   ↓
Load employee data
   ↓
Show form
   ↓
User changes data
   ↓
Click Update
   ↓
PUT /employees/101
   ↓
Backend
   ↓
Database updated
   ↓
Refresh employee list
```

---

# 1️⃣6️⃣ DELETE 🗑️

Delete means:

> Remove existing data.

Example:

```text
DELETE /employees/101
```

Request:

```javascript
async function deleteEmployee(id) {

    await fetch(
        `http://localhost:8000/employees/${id}`,
        {
            method: "DELETE"
        }
    );
}
```

---

# 1️⃣7️⃣ DELETE FLOW

```text
Click Delete
      ↓
Confirm
      ↓
DELETE /employees/101
      ↓
Backend
      ↓
Database
      ↓
Employee deleted
      ↓
Refresh list
```

---

# 1️⃣8️⃣ CONFIRM BEFORE DELETE ⚠️

Don't immediately delete important data without confirmation.

Example:

```javascript
function handleDelete(id) {

    const confirmed =
        window.confirm(
            "Are you sure you want to delete?"
        );

    if (!confirmed) {
        return;
    }

    deleteEmployee(id);
}
```

---

# 1️⃣9️⃣ LOADING STATE ⏳

API requests take time.

During a request, we should tell the user what is happening.

Create:

```jsx
const [loading, setLoading] =
    useState(false);
```

Then:

```javascript
setLoading(true);

try {

    // API request

} finally {

    setLoading(false);
}
```

Display:

```jsx
{loading && (
    <p>Loading...</p>
)}
```

---

# 2️⃣0️⃣ WHY LOADING STATE?

Without loading state:

```text
User clicks button
       ↓
Nothing happens visually
       ↓
User clicks again
       ↓
Confusion
```

With loading:

```text
User clicks
   ↓
Loading...
   ↓
Response
   ↓
Success
```

---

# 2️⃣1️⃣ ERROR STATE ❌

API requests can fail.

Example:

```text
Backend down
Network problem
Invalid data
Server error
Unauthorized
Not found
```

Create:

```jsx
const [error, setError] =
    useState("");
```

Then:

```javascript
try {

    // request

} catch (err) {

    setError(
        "Something went wrong"
    );
}
```

Display:

```jsx
{error && (
    <p>{error}</p>
)}
```

---

# 2️⃣2️⃣ LOADING + ERROR + DATA

A common React API pattern is:

```jsx
const [data, setData] =
    useState([]);

const [loading, setLoading] =
    useState(false);

const [error, setError] =
    useState("");
```

Think:

```text
DATA
LOADING
ERROR
```

These three states are extremely common in API-driven React applications.

---

# 2️⃣3️⃣ COMPLETE READ PATTERN

```jsx
useEffect(() => {

    async function fetchEmployees() {

        setLoading(true);
        setError("");

        try {

            const response =
                await fetch(
                    "http://localhost:8000/employees"
                );

            if (!response.ok) {

                throw new Error(
                    "Failed to fetch employees"
                );
            }

            const data =
                await response.json();

            setEmployees(data);

        } catch (error) {

            setError(
                error.message
            );

        } finally {

            setLoading(false);
        }
    }

    fetchEmployees();

}, []);
```

---

# 2️⃣4️⃣ WHY `response.ok`?

`fetch()` does not automatically throw an exception for every HTTP error response.

So we often check:

```javascript
if (!response.ok) {
    throw new Error(
        "Request failed"
    );
}
```

Then our `catch` can handle the failure.

---

# 2️⃣5️⃣ FORM VALIDATION 📝

Before creating or updating an employee:

```text
Name
Email
City
```

we validate.

Example:

```javascript
if (!formData.name.trim()) {

    setError(
        "Name is required"
    );

    return;
}
```

Email:

```javascript
if (!formData.email.includes("@")) {

    setError(
        "Enter a valid email"
    );

    return;
}
```

This connects directly to the validation concepts you learned in Chapter 26.

---

# 2️⃣6️⃣ SEARCH 🔍

Suppose we have:

```text
Ramesh
John
Rahul
Robert
Ravi
```

User enters:

```text
Ra
```

We want:

```text
Ramesh
Rahul
Ravi
```

Search means:

> Find records matching the user's search text.

---

# 2️⃣7️⃣ CLIENT-SIDE SEARCH

Example:

```jsx
const [search, setSearch] =
    useState("");
```

Then:

```javascript
const filteredEmployees =
    employees.filter(employee =>
        employee.name
            .toLowerCase()
            .includes(
                search.toLowerCase()
            )
    );
```

Display:

```jsx
{filteredEmployees.map(employee => (
    <p key={employee.id}>
        {employee.name}
    </p>
))}
```

---

# 2️⃣8️⃣ SEARCH FLOW

```text
User types "ra"
        ↓
search state
        ↓
filter()
        ↓
Check employee names
        ↓
Matching employees
        ↓
Display
```

---

# 2️⃣9️⃣ SEARCH INPUT

```jsx
<input
    type="text"
    value={search}
    onChange={(event) =>
        setSearch(event.target.value)
    }
    placeholder="Search employee"
/>
```

---

# 3️⃣0️⃣ FILTER 🔎

Search and filter are related, but not exactly the same.

Example:

```text
Search:
"Ramesh"

Filter:
City = Hyderabad
```

Example UI:

```text
Search: [___________]

City:
[ All ▼ ]
[ Hyderabad ]
[ Bangalore ]
[ Chennai ]
```

---

# 3️⃣1️⃣ FILTER EXAMPLE

```jsx
const [city, setCity] =
    useState("all");
```

Then:

```javascript
const filteredEmployees =
    employees.filter(employee => {

        if (city === "all") {
            return true;
        }

        return employee.city === city;
    });
```

---

# 3️⃣2️⃣ SEARCH + FILTER

We can combine both:

```javascript
const filteredEmployees =
    employees.filter(employee => {

        const matchesSearch =
            employee.name
                .toLowerCase()
                .includes(
                    search.toLowerCase()
                );

        const matchesCity =
            city === "all" ||
            employee.city === city;

        return (
            matchesSearch &&
            matchesCity
        );
    });
```

Flow:

```text
All Employees
      ↓
Search filter
      ↓
City filter
      ↓
Final employees
```

---

# 3️⃣3️⃣ PAGINATION 📄

Imagine the API returns:

```text
1000 employees
```

Should we display all 1000 at once?

Usually, no.

Instead:

```text
Page 1 → 1-10
Page 2 → 11-20
Page 3 → 21-30
...
```

This is pagination.

---

# 3️⃣4️⃣ PAGINATION CONCEPT

Suppose:

```text
Total records = 100
Records per page = 10
```

Then:

```text
Page 1 → 1-10
Page 2 → 11-20
Page 3 → 21-30
...
Page 10 → 91-100
```

---

# 3️⃣5️⃣ PAGINATION STATE

```jsx
const [page, setPage] =
    useState(1);

const [pageSize] =
    useState(10);
```

Then:

```javascript
const startIndex =
    (page - 1) * pageSize;

const endIndex =
    startIndex + pageSize;
```

Example:

```text
page = 2
pageSize = 10

startIndex
= (2 - 1) * 10
= 10

endIndex
= 10 + 10
= 20
```

So we take:

```text
records[10...20]
```

---

# 3️⃣6️⃣ CLIENT-SIDE PAGINATION

```javascript
const paginatedEmployees =
    filteredEmployees.slice(
        startIndex,
        endIndex
    );
```

Then:

```jsx
{paginatedEmployees.map(
    employee => (
        <div key={employee.id}>
            {employee.name}
        </div>
    )
)}
```

---

# 3️⃣7️⃣ PAGINATION BUTTONS

```jsx
<button
    onClick={() =>
        setPage(page - 1)
    }
    disabled={page === 1}
>
    Previous
</button>

<span>
    Page {page}
</span>

<button
    onClick={() =>
        setPage(page + 1)
    }
>
    Next
</button>
```

---

# 3️⃣8️⃣ CLIENT-SIDE vs SERVER-SIDE PAGINATION

### Client-side

Backend sends many/all records.

```text
Backend
 ↓
1000 records
 ↓
React
 ↓
Pagination
```

### Server-side

React requests only the required page.

```text
React
 ↓
GET /employees?page=2&limit=10
 ↓
Backend
 ↓
Database
 ↓
10 records
 ↓
React
```

For large datasets, server-side pagination is often more appropriate.

---

# 3️⃣9️⃣ COMPLETE CRUD FLOW 🚀

Now combine everything:

```text
                 CRUD APP
                    │
       ┌────────────┼────────────┐
       ↓            ↓            ↓
    CREATE         READ        UPDATE
       │            │            │
       └────────────┼────────────┘
                    ↓
                  DELETE
                    │
        ┌───────────┼───────────┐
        ↓           ↓           ↓
      SEARCH      FILTER    PAGINATION
                    │
                    ↓
             FORM VALIDATION
                    │
                    ↓
            LOADING / ERROR
                    │
                    ↓
                   API
                    │
                    ↓
                 Backend
                    │
                    ↓
                 Database
```

---

# 4️⃣0️⃣ RECOMMENDED FOLDER STRUCTURE 📁

For a beginner CRUD application:

```text
src/
│
├── components/
│   ├── EmployeeForm.jsx
│   ├── EmployeeList.jsx
│   ├── EmployeeItem.jsx
│   ├── SearchBar.jsx
│   └── Pagination.jsx
│
├── services/
│   └── employeeApi.js
│
├── pages/
│   └── Employees.jsx
│
├── App.jsx
├── main.jsx
└── index.css
```

---

# 4️⃣1️⃣ API SERVICE FILE

Instead of putting every API request inside components, we can create:

```text
services/
└── employeeApi.js
```

Example:

```javascript
const API_URL =
    "http://localhost:8000/employees";

export async function getEmployees() {

    const response =
        await fetch(API_URL);

    if (!response.ok) {
        throw new Error(
            "Failed to fetch employees"
        );
    }

    return response.json();
}
```

Create:

```javascript
export async function createEmployee(
    employee
) {

    const response =
        await fetch(API_URL, {

            method: "POST",

            headers: {
                "Content-Type":
                    "application/json"
            },

            body:
                JSON.stringify(employee)
        });

    if (!response.ok) {
        throw new Error(
            "Failed to create employee"
        );
    }

    return response.json();
}
```

---

# 4️⃣2️⃣ WHY API SERVICE FILE?

Without service file:

```text
Component
 ├── GET
 ├── POST
 ├── PUT
 ├── DELETE
 └── lots of code
```

With service file:

```text
Component
    ↓
API Service
    ↓
Backend
```

This makes the application easier to maintain.

---

# 4️⃣3️⃣ COMPLETE CRUD SERVICE

```javascript
const API_URL =
    "http://localhost:8000/employees";

export async function getEmployees() {

    const response =
        await fetch(API_URL);

    if (!response.ok) {
        throw new Error(
            "Failed to fetch employees"
        );
    }

    return response.json();
}

export async function createEmployee(
    employee
) {

    const response =
        await fetch(API_URL, {

            method: "POST",

            headers: {
                "Content-Type":
                    "application/json"
            },

            body:
                JSON.stringify(employee)
        });

    if (!response.ok) {
        throw new Error(
            "Failed to create employee"
        );
    }

    return response.json();
}

export async function updateEmployee(
    id,
    employee
) {

    const response =
        await fetch(
            `${API_URL}/${id}`,
            {
                method: "PUT",

                headers: {
                    "Content-Type":
                        "application/json"
                },

                body:
                    JSON.stringify(employee)
            }
        );

    if (!response.ok) {
        throw new Error(
            "Failed to update employee"
        );
    }

    return response.json();
}

export async function deleteEmployee(id) {

    const response =
        await fetch(
            `${API_URL}/${id}`,
            {
                method: "DELETE"
            }
        );

    if (!response.ok) {
        throw new Error(
            "Failed to delete employee"
        );
    }
}
```

---

# 4️⃣4️⃣ COMPONENT → SERVICE → API

Remember this architecture:

```text
EmployeeList.jsx
       ↓
getEmployees()
       ↓
employeeApi.js
       ↓
fetch()
       ↓
Backend API
       ↓
Database
```

---

# 4️⃣5️⃣ AFTER CREATE — WHAT SHOULD HAPPEN?

Suppose user creates:

```text
Ramesh
```

Backend successfully creates it.

Now React should update the list.

Two common approaches:

### Approach 1

Fetch all employees again:

```text
POST
 ↓
Success
 ↓
GET employees
 ↓
Update list
```

### Approach 2

Add the returned new employee directly to state:

```text
POST
 ↓
New employee returned
 ↓
setEmployees(...)
```

For beginners, refetching can be easier to understand.

---

# 4️⃣6️⃣ AFTER UPDATE

```text
PUT
 ↓
Success
 ↓
GET employees
 ↓
Display updated list
```

---

# 4️⃣7️⃣ AFTER DELETE

```text
DELETE
 ↓
Success
 ↓
GET employees
 ↓
Display updated list
```

Later, you can optimize these operations by updating local state directly.

---

# 4️⃣8️⃣ COMMON MISTAKES ❌

## Mistake 1 — No `key` in `.map()`

Wrong:

```jsx
employees.map(employee => (
    <p>
        {employee.name}
    </p>
))
```

Better:

```jsx
employees.map(employee => (
    <p key={employee.id}>
        {employee.name}
    </p>
))
```

---

## Mistake 2 — Not checking API errors

Don't assume:

```text
API always succeeds
```

Always handle failure.

---

## Mistake 3 — Forgetting loading state

Users need feedback during API operations.

---

## Mistake 4 — Delete without confirmation

For important data, confirm before deletion.

---

## Mistake 5 — Mixing API code everywhere

Prefer an API service layer for larger applications.

---

## Mistake 6 — Not refreshing data after mutation

After create/update/delete, make sure the UI reflects the latest data.

---

## Mistake 7 — Search doesn't reset pagination

Suppose:

```text
Current page = 5
```

Then user searches:

```text
Ramesh
```

Only 2 results exist.

Page 5 may now be empty.

A good pattern is:

```javascript
setSearch(value);
setPage(1);
```

when search criteria changes.

---

# 4️⃣9️⃣ BEST PRACTICES ⭐

### ✅ Separate UI and API logic

```text
components/
services/
```

### ✅ Keep API functions reusable

```text
getEmployees()
createEmployee()
updateEmployee()
deleteEmployee()
```

### ✅ Show loading states

```text
Loading...
```

### ✅ Show meaningful errors

```text
Failed to load employees.
Please try again.
```

### ✅ Validate forms

Before sending data to the backend.

### ✅ Confirm destructive operations

```text
Are you sure?
```

### ✅ Keep one source of truth

Don't maintain the same employee list in many unrelated states.

---

# 5️⃣0️⃣ REAL-WORLD CRUD ARCHITECTURE

```text
                    USER
                     │
                     ↓
               React UI
                     │
       ┌─────────────┼─────────────┐
       ↓             ↓             ↓
      Form          List        Search
       │             │             │
       └─────────────┼─────────────┘
                     ↓
               React State
                     │
                     ↓
               API Service
                     │
                     ↓
                  REST API
                     │
                     ↓
                  Backend
                     │
                     ↓
                  Database
```

---

# 5️⃣1️⃣ CRUD DRY RUN 🧠

Let's add employee **Ramesh**.

### CREATE

```text
Fill form
 ↓
Click Add
 ↓
Validation
 ↓
POST
 ↓
Backend
 ↓
Database
 ↓
Success
```

### READ

```text
GET
 ↓
Backend
 ↓
Database
 ↓
Employees
 ↓
React
 ↓
Display
```

### UPDATE

```text
Click Edit
 ↓
Load data
 ↓
Change data
 ↓
PUT
 ↓
Backend
 ↓
Database
 ↓
Updated
```

### DELETE

```text
Click Delete
 ↓
Confirm
 ↓
DELETE
 ↓
Backend
 ↓
Database
 ↓
Removed
```

---

# 5️⃣2️⃣ INTERVIEW QUESTIONS 🎤

## Q1. What does CRUD mean?

**Answer:**

```text
C → Create
R → Read
U → Update
D → Delete
```

---

## Q2. Which HTTP method is commonly used for Create?

**Answer:**

```text
POST
```

---

## Q3. Which HTTP method is commonly used for Read?

**Answer:**

```text
GET
```

---

## Q4. Which methods can be used for Update?

**Answer:**

```text
PUT
PATCH
```

---

## Q5. Which method is commonly used for Delete?

**Answer:**

```text
DELETE
```

---

## Q6. Why do we use loading state?

**Answer:**  
To give the user feedback while an asynchronous operation is in progress.

---

## Q7. Why do we need error state?

**Answer:**  
To store and display information when an API or application operation fails.

---

## Q8. Why use an API service file?

**Answer:**  
To separate API communication from UI components and make API functions reusable and easier to maintain.

---

## Q9. What is client-side pagination?

**Answer:**  
The application receives a collection of records and divides the displayed records into pages in the frontend.

---

## Q10. What is server-side pagination?

**Answer:**  
The frontend requests only the required page of data from the backend.

---

## Q11. Search vs filter?

**Answer:**

```text
Search
→ Find matching text/records

Filter
→ Restrict records based on conditions
```

---

## Q12. Why should delete operations often require confirmation?

**Answer:**  
Because deletion is a destructive action and confirmation helps prevent accidental removal.

---

# 5️⃣3️⃣ PRACTICE TASKS 📝

## Task 1 — Product CRUD

Create:

```text
Product Name
Price
Category
Quantity
```

Operations:

```text
Create
Read
Update
Delete
```

---

## Task 2 — Employee Search

Create:

```text
Search Employee
```

Search by:

```text
Name
Email
```

---

## Task 3 — Employee Filter

Filter by:

```text
Hyderabad
Bangalore
Chennai
```

---

## Task 4 — Pagination

Display:

```text
5 employees per page
```

Buttons:

```text
Previous
Next
```

---

## Task 5 — Loading + Error

Show:

```text
Loading...
```

while fetching.

Show:

```text
Failed to load employees
```

when the API fails.

---

# 5️⃣4️⃣ MINI PROJECT 🚀

# 👨‍💼 EMPLOYEE MANAGEMENT SYSTEM

Build a complete application.

### Fields

```text
Name
Email
Phone
City
Department
Salary
```

### Features

```text
➕ Create employee

👀 Read employees

✏️ Update employee

🗑️ Delete employee

🔍 Search employee

🔎 Filter by department

📄 Pagination

📝 Form validation

⏳ Loading state

❌ Error state
```

### Suggested structure

```text
src/
│
├── components/
│   ├── EmployeeForm.jsx
│   ├── EmployeeList.jsx
│   ├── EmployeeItem.jsx
│   ├── SearchBar.jsx
│   ├── Filter.jsx
│   └── Pagination.jsx
│
├── services/
│   └── employeeApi.js
│
├── pages/
│   └── Employees.jsx
│
├── App.jsx
└── main.jsx
```

---

# 5️⃣5️⃣ QUIZ 🧠

### Q1. What does C in CRUD mean?

**Answer:**  
Create.

---

### Q2. What HTTP method is commonly used for Create?

**Answer:**  
POST.

---

### Q3. What HTTP method is commonly used for Read?

**Answer:**  
GET.

---

### Q4. What HTTP methods are commonly used for Update?

**Answer:**  
PUT and PATCH.

---

### Q5. What HTTP method is commonly used for Delete?

**Answer:**  
DELETE.

---

### Q6. Why do we use `useEffect` in many CRUD applications?

**Answer:**  
To perform side effects such as fetching initial data from an API.

---

### Q7. What does `JSON.stringify()` do?

**Answer:**  
It converts a JavaScript value, commonly an object, into a JSON string.

---

### Q8. What is search?

**Answer:**  
Finding records that match a search condition, commonly text entered by the user.

---

### Q9. What is pagination?

**Answer:**  
Dividing a large collection of records into smaller pages.

---

### Q10. What three common UI states should API-driven components handle?

**Answer:**

```text
Loading
Success/Data
Error
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
          CRUD
           │
     ┌─────┼─────┐
     ↓     ↓     ↓
 CREATE   READ   UPDATE
   POST    GET   PUT/PATCH
           │
           ↓
         DELETE
         DELETE
```

Then remember the complete application:

```text
📝 FORM
   ↓
✅ VALIDATION
   ↓
🌐 API
   ↓
⏳ LOADING
   ↓
📦 DATA
   ↓
🔍 SEARCH
   ↓
🔎 FILTER
   ↓
📄 PAGINATION
   ↓
❌ ERROR HANDLING
```

And the most important architecture:

```text
React Component
       ↓
API Service
       ↓
REST API
       ↓
Backend
       ↓
Database
```

---

# ✅ CHAPTER 28 CHECKLIST

Before moving forward, make sure you understand:

- [ ] CRUD
- [ ] Create
- [ ] Read
- [ ] Update
- [ ] Delete
- [ ] POST
- [ ] GET
- [ ] PUT
- [ ] PATCH
- [ ] DELETE
- [ ] API integration
- [ ] Loading state
- [ ] Error state
- [ ] Form validation
- [ ] Search
- [ ] Filter
- [ ] Pagination
- [ ] API service files
- [ ] CRUD architecture
- [ ] Client-side pagination
- [ ] Server-side pagination

# 🎉 CHAPTER 28 COMPLETE

This chapter is especially important for **real-world React development and interviews** because CRUD applications combine many React fundamentals into one practical workflow.

Your source syllabus now moves to:

# ⏭️ CHAPTER 29 — CSS WITH REACT 🎨

Topics:

```text
CSS Files
Inline CSS
CSS Modules
Dynamic Styles
Conditional Classes
Responsive Design
```

These are the exact Chapter 29 topics in your course plan.

Type **`NEXT`** when you are ready.