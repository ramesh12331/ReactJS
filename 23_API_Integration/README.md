
# 🌐 CHAPTER 23 — API INTEGRATION

In the previous chapters, we learned React itself.

Now we are entering a **very important real-world topic**:

> 🔥 **How applications communicate with backend servers.**

A React application by itself usually displays UI.

But real applications need data such as:

```text
👤 Users
🛍️ Products
📦 Orders
💰 Payments
📚 Courses
🎬 Movies
🌦️ Weather
```

That data usually comes from a **backend API**.

Your course specifically places API Integration before the next chapter, **API + React**, where we will actually fetch data inside React and manage loading/error/success states.

---

# 📚 WHAT WE WILL LEARN

1. What is an API?
2. Client and Server
3. What is REST API?
4. HTTP / HTTPS
5. HTTP methods
6. GET
7. POST
8. PUT
9. PATCH
10. DELETE
11. CRUD
12. Request
13. Response
14. URL
15. Route Parameters
16. Query Parameters
17. Headers
18. Body
19. JSON
20. Status Codes
21. `fetch()`
22. Axios
23. `fetch()` vs Axios
24. Complete API flow
25. Common mistakes
26. Best practices
27. Interview Questions
28. Practice
29. Mini Challenge
30. Quiz
31. Final Revision

---

# 1️⃣ WHAT IS AN API?

## 📖 Simple Definition

**API = Application Programming Interface**

An API allows two applications to communicate with each other.

For example:

```text
React Application
       ↓
      API
       ↓
Backend Server
       ↓
   Database
```

Your course describes an API as a communication layer between applications.

---

# 2️⃣ REAL-WORLD ANALOGY 🍽️

Imagine you go to a restaurant.

```text
You
 ↓
Waiter
 ↓
Kitchen
```

You don't normally walk directly into the kitchen and prepare your food.

Instead:

```text
You
 ↓
Order
 ↓
Waiter
 ↓
Kitchen
 ↓
Food
 ↓
Waiter
 ↓
You
```

The **waiter** is similar to an API.

In software:

```text
React
 ↓
API Request
 ↓
Backend
 ↓
Database
 ↓
API Response
 ↓
React
```

---

# 3️⃣ CLIENT AND SERVER

There are two important sides.

## 🖥️ Client

The client is usually the application the user interacts with.

Examples:

```text
React
Browser
Mobile App
```

## 🖥️ Server

The server handles application logic and data.

Examples:

```text
FastAPI
Node.js
Express
Django
```

---

# 4️⃣ COMPLETE CLIENT-SERVER FLOW

```text
┌─────────────────┐
│  React Frontend │
│     Client      │
└────────┬────────┘
         │
         │ Request
         ↓
┌─────────────────┐
│  Backend API    │
│     Server      │
└────────┬────────┘
         │
         │ Database Query
         ↓
┌─────────────────┐
│    Database     │
└────────┬────────┘
         │
         │ Data
         ↓
┌─────────────────┐
│  Backend API    │
└────────┬────────┘
         │
         │ Response
         ↓
┌─────────────────┐
│  React Frontend │
└─────────────────┘
```

This request/response flow is also reflected in your API notes.

---

# 5️⃣ WHAT IS A REST API?

REST means:

> **Representational State Transfer**

A REST API is an API style that uses HTTP requests and resources.

For example:

```text
/users
/products
/orders
/customers
```

A REST-style API commonly uses HTTP methods to perform operations on these resources.

---

# 6️⃣ HTTP AND HTTPS

## HTTP

HTTP means:

> **HyperText Transfer Protocol**

It is used for communication between client and server.

## HTTPS

HTTPS is HTTP communication secured using TLS.

```text
HTTP
 ↓
Communication

HTTPS
 ↓
Secure communication
```

Your notes identify HTTPS as secure HTTP communication.

---

# 7️⃣ HTTP METHODS

This is one of the most important API concepts.

Remember:

```text
C → Create → POST
R → Read   → GET
U → Update → PUT / PATCH
D → Delete → DELETE
```

Your course uses this exact CRUD mapping.

---

# 8️⃣ GET

## 📖 Meaning

`GET` is used to **retrieve/read data**.

Example:

```http
GET /users
```

Meaning:

> Give me the users.

Another:

```http
GET /products
```

Meaning:

> Give me the products.

---

# 9️⃣ GET Example

Suppose the server returns:

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

React can receive this data.

Flow:

```text
React
 ↓
GET /products
 ↓
Backend
 ↓
Database
 ↓
Products
 ↓
React
```

---

# 🔟 POST

`POST` is used to **create new data**.

Example:

```http
POST /users
```

Suppose we want to create:

```json
{
    "name": "Ramesh",
    "city": "Hyderabad"
}
```

The frontend sends this data to the server.

---

# 1️⃣1️⃣ PUT

`PUT` is used to update an existing resource.

Example:

```http
PUT /users/10
```

Suppose user 10 is:

```json
{
    "name": "Ramesh",
    "city": "Hyderabad"
}
```

We might send updated information:

```json
{
    "name": "Ramesh Kumar",
    "city": "Hyderabad"
}
```

---

# 1️⃣2️⃣ PATCH

`PATCH` is generally used for a **partial update**.

Suppose the user has:

```json
{
    "name": "Ramesh",
    "age": 30,
    "city": "Hyderabad"
}
```

We only want to change the city.

```http
PATCH /users/10
```

Body:

```json
{
    "city": "Bangalore"
}
```

Only part of the resource is changed.

---

# 1️⃣3️⃣ PUT vs PATCH

| PUT | PATCH |
|---|---|
| Update/replace resource | Partial update |
| Often sends the complete representation | Sends only changed fields |
| `PUT /users/10` | `PATCH /users/10` |

🧠 Remember:

```text
PUT
 ↓
Full Update / Replace

PATCH
 ↓
Partial Update
```

---

# 1️⃣4️⃣ DELETE

`DELETE` removes data.

Example:

```http
DELETE /users/10
```

Meaning:

> Delete user 10.

Flow:

```text
React
 ↓
DELETE /users/10
 ↓
Backend
 ↓
Database
 ↓
User deleted
```

---

# 1️⃣5️⃣ CRUD

CRUD is extremely important.

```text
C = Create
R = Read
U = Update
D = Delete
```

Mapping:

```text
Create → POST
Read   → GET
Update → PUT / PATCH
Delete → DELETE
```

Your source explicitly uses this mapping.

---

# 1️⃣6️⃣ WHAT IS A REQUEST?

A **request** is sent by the client to the server.

Example:

```text
React
 ↓
Request
 ↓
Backend
```

A request commonly contains:

```text
1. URL
2. Headers
3. Body
```

Your notes describe these three main parts.

---

# 1️⃣7️⃣ REQUEST STRUCTURE

```text
Request
│
├── URL
│   ├── Route Parameters
│   └── Query Parameters
│
├── Headers
│
└── Body
```

---

# 1️⃣8️⃣ WHAT IS A RESPONSE?

After receiving a request, the server sends a response.

```text
React
 ↓
Request
 ↓
Server
 ↓
Response
 ↓
React
```

A response commonly contains:

```text
Status Code
Headers
Body
```

Your source shows this structure explicitly.

---

# 1️⃣9️⃣ REQUEST vs RESPONSE

| Request | Response |
|---|---|
| Client → Server | Server → Client |
| Sent by frontend | Sent by backend |
| URL | Status code |
| Headers | Headers |
| Body | Body |

Memory trick:

```text
REQUEST
   ↓
Ask

RESPONSE
   ↓
Answer
```

---

# 2️⃣0️⃣ WHAT IS A URL?

Example:

```text
http://localhost:3000/api/users
```

Break it down:

```text
http
 ↓
Protocol

localhost
 ↓
Computer / Host

3000
 ↓
Port

/api/users
 ↓
Resource path
```

---

# 2️⃣1️⃣ URL WITH PARAMETERS

Consider:

```text
http://localhost:3000/api/users/100
```

Here:

```text
/users
 ↓
Resource

/100
 ↓
Route Parameter
```

So:

```text
/users/100
```

means:

> User with ID 100.

---

# 2️⃣2️⃣ ROUTE PARAMETERS

A route parameter is a value directly inside the URL path.

Example:

```text
/users/100
```

Here:

```text
100
```

is the route parameter.

Another example:

```text
/users/100/orders/500
```

Here:

```text
100 → User ID
500 → Order ID
```

Your source describes route parameters as values included directly inside the URL path.

---

# 2️⃣3️⃣ QUERY PARAMETERS

Query parameters are generally written after:

```text
?
```

Example:

```text
/users?city=Hyderabad
```

Here:

```text
city=Hyderabad
```

is a query parameter.

Multiple parameters:

```text
/users?city=Hyderabad&age=30
```

There are two:

```text
city = Hyderabad
age  = 30
```

Your API notes describe query parameters as values passed after `?`.

---

# 2️⃣4️⃣ ROUTE PARAMETER vs QUERY PARAMETER

### Route Parameter

```text
/users/100
```

Usually identifies a specific resource.

### Query Parameter

```text
/users?city=Hyderabad
```

Often used for filtering, searching, sorting, pagination, or optional values.

Memory:

```text
/users/100
       ↑
       ID

/users?city=Hyderabad
       ↑
       Filter
```

---

# 2️⃣5️⃣ HEADERS

Headers contain additional information about a request or response.

Example:

```http
Content-Type: application/json
```

Common headers include:

```text
Authorization
Content-Type
Accept
User-Agent
```

Your source lists these as common header examples.

---

# 2️⃣6️⃣ CONTENT-TYPE

One important header is:

```text
Content-Type
```

For JSON:

```http
Content-Type: application/json
```

It tells the server that the request body contains JSON data.

---

# 2️⃣7️⃣ REQUEST BODY

The body contains the actual data being sent.

Example:

```json
{
    "name": "Ramesh",
    "age": 30
}
```

Body is commonly used with:

```text
POST
PUT
PATCH
```

Your notes identify the request body as actual information and associate it with these methods.

---

# 2️⃣8️⃣ WHAT IS JSON?

JSON means:

> **JavaScript Object Notation**

It is a lightweight format commonly used for exchanging structured data between applications.

Example:

```json
{
    "name": "Ramesh",
    "age": 30,
    "city": "Hyderabad"
}
```

JSON uses:

```text
key : value
```

---

# 2️⃣9️⃣ JSON ARRAY

We can also receive an array:

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

This is very common for:

```text
GET /products
```

---

# 3️⃣0️⃣ HTTP STATUS CODES

When the server responds, it usually gives a status code.

Important ones:

| Code | Meaning |
|---|---|
| `200` | OK / Success |
| `201` | Created |
| `204` | Success, No Content |
| `400` | Bad Request |
| `401` | Unauthorized |
| `403` | Forbidden |
| `404` | Not Found |
| `409` | Conflict |
| `500` | Internal Server Error |

Your API notes include these common status codes and meanings.

---

# 3️⃣1️⃣ STATUS CODE GROUPS

A simple way to remember:

```text
2xx → Success
4xx → Client-side/request problem
5xx → Server-side problem
```

Examples:

```text
200 → Success
201 → Created

400 → Bad Request
401 → Unauthorized
403 → Forbidden
404 → Not Found

500 → Server Error
```

---

# 3️⃣2️⃣ WHAT IS `fetch()`?

JavaScript provides:

```javascript
fetch()
```

for making HTTP requests.

Basic syntax:

```javascript
fetch("URL")
```

Example:

```javascript
fetch("https://example.com/users");
```

---

# 3️⃣3️⃣ Basic `fetch()` Example

```javascript
fetch("https://example.com/users")
    .then(response => response.json())
    .then(data => {
        console.log(data);
    });
```

Let's understand it.

### Step 1

```javascript
fetch("https://example.com/users")
```

Send request.

### Step 2

```javascript
.then(response => response.json())
```

Convert the response body into JavaScript data.

### Step 3

```javascript
.then(data => {
    console.log(data);
})
```

Use the data.

---

# 3️⃣4️⃣ `fetch()` with async/await

Modern JavaScript commonly uses:

```javascript
async/await
```

Example:

```javascript
async function getUsers() {

    const response = await fetch(
        "https://example.com/users"
    );

    const data = await response.json();

    console.log(data);
}
```

Flow:

```text
fetch()
 ↓
response
 ↓
response.json()
 ↓
data
```

---

# 3️⃣5️⃣ GET Request with fetch

```javascript
async function getUsers() {

    const response = await fetch(
        "https://example.com/users"
    );

    const data = await response.json();

    return data;
}
```

This performs a GET request because GET is the default HTTP method for `fetch()`.

---

# 3️⃣6️⃣ POST Request with fetch

For POST:

```javascript
async function createUser() {

    const response = await fetch(
        "https://example.com/users",
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify({
                name: "Ramesh",
                city: "Hyderabad"
            })
        }
    );

    const data = await response.json();

    return data;
}
```

Important parts:

```javascript
method: "POST"
```

means create request.

```javascript
headers
```

describes the data.

```javascript
body
```

contains the actual data.

---

# 3️⃣7️⃣ Why `JSON.stringify()`?

Our JavaScript object:

```javascript
{
    name: "Ramesh",
    city: "Hyderabad"
}
```

needs to be converted into JSON text before sending it as a JSON request body.

We use:

```javascript
JSON.stringify()
```

Example:

```javascript
const user = {
    name: "Ramesh",
    city: "Hyderabad"
};

const jsonData = JSON.stringify(user);
```

---

# 3️⃣8️⃣ JSON.parse()

The opposite operation is:

```javascript
JSON.parse()
```

Concept:

```text
JavaScript Object
       ↓
JSON.stringify()
       ↓
JSON text
```

And:

```text
JSON text
       ↓
JSON.parse()
       ↓
JavaScript Object
```

With `fetch()`, `response.json()` is commonly used to parse a JSON response.

---

# 3️⃣9️⃣ What is Axios?

Axios is a popular HTTP client library used to make requests.

Example:

```javascript
axios.get("/users");
```

Instead of:

```javascript
fetch("/users");
```

Axios provides its own API for HTTP requests.

---

# 4️⃣0️⃣ Installing Axios

Inside your React project:

```bash
npm install axios
```

Then:

```javascript
import axios from "axios";
```

---

# 4️⃣1️⃣ Axios GET

```javascript
import axios from "axios";

async function getUsers() {

    const response = await axios.get(
        "https://example.com/users"
    );

    console.log(response.data);
}
```

Notice:

```javascript
response.data
```

Axios provides the response data through the `data` property.

---

# 4️⃣2️⃣ Axios POST

```javascript
import axios from "axios";

async function createUser() {

    const response = await axios.post(
        "https://example.com/users",
        {
            name: "Ramesh",
            city: "Hyderabad"
        }
    );

    console.log(response.data);
}
```

This is convenient because Axios handles JSON request/response details for you in common cases.

---

# 4️⃣3️⃣ Axios PUT

```javascript
await axios.put(
    "https://example.com/users/10",
    {
        name: "Ramesh Kumar",
        city: "Hyderabad"
    }
);
```

---

# 4️⃣4️⃣ Axios PATCH

```javascript
await axios.patch(
    "https://example.com/users/10",
    {
        city: "Bangalore"
    }
);
```

---

# 4️⃣5️⃣ Axios DELETE

```javascript
await axios.delete(
    "https://example.com/users/10"
);
```

---

# 4️⃣6️⃣ fetch vs Axios

| fetch | Axios |
|---|---|
| Built into modern browsers | External package |
| No installation needed | Install Axios |
| More manual handling | Convenient API |
| `response.json()` commonly needed | Response data available via `response.data` |
| Good for simple requests | Popular for larger applications |

🧠 Don't think:

> Axios is always better.

Instead:

> Both can communicate with APIs. Choose based on project requirements and team conventions.

---

# 4️⃣7️⃣ COMPLETE API FLOW

Let's imagine:

```text
React
```

needs products.

### Step 1

React sends:

```http
GET /products
```

### Step 2

Backend receives request.

### Step 3

Backend asks database:

```text
Give me products.
```

### Step 4

Database returns:

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

### Step 5

Backend sends response.

### Step 6

React receives the response.

```text
React
 ↓
API
 ↓
Backend
 ↓
Database
 ↓
Backend
 ↓
API Response
 ↓
React
 ↓
UI
```

---

# 4️⃣8️⃣ REAL-WORLD EXAMPLE — PRODUCT API 🛍️

Suppose we have:

```text
/products
```

### GET

```http
GET /products
```

Get all products.

### GET One

```http
GET /products/10
```

Get product 10.

### POST

```http
POST /products
```

Create product.

### PUT

```http
PUT /products/10
```

Update product 10.

### PATCH

```http
PATCH /products/10
```

Partially update product 10.

### DELETE

```http
DELETE /products/10
```

Delete product 10.

---

# 4️⃣9️⃣ API TABLE 🧠

| Operation | Method | Example |
|---|---|---|
| Create | POST | `/products` |
| Read all | GET | `/products` |
| Read one | GET | `/products/10` |
| Update | PUT | `/products/10` |
| Partial Update | PATCH | `/products/10` |
| Delete | DELETE | `/products/10` |

Memorize this table.

---

# 5️⃣0️⃣ PRACTICAL API TESTING WITH POSTMAN

Before connecting React to an API, developers often test APIs independently.

For example:

```text
POSTMAN
   ↓
GET
   ↓
API URL
   ↓
Send
   ↓
Response
```

This helps determine whether the backend API works correctly before debugging React.

---

# 5️⃣1️⃣ Example API Request

Suppose:

```text
GET
```

URL:

```text
http://localhost:8000/products
```

Postman sends:

```text
GET /products
```

Server may respond:

```text
200 OK
```

with:

```json
[
    {
        "id": 1,
        "name": "Laptop"
    }
]
```

---

# 5️⃣2️⃣ React + FastAPI Connection

This is especially useful for you because you have already learned FastAPI.

Architecture:

```text
┌──────────────┐
│    React     │
│  Frontend    │
└──────┬───────┘
       │
       │ HTTP Request
       ↓
┌──────────────┐
│   FastAPI    │
│   Backend    │
└──────┬───────┘
       │
       │ SQL
       ↓
┌──────────────┐
│ PostgreSQL   │
└──────────────┘
```

This will become very important in later chapters.

---

# 5️⃣3️⃣ COMMON MISTAKES ❌

## Mistake 1 — Forgetting to convert JSON response

With fetch:

```javascript
const response = await fetch(url);

console.log(response);
```

This is not the actual parsed JSON data.

Usually:

```javascript
const data = await response.json();
```

Then:

```javascript
console.log(data);
```

---

# 5️⃣4️⃣ Mistake 2 — Forgetting `JSON.stringify()`

Wrong:

```javascript
body: {
    name: "Ramesh"
}
```

When sending JSON with fetch, commonly use:

```javascript
body: JSON.stringify({
    name: "Ramesh"
})
```

---

# 5️⃣5️⃣ Mistake 3 — Wrong Content-Type

When sending JSON:

```javascript
headers: {
    "Content-Type": "application/json"
}
```

---

# 5️⃣6️⃣ Mistake 4 — Confusing PUT and PATCH

Remember:

```text
PUT
 ↓
Update/replace

PATCH
 ↓
Partial update
```

---

# 5️⃣7️⃣ Mistake 5 — Ignoring Status Codes

Don't assume every request succeeded.

A response might be:

```text
200
```

or:

```text
404
```

or:

```text
500
```

You should understand the response before using the data.

---

# 5️⃣8️⃣ BEST PRACTICES ✅

### 1. Keep API URLs organized

Instead of writing URLs everywhere:

```javascript
fetch("http://localhost:8000/products");
```

large applications commonly centralize API configuration.

---

### 2. Handle errors

Don't only write:

```javascript
const response = await fetch(url);
```

Think about:

```text
Success
Failure
Network error
Server error
```

---

### 3. Use meaningful HTTP methods

```text
Create → POST
Read → GET
Update → PUT/PATCH
Delete → DELETE
```

---

### 4. Validate backend responses

Don't blindly assume:

```javascript
data.products
```

exists.

Understand the API response structure.

---

### 5. Keep API logic reusable

Later we will create structures such as:

```text
src/
├── api/
│   └── productApi.js
│
├── components/
├── pages/
└── App.jsx
```

Chapter 24 will focus more deeply on API + React and reusable API functions.

---

# 5️⃣9️⃣ INTERVIEW QUESTIONS 🎯

## Beginner

### Q1. What is an API?

**Answer:**

API stands for Application Programming Interface. It provides a way for applications to communicate with each other.

---

### Q2. What is REST API?

**Answer:**

A REST API is an API style that commonly uses HTTP methods to work with resources.

---

### Q3. What is GET?

**Answer:**

GET is used to retrieve/read data.

---

### Q4. What is POST?

**Answer:**

POST is commonly used to create new data.

---

### Q5. What is DELETE?

**Answer:**

DELETE is used to remove a resource.

---

# 6️⃣0️⃣ INTERMEDIATE INTERVIEW QUESTIONS

### Q6. PUT vs PATCH?

**Answer:**

PUT is generally used for updating/replacing a resource, while PATCH is generally used for partial updates.

---

### Q7. What is a request?

**Answer:**

A request is information sent from the client to the server.

It can contain:

```text
URL
Headers
Body
```

---

### Q8. What is a response?

**Answer:**

A response is information returned from the server to the client.

It commonly contains:

```text
Status Code
Headers
Body
```

---

### Q9. What is JSON?

**Answer:**

JSON is a lightweight format commonly used for exchanging structured data between applications.

---

### Q10. What is a header?

**Answer:**

A header contains additional information or metadata about an HTTP request or response.

---

# 6️⃣1️⃣ ADVANCED INTERVIEW QUESTIONS

### Q11. What is the difference between route parameters and query parameters?

**Answer:**

Route parameters are part of the URL path:

```text
/users/100
```

Query parameters appear after `?`:

```text
/users?city=Hyderabad
```

---

### Q12. What is `fetch()`?

**Answer:**

`fetch()` is a browser API used to make HTTP requests.

---

### Q13. What is Axios?

**Answer:**

Axios is an HTTP client library used to make HTTP requests from JavaScript applications.

---

### Q14. What is `JSON.stringify()`?

**Answer:**

It converts a JavaScript value into a JSON string.

---

### Q15. What is `response.json()`?

**Answer:**

With fetch, it reads and parses a JSON response body.

---

# 6️⃣2️⃣ PRACTICE QUESTIONS 🧪

### Task 1

Explain:

```text
GET
POST
PUT
PATCH
DELETE
```

in your own words.

---

### Task 2

Identify the route parameter:

```text
/users/100
```

Answer:

```text
100
```

---

### Task 3

Identify the query parameters:

```text
/products?category=mobile&price=50000
```

Answer:

```text
category = mobile
price = 50000
```

---

### Task 4

Write a GET request using fetch.

---

### Task 5

Write a POST request using fetch.

---

### Task 6

Write an Axios GET request.

---

### Task 7

Write an Axios DELETE request.

---

### Task 8

Explain:

```text
200
201
400
401
403
404
500
```

---

# 6️⃣3️⃣ MINI CHALLENGE 🚀

Imagine you are building a **Book Store API**.

Create the following API design:

```text
/books
```

Requirements:

```text
GET    /books
GET    /books/10
POST   /books
PUT    /books/10
PATCH  /books/10
DELETE /books/10
```

Now answer:

### 1. Which method creates a book?

### 2. Which method gets all books?

### 3. Which method gets book 10?

### 4. Which method updates book 10?

### 5. Which method partially updates book 10?

### 6. Which method deletes book 10?

Try answering yourself before checking the answers below.

---

# 6️⃣4️⃣ MINI CHALLENGE ANSWERS

```text
Create
→ POST /books

Read all
→ GET /books

Read one
→ GET /books/10

Update
→ PUT /books/10

Partial Update
→ PATCH /books/10

Delete
→ DELETE /books/10
```

---

# 6️⃣5️⃣ QUIZ 🧠

### Q1. What does API stand for?

**Answer:** Application Programming Interface.

---

### Q2. Which HTTP method is generally used to read data?

**Answer:** GET.

---

### Q3. Which method is generally used to create data?

**Answer:** POST.

---

### Q4. Which methods can be used for updates?

**Answer:** PUT and PATCH.

---

### Q5. Which method deletes data?

**Answer:** DELETE.

---

### Q6. What does CRUD stand for?

**Answer:**

```text
Create
Read
Update
Delete
```

---

### Q7. What is this?

```text
/users/100
```

**Answer:** `100` is a route parameter.

---

### Q8. What is this?

```text
/users?city=Hyderabad
```

**Answer:** `city=Hyderabad` is a query parameter.

---

### Q9. What does `JSON.stringify()` do?

**Answer:** Converts a JavaScript value into a JSON string.

---

### Q10. What does HTTP 404 mean?

**Answer:** Not Found.

---

# 6️⃣6️⃣ FINAL MEMORY TRICK 🧠

Remember:

```text
             API
              │
        HTTP / HTTPS
              │
       ┌──────┴──────┐
       │             │
    REQUEST       RESPONSE
       │             │
   URL             Status
   Headers         Headers
   Body            Body
```

And CRUD:

```text
C → POST
R → GET
U → PUT / PATCH
D → DELETE
```

And parameters:

```text
/users/100
       ↑
   Route Parameter


/users?city=Hyderabad
       ↑
   Query Parameter
```

And API flow:

```text
React
  ↓
Request
  ↓
Backend
  ↓
Database
  ↓
Backend
  ↓
Response
  ↓
React
  ↓
UI
```

---

# 📌 CHAPTER 23 QUICK REVISION

| Concept | Meaning |
|---|---|
| API | Communication interface between applications |
| REST API | API style using resources and HTTP |
| HTTP | Communication protocol |
| HTTPS | Secure HTTP communication |
| GET | Read |
| POST | Create |
| PUT | Update/replace |
| PATCH | Partial update |
| DELETE | Delete |
| Request | Client → Server |
| Response | Server → Client |
| URL | Resource location |
| Route Parameter | Value inside URL path |
| Query Parameter | Value after `?` |
| Headers | Additional request/response information |
| Body | Actual data |
| JSON | Structured data exchange format |
| `fetch()` | Browser HTTP request API |
| Axios | HTTP client library |
| 200 | Success |
| 201 | Created |
| 404 | Not Found |
| 500 | Server Error |

---

# 🎯 CHAPTER 23 COMPLETE

The most important thing to remember is:

```text
React
  ↓
API Request
  ↓
Backend
  ↓
Database
  ↓
API Response
  ↓
React
  ↓
UI
```

You now understand the **API foundation**.

In the next chapter, we will finally connect these concepts directly to React:

# 🔌 CHAPTER 24 — API + REACT

We will learn:

- 🌐 Fetch data in React
- ⏳ Loading state
- ❌ Error state
- ✅ Success state
- 📤 POST requests
- ✏️ PUT requests
- 🗑️ DELETE requests
- 🔄 CRUD application
- 📁 API service files
- ♻️ Reusable API functions

**Type `NEXT` when you're ready for Chapter 24.**