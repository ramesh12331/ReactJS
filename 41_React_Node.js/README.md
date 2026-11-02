# 🚀 CHAPTER 41 — REACT + NODE.JS

Your React application is the **frontend**.

Node.js + Express can provide the **backend API**.

The basic architecture is:

```text
⚛️ React Frontend
       ↓
   HTTP Request
       ↓
🟢 Node.js + Express
       ↓
    REST API
       ↓
   💾 Database
```

Your course specifically lists these Chapter 41 topics:

- React frontend
- Node backend
- Express API
- CORS
- API communication
- Environment variables

---

# 1. 🧠 What Is Full-Stack Architecture?

A real application usually has multiple layers.

For example, an e-commerce application:

```text
👤 User
  ↓
⚛️ React
  ↓
🌐 HTTP Request
  ↓
🟢 Node.js
  ↓
🚂 Express
  ↓
💾 Database
```

React handles what the user sees.

Node.js/Express handles backend operations.

The database stores data.

---

# 2. 🎨 Frontend vs Backend

## ⚛️ Frontend

React is responsible for:

```text
UI
Components
Forms
Buttons
Pages
Routing
State
User interaction
```

Example:

```text
Products Page
    ↓
Product Card
    ↓
Add to Cart button
```

---

## 🟢 Backend

Node.js + Express can handle:

```text
API
Business logic
Authentication
Validation
Database communication
CRUD operations
```

Example:

```text
GET /api/products
POST /api/products
PUT /api/products/:id
DELETE /api/products/:id
```

---

# 3. 🧠 What Is Node.js?

Node.js allows JavaScript to run outside the browser.

Normally:

```text
JavaScript
   ↓
Browser
```

With Node.js:

```text
JavaScript
   ↓
Node.js
   ↓
Server
```

So we can use JavaScript to build backend applications.

---

# 4. 🚂 What Is Express?

Express is a Node.js web framework commonly used to build APIs.

Without Express, handling HTTP requests manually can become more cumbersome.

With Express:

```text
GET /api/products
       ↓
Express Route
       ↓
Controller / Logic
       ↓
Response
```

---

# 5. 🌐 What Is an API?

API means:

> **Application Programming Interface**

In our React + Node application, the API allows the frontend and backend to communicate.

Example:

```text
React
 ↓
GET /api/products
 ↓
Express
 ↓
Response
 ↓
React
```

---

# 6. 🧠 Real-World Analogy

Imagine a restaurant.

```text
👤 Customer
    ↓
🍽️ Waiter
    ↓
👨‍🍳 Kitchen
    ↓
🍲 Food
```

In our application:

```text
⚛️ React
    ↓
🌐 API
    ↓
🟢 Node/Express
    ↓
💾 Database
```

The API is like the waiter.

React doesn't directly need to know how the database works.

---

# 7. 🏗️ Project Structure

Let's create two separate applications.

```text
my-fullstack-app/
│
├── frontend/
│   └── React application
│
└── backend/
    └── Node + Express application
```

More specifically:

```text
my-fullstack-app/
│
├── frontend/
│   ├── src/
│   ├── package.json
│   └── ...
│
└── backend/
    ├── controllers/
    ├── routes/
    ├── models/
    ├── server.js
    ├── package.json
    └── .env
```

---

# 8. ⚛️ STEP 1 — React Frontend

You already know how to create a React application using Vite.

For example:

```bash
npm create vite@latest frontend
```

Then:

```bash
cd frontend
npm install
npm run dev
```

Frontend might run at:

```text
http://localhost:5173
```

---

# 9. 🟢 STEP 2 — Create Node Backend

Open another terminal.

Create:

```text
backend
```

Then:

```bash
mkdir backend
cd backend
npm init -y
```

Install Express:

```bash
npm install express
```

Install CORS:

```bash
npm install cors
```

Install dotenv:

```bash
npm install dotenv
```

---

# 10. 📄 Create `server.js`

Inside:

```text
backend/
└── server.js
```

Write:

```js
const express = require("express");

const app = express();

const PORT = 5000;


app.use(express.json());


app.get("/", (req, res) => {

    res.json({
        message: "Backend is running"
    });

});


app.listen(PORT, () => {

    console.log(
        `Server running on port ${PORT}`
    );

});
```

---

# 11. 🔍 Line-by-Line Explanation

### Import Express

```js
const express = require("express");
```

We import Express.

---

### Create Express application

```js
const app = express();
```

`app` represents our Express server/application.

---

### Port

```js
const PORT = 5000;
```

Our backend will listen on port `5000`.

So:

```text
http://localhost:5000
```

---

### JSON middleware

```js
app.use(express.json());
```

This allows Express to understand JSON request bodies.

For example:

```json
{
    "name": "Laptop",
    "price": 50000
}
```

---

### Route

```js
app.get("/", (req, res) => {

    res.json({
        message: "Backend is running"
    });

});
```

When the browser sends:

```text
GET /
```

Express returns JSON.

---

### Start server

```js
app.listen(PORT, () => {

    console.log(
        `Server running on port ${PORT}`
    );

});
```

This starts the backend server.

---

# 12. ▶️ Start Backend

Add a script to `package.json`:

```json
{
    "scripts": {
        "start": "node server.js"
    }
}
```

Then:

```bash
npm start
```

You should see:

```text
Server running on port 5000
```

---

# 13. 🧪 Test Backend

Open:

```text
http://localhost:5000
```

You should receive:

```json
{
    "message": "Backend is running"
}
```

So:

```text
Browser
   ↓
localhost:5000
   ↓
Express
   ↓
GET /
   ↓
JSON response
```

---

# 14. 🌐 Create a Products API

Let's create some temporary product data.

```js
const products = [
    {
        id: 1,
        name: "Laptop",
        price: 50000
    },
    {
        id: 2,
        name: "Phone",
        price: 25000
    },
    {
        id: 3,
        name: "Keyboard",
        price: 2000
    }
];
```

Then:

```js
app.get("/api/products", (req, res) => {

    res.json(products);

});
```

Now:

```text
GET /api/products
```

returns:

```json
[
    {
        "id": 1,
        "name": "Laptop",
        "price": 50000
    },
    {
        "id": 2,
        "name": "Phone",
        "price": 25000
    },
    {
        "id": 3,
        "name": "Keyboard",
        "price": 2000
    }
]
```

---

# 15. 🔗 React → Node

Now we connect the two applications.

```text
⚛️ React
   │
   │ GET /api/products
   ↓
🟢 Express
   │
   ↓
products[]
   │
   ↓
JSON
   │
   ↓
⚛️ React
```

---

# 16. ⚛️ Fetch API from React

Inside:

```text
frontend/src/
```

Create:

```text
Products.jsx
```

Code:

```jsx
import {
    useEffect,
    useState
} from "react";


function Products() {

    const [products, setProducts] =
        useState([]);

    const [loading, setLoading] =
        useState(true);

    const [error, setError] =
        useState(null);


    useEffect(() => {

        fetch(
            "http://localhost:5000/api/products"
        )
            .then((response) => {

                if (!response.ok) {

                    throw new Error(
                        "Failed to fetch products"
                    );
                }

                return response.json();
            })
            .then((data) => {

                setProducts(data);

            })
            .catch((error) => {

                setError(
                    error.message
                );

            })
            .finally(() => {

                setLoading(false);

            });

    }, []);


    if (loading) {

        return (
            <h2>
                ⏳ Loading...
            </h2>
        );
    }


    if (error) {

        return (
            <h2>
                ❌ {error}
            </h2>
        );
    }


    return (

        <div>

            <h1>
                Products
            </h1>

            {products.map((product) => (

                <div key={product.id}>

                    <h2>
                        {product.name}
                    </h2>

                    <p>
                        ₹{product.price}
                    </p>

                </div>

            ))}

        </div>
    );
}


export default Products;
```

---

# 17. 🔍 What Is Happening?

React starts:

```text
Products component
       ↓
useEffect()
       ↓
fetch()
```

Request:

```text
GET http://localhost:5000/api/products
```

Backend receives it:

```text
Express
 ↓
GET /api/products
```

Backend returns:

```json
[
    {
        "id": 1,
        "name": "Laptop",
        "price": 50000
    }
]
```

React receives the JSON:

```text
response.json()
       ↓
data
       ↓
setProducts(data)
       ↓
React re-renders
```

---

# 18. 🧠 Complete Communication Flow

Memorize this:

```text
                 USER
                   ↓
              ⚛️ REACT
                   ↓
             fetch("/api")
                   ↓
                HTTP
                   ↓
          🟢 NODE + EXPRESS
                   ↓
               ROUTE
                   ↓
             BUSINESS LOGIC
                   ↓
              💾 DATABASE
                   ↓
              JSON RESPONSE
                   ↓
          🟢 NODE + EXPRESS
                   ↓
                 HTTP
                   ↓
              ⚛️ REACT
                   ↓
                  UI
```

This is one of the most important full-stack concepts.

---

# 19. 📡 GET Request

GET is used to retrieve data.

React:

```js
fetch(
    "http://localhost:5000/api/products"
);
```

Backend:

```js
app.get(
    "/api/products",
    (req, res) => {

        res.json(products);

    }
);
```

Flow:

```text
React
 ↓
GET
 ↓
/api/products
 ↓
Express
 ↓
products
```

---

# 20. ➕ POST Request

POST is generally used to send/create data.

React:

```js
fetch(
    "http://localhost:5000/api/products",
    {
        method: "POST",

        headers: {
            "Content-Type":
                "application/json"
        },

        body: JSON.stringify({
            name: "Mouse",
            price: 1000
        })
    }
);
```

Backend:

```js
app.post(
    "/api/products",
    (req, res) => {

        console.log(req.body);

        res.json({
            message:
                "Product created"
        });

    }
);
```

---

# 21. 🔍 Understanding POST

React sends:

```json
{
    "name": "Mouse",
    "price": 1000
}
```

Express receives:

```js
req.body
```

So:

```text
React
 ↓
POST
 ↓
JSON body
 ↓
Express
 ↓
req.body
```

---

# 22. ✏️ PUT Request

PUT is generally used to update a resource.

Example:

```js
fetch(
    "http://localhost:5000/api/products/1",
    {
        method: "PUT",

        headers: {
            "Content-Type":
                "application/json"
        },

        body: JSON.stringify({
            name: "Gaming Laptop",
            price: 70000
        })
    }
);
```

Backend:

```js
app.put(
    "/api/products/:id",
    (req, res) => {

        console.log(
            req.params.id
        );

        console.log(
            req.body
        );

        res.json({
            message:
                "Product updated"
        });

    }
);
```

---

# 23. 🗑️ DELETE Request

Delete a product:

```js
fetch(
    "http://localhost:5000/api/products/1",
    {
        method: "DELETE"
    }
);
```

Backend:

```js
app.delete(
    "/api/products/:id",
    (req, res) => {

        console.log(
            req.params.id
        );

        res.json({
            message:
                "Product deleted"
        });

    }
);
```

---

# 24. 🧠 CRUD Architecture

You already learned CRUD from your backend work.

React + Node:

```text
CREATE
  ↓
POST

READ
  ↓
GET

UPDATE
  ↓
PUT/PATCH

DELETE
  ↓
DELETE
```

Table:

| Operation | HTTP Method | Example |
|---|---|---|
| Create | POST | `/api/products` |
| Read | GET | `/api/products` |
| Update | PUT | `/api/products/1` |
| Delete | DELETE | `/api/products/1` |

---

# 25. 🚨 What Is CORS?

Now you may see an error like:

```text
Access to fetch at ...
has been blocked by CORS policy
```

Why?

Because our applications are running on different origins.

For example:

```text
React:
http://localhost:5173

Backend:
http://localhost:5000
```

Different ports mean different origins.

The browser applies cross-origin security rules.

---

# 26. 🧠 Simple CORS Analogy

Imagine:

```text
🏠 House A
```

tries to enter:

```text
🏠 House B
```

House B says:

> "Are you allowed to enter?"

CORS is part of the browser/server mechanism that determines whether cross-origin requests are allowed.

---

# 27. 🛠️ Install CORS

Backend:

```bash
npm install cors
```

Then:

```js
const cors = require("cors");

app.use(cors());
```

Complete beginning:

```js
const express = require("express");
const cors = require("cors");

const app = express();

const PORT = 5000;


app.use(cors());

app.use(express.json());
```

Now our development frontend can communicate with the backend when the server's CORS policy permits it.

---

# 28. 🔐 Restrict CORS

Instead of allowing all origins:

```js
app.use(cors());
```

you can configure allowed origins.

For example:

```js
app.use(
    cors({
        origin:
            "http://localhost:5173"
    })
);
```

Meaning:

```text
Only this frontend origin
        ↓
http://localhost:5173
```

is allowed by this CORS configuration.

---

# 29. ⚠️ CORS Important Point

CORS is primarily a **browser security mechanism**.

It is not:

```text
❌ Authentication
❌ Authorization
❌ Database security
```

CORS controls which browser origins are permitted to make certain cross-origin requests.

---

# 30. 🔐 Environment Variables

Our React application currently contains:

```js
fetch(
    "http://localhost:5000/api/products"
);
```

This works during development.

But production may use another URL.

For example:

```text
Development:
http://localhost:5000

Production:
https://api.example.com
```

We don't want to hard-code URLs throughout the application.

---

# 31. ⚛️ Vite Environment Variables

In a Vite React application, environment variables exposed to client-side code use the `VITE_` prefix.

Create:

```text
.env
```

Example:

```env
VITE_API_URL=http://localhost:5000
```

Then:

```jsx
const API_URL =
    import.meta.env.VITE_API_URL;
```

Use:

```jsx
fetch(
    `${API_URL}/api/products`
);
```

---

# 32. 🧠 Why Use Environment Variables?

Instead of:

```text
❌ Hard-coded API URL everywhere
```

we have:

```text
.env
   ↓
VITE_API_URL
   ↓
React
```

This makes configuration easier between environments.

---

# 33. ⚠️ Important Security Rule

A frontend environment variable is **not a secret** just because it is stored in `.env`.

Anything exposed to browser-side React code can ultimately be visible to users.

Therefore, don't put sensitive secrets such as:

```text
❌ Database password
❌ Private API secret
❌ Server private key
❌ JWT signing secret
```

in client-exposed Vite variables.

Frontend variables should contain values that are safe to expose, such as:

```text
API base URL
public configuration
```

---

# 34. 🟢 Backend Environment Variables

For Node.js, we can use `.env`.

Install:

```bash
npm install dotenv
```

Create:

```text
backend/.env
```

Example:

```env
PORT=5000
DATABASE_URL=your_database_url
```

Then:

```js
require("dotenv").config();

const PORT =
    process.env.PORT || 5000;
```

---

# 35. 🧠 Frontend vs Backend Environment Variables

Remember:

```text
React / Vite
     ↓
import.meta.env.VITE_...

Node.js
     ↓
process.env....
```

Important:

```text
Frontend
 ↓
Publicly exposed configuration

Backend
 ↓
Can safely keep server-only secrets
```

Of course, server secrets must still be handled securely and not committed to Git.

---

# 36. 📁 Full Project

A practical setup:

```text
my-fullstack-app/
│
├── frontend/
│   │
│   ├── src/
│   │   ├── components/
│   │   ├── pages/
│   │   ├── services/
│   │   └── App.jsx
│   │
│   ├── .env
│   └── package.json
│
└── backend/
    │
    ├── controllers/
    ├── routes/
    ├── models/
    ├── middleware/
    ├── server.js
    ├── .env
    └── package.json
```

---

# 37. 🏗️ Better Backend Architecture

Instead of putting everything into `server.js`:

```text
❌ server.js
   ├── routes
   ├── database
   ├── business logic
   ├── validation
   └── everything
```

we can separate:

```text
backend/
│
├── controllers/
│
├── routes/
│
├── models/
│
├── middleware/
│
├── services/
│
├── config/
│
└── server.js
```

You already have experience with this type of Node/Express organization.

---

# 38. 🌐 Routes

Example:

```text
routes/
└── productRoutes.js
```

```js
const express = require("express");

const router =
    express.Router();


router.get(
    "/products",
    (req, res) => {

        res.json({
            message:
                "Products API"
        });

    }
);


module.exports = router;
```

---

# 39. 🔌 Connect Route to Server

In `server.js`:

```js
const productRoutes =
    require("./routes/productRoutes");


app.use(
    "/api",
    productRoutes
);
```

Now:

```text
/api + /products
```

becomes:

```text
/api/products
```

---

# 40. 🧠 Backend Request Flow

```text
React
  ↓
GET /api/products
  ↓
server.js
  ↓
productRoutes
  ↓
Controller
  ↓
Service
  ↓
Database
  ↓
Controller
  ↓
JSON response
  ↓
React
```

This is a professional full-stack flow.

---

# 41. ⚠️ Common Mistakes

## Mistake 1 — Wrong backend URL

React:

```js
fetch(
    "http://localhost:5000/api/products"
);
```

But backend actually runs on:

```text
3000
```

Result:

```text
❌ Failed to fetch
```

Always check the backend port.

---

## Mistake 2 — Backend not running

React is running:

```text
5173
```

but backend isn't running:

```text
5000 ❌
```

Then the API request fails.

Run both:

```text
Terminal 1
Frontend

Terminal 2
Backend
```

---

## Mistake 3 — CORS not configured

You may see a browser CORS error.

Configure CORS appropriately on the backend.

---

## Mistake 4 — Forgetting `express.json()`

If you send:

```json
{
    "name": "Mouse"
}
```

but don't configure:

```js
app.use(express.json());
```

then `req.body` may not contain the parsed JSON body as expected.

---

## Mistake 5 — Hard-coding URLs everywhere

Avoid:

```text
fetch("http://localhost:5000/...")
```

throughout many files.

Centralize your API base URL.

---

# 42. 🧪 COMPLETE MINI BACKEND

Let's combine the basics.

```js
const express = require("express");
const cors = require("cors");
require("dotenv").config();


const app = express();

const PORT =
    process.env.PORT || 5000;


app.use(cors());

app.use(express.json());


const products = [

    {
        id: 1,
        name: "Laptop",
        price: 50000
    },

    {
        id: 2,
        name: "Phone",
        price: 25000
    },

    {
        id: 3,
        name: "Keyboard",
        price: 2000
    }

];


app.get(
    "/api/products",
    (req, res) => {

        res.json(products);

    }
);


app.post(
    "/api/products",
    (req, res) => {

        const product =
            req.body;

        console.log(
            "New product:",
            product
        );

        res.status(201).json({

            message:
                "Product created",

            product

        });

    }
);


app.listen(
    PORT,
    () => {

        console.log(
            `Server running on port ${PORT}`
        );

    }
);
```

---

# 43. 🔄 COMPLETE DRY RUN

Suppose React runs on:

```text
http://localhost:5173
```

Backend:

```text
http://localhost:5000
```

User opens:

```text
/products
```

React renders:

```text
Products.jsx
```

Then:

```text
useEffect()
    ↓
fetch()
    ↓
GET http://localhost:5000/api/products
```

Backend:

```text
Express
 ↓
/api/products
 ↓
products[]
```

Backend returns:

```json
[
    {
        "id": 1,
        "name": "Laptop",
        "price": 50000
    }
]
```

React:

```text
response.json()
       ↓
data
       ↓
setProducts(data)
       ↓
re-render
```

UI:

```text
Products

Laptop
₹50000

Phone
₹25000

Keyboard
₹2000
```

---

# 44. 💼 INTERVIEW QUESTIONS

## 🟢 Beginner

### Q1. What is Node.js?

Node.js is a JavaScript runtime that allows JavaScript to run outside the browser.

### Q2. What is Express?

Express is a Node.js web framework commonly used to create web servers and APIs.

### Q3. How does React communicate with Node.js?

Usually through HTTP requests to backend API endpoints.

---

## 🟡 Intermediate

### Q4. What is CORS?

CORS is a browser security mechanism involving rules that control cross-origin HTTP requests.

### Q5. Why do we use `express.json()`?

To parse incoming JSON request bodies.

### Q6. What is `req.body`?

It contains the parsed request body when the appropriate middleware has processed it.

### Q7. What is `req.params`?

It contains values captured from route parameters.

Example:

```text
/api/products/10
```

Route:

```js
/api/products/:id
```

Then:

```js
req.params.id
```

is:

```text
10
```

---

## 🔴 Advanced

### Q8. Why should API logic be separated from React components?

To improve separation of concerns, reusability, testing, and maintainability.

### Q9. Why shouldn't frontend environment variables contain secrets?

Because client-side values can be exposed to users in the browser/application bundle.

### Q10. What is the typical React + Node architecture?

```text
React
 ↓
HTTP
 ↓
Express
 ↓
Business Logic
 ↓
Database
```

---

# 45. 📝 PRACTICE

### Practice 1

Create:

```text
GET /api/products
```

Return five products.

---

### Practice 2

Create:

```text
GET /api/products/:id
```

Return one product by ID.

---

### Practice 3

Create:

```text
POST /api/products
```

Read:

```js
req.body
```

---

### Practice 4

Connect React to:

```text
GET /api/products
```

and display the products.

---

### Practice 5

Add:

```text
Loading
Error
Success
```

states.

---

### Practice 6

Add a React form:

```text
Product Name
Price
[ Add Product ]
```

Send the data using POST.

---

# 46. 🧩 MINI PROJECT

# 🛒 React + Node Product Management

Build:

```text
⚛️ React
      ↓
🟢 Node.js
      ↓
🚂 Express
      ↓
Products API
```

Features:

```text
GET
POST
PUT
DELETE
```

Frontend:

```text
Products
Add Product
Edit Product
Delete Product
```

Backend:

```text
/api/products
/api/products/:id
```

For now, you can use an in-memory array.

Later we'll connect the backend to a real database.

---

# 47. 📝 10-QUESTION QUIZ

### Q1. What is React responsible for?

**Answer:** The frontend/user interface.

---

### Q2. What is Node.js?

**Answer:** A JavaScript runtime that runs JavaScript outside the browser.

---

### Q3. What is Express?

**Answer:** A Node.js web framework used to build servers and APIs.

---

### Q4. What is an API?

**Answer:** An interface that allows different software parts to communicate.

---

### Q5. Which HTTP method is commonly used to retrieve data?

**Answer:** `GET`.

---

### Q6. Which HTTP method is commonly used to create data?

**Answer:** `POST`.

---

### Q7. Which HTTP method is commonly used to update data?

**Answer:** `PUT` or `PATCH`, depending on the API design.

---

### Q8. Why do we use CORS?

**Answer:** To control whether browser-based cross-origin requests are permitted according to the server's CORS policy.

---

### Q9. What does `express.json()` do?

**Answer:** It parses incoming JSON request bodies.

---

### Q10. Complete the architecture:

```text
React
 ↓
?
 ↓
Express
 ↓
Database
```

**Answer:**

```text
React
 ↓
HTTP Request
 ↓
Express
 ↓
Database
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
⚛️ REACT
   ↓
Frontend

🟢 NODE.JS
   ↓
JavaScript Runtime

🚂 EXPRESS
   ↓
Backend/API Framework

🌐 HTTP
   ↓
Communication

💾 DATABASE
   ↓
Data Storage
```

And the most important full-stack flow:

```text
USER
 ↓
REACT
 ↓
FETCH / HTTP
 ↓
NODE + EXPRESS
 ↓
ROUTE
 ↓
CONTROLLER / SERVICE
 ↓
DATABASE
 ↓
JSON
 ↓
REACT
 ↓
UI
```

### ⭐ One-line memory

> **React displays the data, Node/Express provides the API, and the database stores the data.**

---

# ✅ CHAPTER 41 COMPLETE

You learned:

- ⚛️ React frontend
- 🟢 Node.js backend
- 🚂 Express API
- 🌐 HTTP communication
- GET
- POST
- PUT
- DELETE
- 📡 CORS
- 🔐 Environment variables
- `req.body`
- `req.params`
- API services
- Frontend/backend separation
- Full-stack architecture
- React + Node CRUD flow
- Common mistakes
- Interview questions
- Practice exercises
- Mini project

---

# 🚀 NEXT CHAPTER

## Chapter 42 — React + MongoDB

We will build the complete **MERN architecture**:

```text
⚛️ React
   ↓
🟢 Node.js
   ↓
🚂 Express
   ↓
🍃 MongoDB
```

We will learn:

- MERN architecture
- MongoDB integration
- CRUD
- Express API
- React frontend
- Authentication
- Complete data flow

Type **NEXT** to continue.