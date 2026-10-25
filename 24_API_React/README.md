# 🔌 CHAPTER 24 — API + REACT

## 🎯 Goal of This Chapter

In Chapter 23, we learned:

```text
API
GET
POST
PUT
PATCH
DELETE
fetch()
Axios
Request
Response
JSON
Status Codes
```

Now we will use those concepts **inside React**.

Our basic goal:

```text
React
  ↓
API Request
  ↓
Backend
  ↓
Response
  ↓
React State
  ↓
UI
```

Your course defines Chapter 24 around fetching data, loading/error/success states, POST, PUT, DELETE, CRUD, API service files, and reusable API functions.

---

# 📚 WHAT WE WILL LEARN

1. React + API architecture
2. Fetch data in React
3. `useEffect()` + API
4. `useState()` for API data
5. Loading state
6. Error state
7. Success state
8. GET request
9. POST request
10. PUT request
11. DELETE request
12. CRUD application
13. API service files
14. Reusable API functions
15. Complete practical example
16. Dry run
17. Common mistakes
18. Best practices
19. Interview questions
20. Practice tasks
21. Mini challenge
22. Quiz
23. Final revision

---

# 1️⃣ REACT + API ARCHITECTURE

A real application usually looks like:

```text
┌──────────────────┐
│   React Frontend │
│                  │
│ Components       │
│ State            │
│ Forms            │
└────────┬─────────┘
         │
         │ HTTP Request
         ↓
┌──────────────────┐
│    Backend API   │
│                  │
│ FastAPI / Node   │
└────────┬─────────┘
         │
         │ Database Query
         ↓
┌──────────────────┐
│    PostgreSQL    │
│    MongoDB       │
└──────────────────┘
```

This is especially important for you because you already know **FastAPI + PostgreSQL**.

---

# 2️⃣ SIMPLE EXAMPLE

Suppose our backend has:

```text
GET /products
```

React needs to display:

```text
Products

Laptop
Mobile
Keyboard
Mouse
```

Flow:

```text
React
 ↓
GET /products
 ↓
FastAPI
 ↓
PostgreSQL
 ↓
Products
 ↓
React
 ↓
Display Products
```

---

# 3️⃣ WHERE SHOULD WE FETCH DATA?

For data that should be loaded when a component appears, a common beginner approach is:

```jsx
useEffect()
```

Example:

```jsx
useEffect(() => {
    fetchProducts();
}, []);
```

Why?

Because API calls are a **side effect**.

You already learned `useEffect` in Chapter 12.

---

# 4️⃣ BASIC API COMPONENT

Let's create:

```text
Products.jsx
```

Example:

```jsx
import { useEffect, useState } from "react";

function Products() {

    const [products, setProducts] = useState([]);

    useEffect(() => {

        fetch("https://example.com/products")
            .then(response => response.json())
            .then(data => {
                setProducts(data);
            });

    }, []);

    return (
        <div>

            <h1>Products</h1>

            {products.map(product => (
                <p key={product.id}>
                    {product.name}
                </p>
            ))}

        </div>
    );
}

export default Products;
```

---

# 5️⃣ LINE-BY-LINE EXPLANATION

## Step 1

```jsx
import { useEffect, useState } from "react";
```

We need:

```text
useState
 ↓
Store API data

useEffect
 ↓
Run API request
```

---

## Step 2

```jsx
const [products, setProducts] = useState([]);
```

Initially:

```text
products = []
```

After API response:

```text
products = [
    Laptop,
    Mobile,
    Mouse
]
```

---

## Step 3

```jsx
useEffect(() => {
```

React runs the effect after rendering.

---

## Step 4

```jsx
fetch("https://example.com/products")
```

Send request.

---

## Step 5

```jsx
.then(response => response.json())
```

Convert JSON response into JavaScript data.

---

## Step 6

```jsx
.then(data => {
    setProducts(data);
});
```

Store API data in React state.

---

## Step 7

```jsx
products.map(...)
```

Display each product.

---

# 6️⃣ API DATA FLOW 🧠

Remember this:

```text
API
 ↓
fetch()
 ↓
response
 ↓
response.json()
 ↓
data
 ↓
setProducts(data)
 ↓
React re-render
 ↓
UI
```

This is one of the most important flows in this chapter.

---

# 7️⃣ WHY DO WE NEED STATE?

Suppose the API returns:

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

We need somewhere to store this data.

That's why:

```jsx
const [products, setProducts] = useState([]);
```

State stores the API result.

---

# 8️⃣ LOADING STATE ⏳

There is a problem.

When the API request is running:

```text
React
 ↓
Request
 ↓
........ waiting ........
```

What should the user see?

We can show:

```text
Loading...
```

Create:

```jsx
const [loading, setLoading] = useState(true);
```

---

# 9️⃣ COMPLETE LOADING EXAMPLE

```jsx
import { useEffect, useState } from "react";

function Products() {

    const [products, setProducts] = useState([]);
    const [loading, setLoading] = useState(true);

    useEffect(() => {

        fetch("https://example.com/products")
            .then(response => response.json())
            .then(data => {
                setProducts(data);
                setLoading(false);
            });

    }, []);

    if (loading) {
        return <h2>Loading...</h2>;
    }

    return (
        <div>

            <h1>Products</h1>

            {products.map(product => (
                <p key={product.id}>
                    {product.name}
                </p>
            ))}

        </div>
    );
}

export default Products;
```

---

# 🔟 LOADING FLOW

```text
Component starts
      ↓
loading = true
      ↓
Show "Loading..."
      ↓
API request
      ↓
Response received
      ↓
setProducts(data)
      ↓
setLoading(false)
      ↓
Show products
```

---

# 1️⃣1️⃣ ERROR STATE ❌

What if the API fails?

Examples:

```text
Server is down
Network error
Wrong URL
Backend error
```

We need:

```jsx
const [error, setError] = useState("");
```

---

# 1️⃣2️⃣ LOADING + ERROR + DATA

A real API component commonly has three important states:

```text
Loading
Error
Success
```

Think:

```text
              API
               │
        ┌──────┼──────┐
        ↓      ↓      ↓
     Loading  Error  Success
```

---

# 1️⃣3️⃣ COMPLETE GET EXAMPLE

```jsx
import { useEffect, useState } from "react";

function Products() {

    const [products, setProducts] = useState([]);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState("");

    useEffect(() => {

        async function fetchProducts() {

            try {

                const response = await fetch(
                    "https://example.com/products"
                );

                if (!response.ok) {
                    throw new Error(
                        "Failed to fetch products"
                    );
                }

                const data = await response.json();

                setProducts(data);

            } catch (error) {

                setError(error.message);

            } finally {

                setLoading(false);

            }
        }

        fetchProducts();

    }, []);

    if (loading) {
        return <h2>Loading...</h2>;
    }

    if (error) {
        return <h2>Error: {error}</h2>;
    }

    return (
        <div>

            <h1>Products</h1>

            {products.map(product => (
                <div key={product.id}>

                    <h3>
                        {product.name}
                    </h3>

                </div>
            ))}

        </div>
    );
}

export default Products;
```

---

# 1️⃣4️⃣ WHY `response.ok`?

This is very important.

With `fetch()`, an HTTP error status such as `404` or `500` does not automatically cause the promise to reject.

So we often check:

```jsx
if (!response.ok) {
    throw new Error("Request failed");
}
```

This lets our `catch` block handle unsuccessful HTTP responses.

---

# 1️⃣5️⃣ WHY `finally`?

We want loading to stop whether the request:

```text
Success
```

or:

```text
Failure
```

So:

```jsx
finally {
    setLoading(false);
}
```

is useful.

Flow:

```text
             API Request
                 ↓
          ┌──────┴──────┐
          ↓             ↓
       Success        Error
          ↓             ↓
          └──────┬──────┘
                 ↓
          setLoading(false)
```

---

# 1️⃣6️⃣ SUCCESS STATE ✅

Sometimes we want an explicit success state.

Example:

```jsx
const [success, setSuccess] = useState("");
```

After successful POST:

```jsx
setSuccess("Product created successfully");
```

UI:

```jsx
{success && <p>{success}</p>}
```

---

# 1️⃣7️⃣ API STATE MODEL 🧠

A useful mental model:

```text
API State
│
├── data
├── loading
├── error
└── success
```

For example:

```jsx
const [products, setProducts] = useState([]);
const [loading, setLoading] = useState(false);
const [error, setError] = useState("");
const [success, setSuccess] = useState("");
```

---

# 1️⃣8️⃣ GET REQUEST

GET means:

```text
Read data
```

Example:

```jsx
const response = await fetch(
    "http://localhost:8000/products"
);
```

Then:

```jsx
const data = await response.json();
```

Then:

```jsx
setProducts(data);
```

---

# 1️⃣9️⃣ POST REQUEST 📤

POST means:

```text
Create data
```

Suppose we want to create:

```json
{
    "name": "Laptop",
    "price": 50000
}
```

React:

```jsx
async function createProduct() {

    const response = await fetch(
        "http://localhost:8000/products",
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify({
                name: "Laptop",
                price: 50000
            })
        }
    );

    const data = await response.json();

    console.log(data);
}
```

---

# 2️⃣0️⃣ POST FLOW

```text
User clicks "Add Product"
          ↓
createProduct()
          ↓
POST request
          ↓
Backend
          ↓
Database
          ↓
Created Product
          ↓
Response
          ↓
React
```

---

# 2️⃣1️⃣ POST WITH FORM DATA

Suppose we have:

```jsx
const [name, setName] = useState("");
const [price, setPrice] = useState("");
```

Form:

```jsx
<input
    value={name}
    onChange={e => setName(e.target.value)}
/>

<input
    value={price}
    onChange={e => setPrice(e.target.value)}
/>
```

Submit:

```jsx
async function handleSubmit(e) {

    e.preventDefault();

    const response = await fetch(
        "http://localhost:8000/products",
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify({
                name: name,
                price: Number(price)
            })
        }
    );

    const data = await response.json();

    console.log(data);
}
```

---

# 2️⃣2️⃣ PUT REQUEST ✏️

PUT is generally used to update a resource.

Example:

```jsx
async function updateProduct(id) {

    const response = await fetch(
        `http://localhost:8000/products/${id}`,
        {
            method: "PUT",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify({
                name: "Updated Laptop",
                price: 60000
            })
        }
    );

    const data = await response.json();

    console.log(data);
}
```

Notice:

```jsx
/products/${id}
```

The ID is dynamically inserted.

---

# 2️⃣3️⃣ DELETE REQUEST 🗑️

Delete a product:

```jsx
async function deleteProduct(id) {

    const response = await fetch(
        `http://localhost:8000/products/${id}`,
        {
            method: "DELETE"
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to delete product"
        );
    }
}
```

---

# 2️⃣4️⃣ CRUD IN REACT

Now we can connect everything:

```text
CREATE → POST
READ   → GET
UPDATE → PUT
DELETE → DELETE
```

React application:

```text
             Product App
                  │
       ┌──────────┼──────────┐
       ↓          ↓          ↓
     Create      Read      Update/Delete
       │          │             │
      POST       GET        PUT / DELETE
```

---

# 2️⃣5️⃣ SIMPLE CRUD ARCHITECTURE

```text
React UI
   ↓
API Functions
   ↓
Backend API
   ↓
Database
```

For example:

```text
ProductForm
ProductList
EditProduct
DeleteProduct
        ↓
productApi.js
        ↓
FastAPI
        ↓
PostgreSQL
```

---

# 2️⃣6️⃣ WHY API SERVICE FILES?

Imagine we write:

```jsx
fetch("http://localhost:8000/products")
```

in 10 components.

That's bad.

If the API URL changes:

```text
localhost:8000
```

to:

```text
api.myapp.com
```

we would need to change many files.

Instead, create:

```text
src/
└── services/
    └── productApi.js
```

This is called separating API logic from UI logic.

---

# 2️⃣7️⃣ API SERVICE FILE

Create:

```text
src/services/productApi.js
```

Code:

```javascript
const BASE_URL =
    "http://localhost:8000/products";

export async function getProducts() {

    const response = await fetch(BASE_URL);

    if (!response.ok) {
        throw new Error(
            "Failed to fetch products"
        );
    }

    return response.json();
}
```

Now your component doesn't need to know the full URL.

---

# 2️⃣8️⃣ POST FUNCTION

Inside:

```text
productApi.js
```

add:

```javascript
export async function createProduct(product) {

    const response = await fetch(
        BASE_URL,
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify(product)
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to create product"
        );
    }

    return response.json();
}
```

---

# 2️⃣9️⃣ PUT FUNCTION

```javascript
export async function updateProduct(
    id,
    product
) {

    const response = await fetch(
        `${BASE_URL}/${id}`,
        {
            method: "PUT",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify(product)
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to update product"
        );
    }

    return response.json();
}
```

---

# 3️⃣0️⃣ DELETE FUNCTION

```javascript
export async function deleteProduct(id) {

    const response = await fetch(
        `${BASE_URL}/${id}`,
        {
            method: "DELETE"
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to delete product"
        );
    }

    return true;
}
```

---

# 3️⃣1️⃣ COMPLETE `productApi.js`

```javascript
const BASE_URL =
    "http://localhost:8000/products";

export async function getProducts() {

    const response = await fetch(BASE_URL);

    if (!response.ok) {
        throw new Error(
            "Failed to fetch products"
        );
    }

    return response.json();
}

export async function createProduct(product) {

    const response = await fetch(
        BASE_URL,
        {
            method: "POST",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify(product)
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to create product"
        );
    }

    return response.json();
}

export async function updateProduct(
    id,
    product
) {

    const response = await fetch(
        `${BASE_URL}/${id}`,
        {
            method: "PUT",

            headers: {
                "Content-Type": "application/json"
            },

            body: JSON.stringify(product)
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to update product"
        );
    }

    return response.json();
}

export async function deleteProduct(id) {

    const response = await fetch(
        `${BASE_URL}/${id}`,
        {
            method: "DELETE"
        }
    );

    if (!response.ok) {
        throw new Error(
            "Failed to delete product"
        );
    }

    return true;
}
```

---

# 3️⃣2️⃣ USING API SERVICE IN REACT

Now:

```jsx
import {
    getProducts
} from "../services/productApi";
```

Then:

```jsx
useEffect(() => {

    async function loadProducts() {

        try {

            const data =
                await getProducts();

            setProducts(data);

        } catch (error) {

            setError(error.message);

        } finally {

            setLoading(false);

        }
    }

    loadProducts();

}, []);
```

Notice the component no longer contains:

```text
fetch(...)
URL
headers
JSON.stringify(...)
```

The API service handles that.

---

# 3️⃣3️⃣ UI vs API LOGIC

This is a very important architecture concept.

### UI component

Responsible for:

```text
Display
State
User interaction
Loading UI
Error UI
```

### API service

Responsible for:

```text
API URL
HTTP method
Headers
Request body
API response
```

Think:

```text
┌──────────────────────┐
│   React Component    │
│                      │
│ UI + State           │
└──────────┬───────────┘
           │
           ↓
┌──────────────────────┐
│     API Service      │
│                      │
│ HTTP Requests        │
└──────────┬───────────┘
           │
           ↓
┌──────────────────────┐
│     Backend API      │
└──────────────────────┘
```

---

# 3️⃣4️⃣ REUSABLE API FUNCTIONS ♻️

Instead of:

```jsx
fetch(...)
```

everywhere, create:

```javascript
getProducts()
createProduct()
updateProduct()
deleteProduct()
```

Then any component can reuse them.

Example:

```jsx
import {
    getProducts,
    deleteProduct
} from "../services/productApi";
```

This improves organization and reuse.

---

# 3️⃣5️⃣ PRACTICAL PRODUCT LIST

```jsx
import {
    useEffect,
    useState
} from "react";

import {
    getProducts,
    deleteProduct
} from "../services/productApi";

function ProductList() {

    const [products, setProducts] =
        useState([]);

    const [loading, setLoading] =
        useState(true);

    const [error, setError] =
        useState("");

    useEffect(() => {

        loadProducts();

    }, []);

    async function loadProducts() {

        try {

            setLoading(true);
            setError("");

            const data =
                await getProducts();

            setProducts(data);

        } catch (error) {

            setError(error.message);

        } finally {

            setLoading(false);

        }
    }

    async function handleDelete(id) {

        try {

            await deleteProduct(id);

            await loadProducts();

        } catch (error) {

            setError(error.message);

        }
    }

    if (loading) {
        return <h2>Loading...</h2>;
    }

    if (error) {
        return <h2>{error}</h2>;
    }

    return (
        <div>

            <h1>Products</h1>

            {products.map(product => (

                <div key={product.id}>

                    <h3>
                        {product.name}
                    </h3>

                    <p>
                        ₹{product.price}
                    </p>

                    <button
                        onClick={() =>
                            handleDelete(product.id)
                        }
                    >
                        Delete
                    </button>

                </div>

            ))}

        </div>
    );
}

export default ProductList;
```

---

# 3️⃣6️⃣ DRY RUN 🧠

Suppose API returns:

```json
[
    {
        "id": 1,
        "name": "Laptop",
        "price": 50000
    },
    {
        "id": 2,
        "name": "Mobile",
        "price": 20000
    }
]
```

### Step 1

Component renders.

```text
products = []
loading = true
```

UI:

```text
Loading...
```

### Step 2

`useEffect()` runs.

```text
getProducts()
```

### Step 3

API request:

```text
GET /products
```

### Step 4

Backend responds.

```text
Laptop
Mobile
```

### Step 5

```jsx
setProducts(data);
```

State becomes:

```text
products = [
    Laptop,
    Mobile
]
```

### Step 6

```jsx
setLoading(false);
```

### Step 7

React re-renders.

UI:

```text
Products

Laptop
₹50000

Mobile
₹20000
```

---

# 3️⃣7️⃣ DELETE DRY RUN

User clicks:

```text
Delete Laptop
```

Flow:

```text
Button
 ↓
handleDelete(1)
 ↓
deleteProduct(1)
 ↓
DELETE /products/1
 ↓
Backend
 ↓
Database
 ↓
Deleted
 ↓
loadProducts()
 ↓
GET /products
 ↓
Updated list
```

---

# 3️⃣8️⃣ WHY REFETCH AFTER DELETE?

After deleting:

```text
Laptop
```

our old React state might still contain:

```text
Laptop
Mobile
```

We need updated data.

One simple beginner approach:

```jsx
await deleteProduct(id);
await loadProducts();
```

Now React gets the latest list from the server.

Later, you can learn more optimized approaches.

---

# 3️⃣9️⃣ CRUD ARCHITECTURE

A small real-world project can look like:

```text
src/
│
├── components/
│   ├── ProductForm.jsx
│   └── ProductList.jsx
│
├── pages/
│   └── Products.jsx
│
├── services/
│   └── productApi.js
│
├── App.jsx
└── main.jsx
```

Flow:

```text
ProductForm
      ↓
productApi.js
      ↓
POST
      ↓
Backend
```

And:

```text
ProductList
      ↓
productApi.js
      ↓
GET
      ↓
Backend
```

---

# 4️⃣0️⃣ API SERVICE vs COMPONENT

## ❌ Not ideal

```jsx
function ProductList() {

    fetch(
        "http://localhost:8000/products"
    );

    // UI code...
}
```

The component becomes responsible for too many things.

---

## ✅ Better separation

```text
ProductList
     ↓
getProducts()
     ↓
productApi.js
     ↓
Backend
```

---

# 4️⃣1️⃣ COMMON MISTAKES ❌

## Mistake 1 — Calling API directly during render

Avoid:

```jsx
function App() {

    fetch("/products");

    return <h1>Products</h1>;
}
```

This can cause repeated requests because rendering can happen multiple times.

Use an appropriate effect or event handler.

---

# 4️⃣2️⃣ Mistake 2 — Missing dependency array

Example:

```jsx
useEffect(() => {
    loadProducts();
});
```

This effect runs after every render.

For an initial load:

```jsx
useEffect(() => {
    loadProducts();
}, []);
```

The empty dependency array means the effect is set up for the initial mount.

---

# 4️⃣3️⃣ Mistake 3 — No loading state

Without loading state, users may see:

```text
Empty page
```

while data is being fetched.

Better:

```text
Loading...
```

---

# 4️⃣4️⃣ Mistake 4 — No error handling

Don't assume API always succeeds.

Always think:

```text
Success
Failure
Network problem
Server problem
```

---

# 4️⃣5️⃣ Mistake 5 — Forgetting `response.json()`

Wrong:

```jsx
const response = await fetch(url);

setProducts(response);
```

Usually:

```jsx
const response = await fetch(url);

const data = await response.json();

setProducts(data);
```

---

# 4️⃣6️⃣ Mistake 6 — Forgetting `response.ok`

Don't blindly assume:

```jsx
const data = await response.json();
```

For robust handling:

```jsx
if (!response.ok) {
    throw new Error("Request failed");
}
```

---

# 4️⃣7️⃣ Mistake 7 — Mixing API logic everywhere

Avoid having:

```text
Component 1 → fetch
Component 2 → fetch
Component 3 → fetch
Component 4 → fetch
```

Prefer reusable API functions.

```text
Components
    ↓
API Service
    ↓
Backend
```

---

# 4️⃣8️⃣ BEST PRACTICES ✅

### 1. Separate API logic

```text
services/
└── productApi.js
```

### 2. Keep API functions reusable

```javascript
getProducts()
createProduct()
updateProduct()
deleteProduct()
```

### 3. Track loading

```javascript
loading
```

### 4. Track errors

```javascript
error
```

### 5. Keep UI and API responsibilities separate

```text
Component → UI

Service → API
```

### 6. Use meaningful function names

Good:

```javascript
getProducts()
deleteProduct()
```

Avoid:

```javascript
abc()
test()
callApi()
```

---

# 4️⃣9️⃣ INTERVIEW QUESTIONS 🎤

### Q1. How do you fetch API data in React?

**Answer:**

A common approach is to use `fetch()` inside `useEffect()` and store the result in state using `useState()`.

---

### Q2. Why use `useEffect()` for API calls?

**Answer:**

Fetching data is a side effect. `useEffect()` allows us to perform the side effect in response to component lifecycle/dependency changes.

---

### Q3. Why do we use `useState()` with API calls?

**Answer:**

To store API data and update the UI when the data changes.

---

### Q4. What are loading, error, and success states?

**Answer:**

They represent different stages of an API operation:

```text
Loading → Request in progress
Error   → Request failed
Success → Request completed successfully
```

---

### Q5. Why check `response.ok`?

**Answer:**

Because `fetch()` does not automatically reject its promise for HTTP error status codes such as 404 or 500.

---

### Q6. Why create API service files?

**Answer:**

To separate API communication from UI components and make API functions reusable.

---

### Q7. What is a reusable API function?

**Answer:**

A function such as:

```javascript
getProducts()
```

that contains API communication and can be called from multiple components.

---

### Q8. How do you send POST data using fetch?

**Answer:**

Use:

```javascript
method: "POST"
```

with appropriate headers and a serialized request body.

---

### Q9. How do you delete a resource?

**Answer:**

Use a DELETE request, for example:

```javascript
fetch(`/products/${id}`, {
    method: "DELETE"
});
```

---

### Q10. What is the benefit of separating UI and API logic?

**Answer:**

It makes the application easier to maintain, test, reuse, and organize.

---

# 5️⃣0️⃣ PRACTICE TASKS 🧪

## Task 1 — GET

Create:

```text
ProductList.jsx
```

Fetch:

```text
GET /products
```

Display:

```text
Product name
Product price
```

---

## Task 2 — Loading

Show:

```text
Loading...
```

while the API request is running.

---

## Task 3 — Error

Show:

```text
Failed to load products
```

if the request fails.

---

## Task 4 — POST

Create a product:

```text
name
price
```

using:

```text
POST /products
```

---

## Task 5 — PUT

Create an edit button.

When clicked:

```text
PUT /products/:id
```

---

## Task 6 — DELETE

Create:

```text
Delete
```

button.

When clicked:

```text
DELETE /products/:id
```

---

## Task 7 — API Service

Create:

```text
src/services/productApi.js
```

and add:

```javascript
getProducts()
createProduct()
updateProduct()
deleteProduct()
```

---

# 5️⃣1️⃣ MINI CHALLENGE 🚀

Build a small:

# 🛍️ PRODUCT MANAGEMENT APP

### Features

```text
┌────────────────────────────┐
│     Product Management     │
├────────────────────────────┤
│ Name:  [____________]      │
│ Price: [____________]      │
│                            │
│       [ Add Product ]      │
├────────────────────────────┤
│ Products                   │
│                            │
│ Laptop   ₹50000 [Delete]   │
│ Mobile   ₹20000 [Delete]   │
│ Mouse     ₹1000 [Delete]   │
└────────────────────────────┘
```

### Requirements

```text
GET
POST
PUT
DELETE
Loading
Error
Success
API service
Reusable functions
```

---

# 5️⃣2️⃣ QUIZ 🧠

### Q1. Which hook is commonly used to trigger an initial API request?

**Answer:** `useEffect`

---

### Q2. Which hook stores API data?

**Answer:** `useState`

---

### Q3. What should you usually call after `fetch()` to read JSON data?

**Answer:**

```javascript
response.json()
```

---

### Q4. What does `loading` represent?

**Answer:**

That the API request is currently in progress.

---

### Q5. What does `error` represent?

**Answer:**

That the API operation failed.

---

### Q6. Which method creates data?

**Answer:**

```text
POST
```

---

### Q7. Which method updates data?

**Answer:**

```text
PUT
```

or:

```text
PATCH
```

depending on the update.

---

### Q8. Which method deletes data?

**Answer:**

```text
DELETE
```

---

### Q9. Where should reusable API functions commonly be placed?

**Answer:**

A service/API layer, for example:

```text
services/productApi.js
```

---

### Q10. Why separate API logic from components?

**Answer:**

To make the application cleaner, reusable, maintainable, and easier to test.

---

# 5️⃣3️⃣ FINAL MEMORY TRICK 🧠

Remember this complete flow:

```text
              REACT
                │
                ↓
           useEffect()
                │
                ↓
             API Call
                │
       ┌────────┼────────┐
       ↓        ↓        ↓
      GET      POST    PUT/DELETE
       │        │        │
       └────────┼────────┘
                ↓
             Backend
                ↓
             Database
                ↓
            Response
                ↓
           setState()
                ↓
              React
                ↓
                UI
```

And remember API state:

```text
        API
         │
    ┌────┼────┐
    ↓    ↓    ↓
Loading Error Success
```

And architecture:

```text
Component
    ↓
API Service
    ↓
Backend API
    ↓
Database
```

---

# 📌 CHAPTER 24 QUICK REVISION

| Concept | Purpose |
|---|---|
| `useEffect` | Trigger API side effects |
| `useState` | Store API data/state |
| `fetch()` | Send HTTP requests |
| Loading | Request in progress |
| Error | Request failed |
| Success | Request completed |
| GET | Read |
| POST | Create |
| PUT | Update |
| DELETE | Delete |
| API Service | Separate API logic |
| Reusable Function | Reuse API operations |
| `response.json()` | Parse JSON response |
| `response.ok` | Check HTTP success |
| `finally` | Run cleanup code after success/failure |

---

# 🎯 CHAPTER 24 COMPLETE

You have now learned the basic **React → API → Backend → Database → React** workflow.

The next chapter is very important because it explains the JavaScript concepts that make API code work properly:

# ⏳ CHAPTER 25 — ASYNC JAVASCRIPT IN REACT

We will learn:

- Promise
- `async`
- `await`
- `try/catch`
- Promise states
- Error handling
- Parallel requests
- Sequential requests
- API loading patterns

**Type `NEXT` to start Chapter 25.**