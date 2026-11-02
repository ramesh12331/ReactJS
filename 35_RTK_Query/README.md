
# 🚀 ReactJS — Chapter 35
# RTK Query

---

# 🎯 What You Will Learn

In this chapter:

- 🧠 What is RTK Query?
- 🤔 Why do we need RTK Query?
- 🏗️ API Slice
- 🔍 Queries
- ✏️ Mutations
- ⏳ Loading states
- ❌ Error states
- 💾 Caching
- 🔄 Automatic refetching
- 📡 API integration
- 🆚 RTK Query vs Axios
- 🧪 Complete CRUD example
- ⚠️ Common mistakes
- 💼 Interview questions
- 📝 Practice
- 🧩 Mini challenge
- 📝 Quiz
- 🧠 Final memory trick

---

# 1. 🧠 What is RTK Query?

**RTK Query** is a data-fetching and caching tool included with Redux Toolkit.

It is designed to make API data management easier.

In Chapter 34, we manually handled:

```text
API request
   ↓
loading
   ↓
success
   ↓
error
   ↓
store data
```

With RTK Query, much of this work is handled for us.

Think:

```text
Chapter 34

React
 ↓
dispatch()
 ↓
createAsyncThunk()
 ↓
Axios
 ↓
API
 ↓
Reducer
 ↓
Redux state


Chapter 35

React
 ↓
RTK Query
 ↓
API
 ↓
Cache + State
 ↓
React
```

---

# 2. 🤔 Why Do We Need RTK Query?

Suppose you have:

```text
Users
Products
Orders
Categories
```

and every feature needs API requests.

Without a dedicated data-fetching solution, you may repeatedly write:

```text
Loading state
Error state
API request
Success state
Store data
Caching
Refetching
```

RTK Query provides tools for these common API-data tasks.

---

# 3. 🧠 RTK Query vs `createAsyncThunk`

### `createAsyncThunk`

You manually manage much of the state:

```js
{
    data: [],
    loading: false,
    error: null
}
```

and write:

```text
pending
fulfilled
rejected
```

### RTK Query

RTK Query provides generated hooks and handles common request state and caching behavior.

Conceptually:

```text
createAsyncThunk
    ↓
More manual API state management


RTK Query
    ↓
More API-data management built in
```

---

# 4. 🏗️ RTK Query Architecture

The basic architecture:

```text
                API
                 ↑
                 │
          RTK Query API Slice
                 │
       ┌─────────┴─────────┐
       ↓                   ↓
    Queries             Mutations
       ↓                   ↓
    GET data          POST/PUT/DELETE
       │                   │
       └─────────┬─────────┘
                 ↓
              Cache
                 ↓
             React UI
```

---

# 5. 📦 Installation

If Redux Toolkit and React Redux are already installed:

```bash
npm install @reduxjs/toolkit react-redux
```

For our RTK Query example, we use the API functionality provided by Redux Toolkit.

---

# 6. 🧩 What is an API Slice?

An **API slice** is where we define our API endpoints.

Example:

```text
userApi
   ↓
getUsers
getUser
createUser
updateUser
deleteUser
```

Instead of putting each API request in separate functions, we define the endpoints together.

---

# 7. 📁 Project Structure

A simple structure:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   └── users/
│       └── userApi.js
│
├── App.jsx
└── main.jsx
```

Our API logic:

```text
features/
   ↓
users/
   ↓
userApi.js
```

---

# 8. 🏗️ Creating an API Slice

Create:

```text
src/features/users/userApi.js
```

Code:

```js
import { createApi, fetchBaseQuery } from "@reduxjs/toolkit/query/react";

export const userApi = createApi({
    reducerPath: "userApi",

    baseQuery: fetchBaseQuery({
        baseUrl: "https://jsonplaceholder.typicode.com"
    }),

    endpoints: (builder) => ({

        getUsers: builder.query({
            query: () => "/users"
        })

    })
});
```

---

# 9. 🔍 Understanding `createApi()`

```js
createApi({
```

creates our RTK Query API slice.

Think:

```text
createApi()
    ↓
API management
    ↓
Endpoints
    ↓
Queries + Mutations
```

---

# 10. 🏷️ `reducerPath`

We have:

```js
reducerPath: "userApi"
```

This identifies where RTK Query's state is stored in Redux.

Think:

```text
Redux Store
   ↓
userApi
   ↓
RTK Query state
```

---

# 11. 🌐 `baseQuery`

We use:

```js
baseQuery: fetchBaseQuery({
    baseUrl: "https://jsonplaceholder.typicode.com"
})
```

This defines the common base URL.

Then instead of writing:

```text
https://jsonplaceholder.typicode.com/users
```

every time, we can write:

```js
query: () => "/users"
```

---

# 12. 🔍 What is `fetchBaseQuery()`?

`fetchBaseQuery()` is a small wrapper around the browser's `fetch()` API designed for common RTK Query HTTP requests.

Think:

```text
fetchBaseQuery
      ↓
HTTP request helper
      ↓
API
```

---

# 13. 🔍 What is an Endpoint?

Inside:

```js
endpoints: (builder) => ({
```

we define our API operations.

For example:

```js
getUsers: builder.query({
    query: () => "/users"
})
```

This creates a **query endpoint**.

---

# 14. 🔍 What is a Query?

A query is generally used to **retrieve data**.

Examples:

```text
GET users
GET products
GET orders
GET one user
```

Think:

```text
Query
  ↓
READ data
```

---

# 15. 👥 `getUsers` Query

We wrote:

```js
getUsers: builder.query({
    query: () => "/users"
})
```

This means:

```text
getUsers
   ↓
GET /users
```

---

# 16. 🪄 Generated Hook

One of the powerful features of RTK Query is that it generates React hooks for endpoints.

For:

```js
getUsers
```

the generated hook is:

```js
useGetUsersQuery
```

So in React:

```jsx
import { useGetUsersQuery } from "./features/users/userApi";
```

Then:

```jsx
const {
    data,
    error,
    isLoading
} = useGetUsersQuery();
```

---

# 17. 🧠 Automatic Hook Naming

Remember the pattern:

```text
Endpoint:
getUsers

Generated hook:
useGetUsersQuery
```

Another example:

```text
getProducts

↓

useGetProductsQuery
```

Another:

```text
getOrders

↓

useGetOrdersQuery
```

Memory:

```text
builder.query()
       ↓
use...Query()
```

---

# 18. 🏪 Add API Slice to Redux Store

Open:

```text
src/app/store.js
```

We need:

```js
import { configureStore } from "@reduxjs/toolkit";

import {
    userApi
} from "../features/users/userApi";
```

Then:

```js
export const store = configureStore({

    reducer: {

        [userApi.reducerPath]:
            userApi.reducer

    },

    middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware().concat(
            userApi.middleware
        )

});
```

---

# 19. 🔍 Why Add the Reducer?

This:

```js
[userApi.reducerPath]: userApi.reducer
```

adds RTK Query's state management to Redux.

Conceptually:

```text
Redux Store
    │
    └── userApi
          │
          ├── queries
          ├── cache
          └── request state
```

---

# 20. ⚙️ Why Add Middleware?

We also add:

```js
middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(
        userApi.middleware
    )
```

RTK Query uses middleware for things such as:

- request lifecycle handling
- caching behavior
- subscriptions
- refetching behavior

So remember:

```text
API slice
   ↓
Reducer + Middleware
   ↓
Redux Store
```

---

# 21. 🔗 Connect Redux Provider

Our `main.jsx`:

```jsx
import React from "react";

import ReactDOM from "react-dom/client";

import { Provider } from "react-redux";

import App from "./App";

import { store } from "./app/store";


ReactDOM.createRoot(
    document.getElementById("root")
).render(

    <React.StrictMode>

        <Provider store={store}>

            <App />

        </Provider>

    </React.StrictMode>
);
```

---

# 22. 🖥️ Use the Query in React

Now:

```jsx
import { useGetUsersQuery }
    from "./features/users/userApi";

function App() {

    const {
        data,
        error,
        isLoading
    } = useGetUsersQuery();

    return (
        <div>
            <h1>Users</h1>
        </div>
    );
}

export default App;
```

---

# 23. ⏳ Loading State

RTK Query gives us:

```js
isLoading
```

We can write:

```jsx
if (isLoading) {
    return <h2>⏳ Loading...</h2>;
}
```

Flow:

```text
Component
   ↓
useGetUsersQuery()
   ↓
API request
   ↓
isLoading = true
   ↓
Loading UI
```

---

# 24. ❌ Error State

We can use:

```js
error
```

Example:

```jsx
if (error) {
    return <h2>❌ Failed to load users</h2>;
}
```

---

# 25. ✅ Success State

When the request succeeds:

```js
data
```

contains the response data.

Example:

```jsx
return (
    <div>

        <h1>Users</h1>

        {data?.map((user) => (

            <div key={user.id}>

                <h2>{user.name}</h2>

                <p>{user.email}</p>

            </div>

        ))}

    </div>
);
```

---

# 26. 🎯 Complete Query Example

## `userApi.js`

```js
import {
    createApi,
    fetchBaseQuery
} from "@reduxjs/toolkit/query/react";


export const userApi = createApi({

    reducerPath: "userApi",

    baseQuery: fetchBaseQuery({
        baseUrl:
            "https://jsonplaceholder.typicode.com"
    }),

    endpoints: (builder) => ({

        getUsers: builder.query({

            query: () => "/users"

        })

    })

});


export const {
    useGetUsersQuery
} = userApi;
```

---

# 27. `store.js`

```js
import {
    configureStore
} from "@reduxjs/toolkit";

import {
    userApi
} from "../features/users/userApi";


export const store = configureStore({

    reducer: {

        [userApi.reducerPath]:
            userApi.reducer

    },

    middleware: (getDefaultMiddleware) =>
        getDefaultMiddleware().concat(
            userApi.middleware
        )

});
```

---

# 28. `App.jsx`

```jsx
import {
    useGetUsersQuery
} from "./features/users/userApi";


function App() {

    const {
        data,
        error,
        isLoading
    } = useGetUsersQuery();


    if (isLoading) {

        return (
            <h2>
                ⏳ Loading users...
            </h2>
        );

    }


    if (error) {

        return (
            <h2>
                ❌ Failed to load users
            </h2>
        );

    }


    return (

        <div>

            <h1>Users</h1>

            {data?.map((user) => (

                <div key={user.id}>

                    <h2>
                        {user.name}
                    </h2>

                    <p>
                        {user.email}
                    </p>

                </div>

            ))}

        </div>

    );
}


export default App;
```

---

# 29. 🧪 Dry Run

When:

```jsx
useGetUsersQuery()
```

runs:

```text
React Component
      ↓
useGetUsersQuery()
      ↓
RTK Query
      ↓
GET /users
      ↓
Server
```

While waiting:

```text
isLoading = true
```

After success:

```text
data = users
```

If it fails:

```text
error = error information
```

---

# 30. 🆚 Query vs Mutation

This is extremely important.

## 🔍 Query

Used mainly to retrieve data.

```text
GET
 ↓
Query
```

Examples:

```text
getUsers
getProducts
getOrders
```

---

## ✏️ Mutation

Used for operations that modify server-side data.

```text
POST
PUT
PATCH
DELETE
 ↓
Mutation
```

Examples:

```text
createUser
updateUser
deleteUser
```

Memory trick:

```text
🔍 Query
   ↓
READ

✏️ Mutation
   ↓
CHANGE
```

---

# 31. ✏️ Create a Mutation

Suppose we want to create a user.

Add:

```js
createUser: builder.mutation({

    query: (newUser) => ({

        url: "/users",

        method: "POST",

        body: newUser

    })

})
```

Complete endpoint section:

```js
endpoints: (builder) => ({

    getUsers: builder.query({

        query: () => "/users"

    }),

    createUser: builder.mutation({

        query: (newUser) => ({

            url: "/users",

            method: "POST",

            body: newUser

        })

    })

})
```

---

# 32. 🪄 Generated Mutation Hook

For:

```text
createUser
```

RTK Query generates:

```text
useCreateUserMutation
```

Export:

```js
export const {
    useGetUsersQuery,
    useCreateUserMutation
} = userApi;
```

---

# 33. 🚀 Using the Mutation

In React:

```jsx
const [
    createUser,
    {
        isLoading,
        error
    }
] = useCreateUserMutation();
```

Then:

```jsx
const handleCreateUser = async () => {

    await createUser({

        name: "Ramesh",

        email: "ramesh@example.com"

    });

};
```

Button:

```jsx
<button onClick={handleCreateUser}>
    Add User
</button>
```

---

# 34. 🧠 Understand Mutation Return Value

Notice this:

```js
const [
    createUser,
    {
        isLoading,
        error
    }
] = useCreateUserMutation();
```

There are two important parts:

```text
createUser
    ↓
function used to trigger mutation


isLoading / error
    ↓
mutation status
```

So:

```js
createUser(data)
```

starts the mutation.

---

# 35. 🔄 Query vs Mutation

| Feature | Query | Mutation |
|---|---|---|
| Main purpose | Read data | Change data |
| Typical HTTP | GET | POST/PUT/PATCH/DELETE |
| Example | getUsers | createUser |
| Hook | `useGetUsersQuery()` | `useCreateUserMutation()` |
| Caching | Yes | Different behavior |
| Trigger | Usually automatically | Usually manually |

Memory:

```text
QUERY
↓
"Give me data"

MUTATION
↓
"Change data"
```

---

# 36. 💾 What is Caching?

Caching means storing previously fetched data so it can be reused instead of unnecessarily requesting the same data again.

Example:

```text
First component
    ↓
GET /users
    ↓
API
    ↓
Data
    ↓
Cache
```

Later:

```text
Second component
    ↓
Needs /users
    ↓
RTK Query
    ↓
Existing cached data
```

This can reduce unnecessary network requests.

---

# 37. 🧠 Real-World Example of Caching

Imagine an e-commerce application.

You open:

```text
Products Page
```

The application requests:

```text
GET /products
```

RTK Query stores the result.

Then you navigate:

```text
Products
 ↓
Product Details
 ↓
Products
```

The existing query data can be reused according to RTK Query's cache/subscription behavior.

---

# 38. 🔄 Automatic Refetching

RTK Query supports automatic refetching behavior.

For example, you can configure a query to refetch when the browser window is focused.

Example:

```jsx
const {
    data,
    error,
    isLoading
} = useGetUsersQuery(
    undefined,
    {
        refetchOnFocus: true
    }
);
```

Conceptually:

```text
Browser
  ↓
User leaves page
  ↓
User returns
  ↓
Focus event
  ↓
RTK Query
  ↓
Refetch
```

This can help keep data fresh.

---

# 39. 🔄 Refetch on Mount or Argument Change

You can also use query options such as:

```jsx
useGetUsersQuery(
    undefined,
    {
        refetchOnMountOrArgChange: true
    }
);
```

This is useful when you want a query to refetch based on mounting or argument changes according to the option's behavior.

---

# 40. 🧠 Cache Tags

RTK Query can use **tags** to connect queries and mutations.

For example:

```text
getUsers
   ↓
provides User tag

createUser
   ↓
invalidates User tag
```

Then the affected query can be refetched.

This is a very important RTK Query concept.

---

# 41. 🏷️ `tagTypes`

Example:

```js
export const userApi = createApi({

    reducerPath: "userApi",

    baseQuery: fetchBaseQuery({
        baseUrl:
            "https://jsonplaceholder.typicode.com"
    }),

    tagTypes: ["User"],

    endpoints: (builder) => ({

        getUsers: builder.query({

            query: () => "/users",

            providesTags: ["User"]

        }),

        createUser: builder.mutation({

            query: (newUser) => ({

                url: "/users",

                method: "POST",

                body: newUser

            }),

            invalidatesTags: ["User"]

        })

    })

});
```

---

# 42. 🧠 Understand Tags

Think:

```text
getUsers
   ↓
provides "User"
   ↓
Cache knows:
"This data represents Users"
```

Then:

```text
createUser
   ↓
invalidates "User"
   ↓
Existing User data becomes stale
   ↓
RTK Query can refetch
```

Flow:

```text
GET USERS
   ↓
providesTags
   ↓
Cache

CREATE USER
   ↓
invalidatesTags
   ↓
Users cache invalidated
   ↓
Refetch
```

---

# 43. 🛒 Real-World Cart Example

Imagine:

```text
Products
Cart
Orders
```

A product query:

```js
getProducts
```

A mutation:

```js
createProduct
```

A tag:

```text
Product
```

Flow:

```text
getProducts
    ↓
providesTags: Product
    ↓
Cache

createProduct
    ↓
invalidatesTags: Product
    ↓
Cache invalidated
    ↓
Products refetched
```

This is one reason RTK Query is useful for CRUD applications.

---

# 44. 🆚 RTK Query vs Axios

This distinction is important.

### Axios

Axios is primarily an HTTP client.

```text
Axios
 ↓
HTTP request
 ↓
Response
```

You generally manage application-level API state yourself.

---

### RTK Query

RTK Query is designed around API data fetching and caching within Redux Toolkit.

```text
RTK Query
 ↓
Request
 ↓
Loading state
 ↓
Data
 ↓
Caching
 ↓
Refetching
 ↓
Invalidation
```

So they are not exactly the same type of tool.

---

# 45. 📊 RTK Query vs Axios

| Feature | Axios | RTK Query |
|---|---|---|
| HTTP requests | ✅ | ✅ |
| GET/POST/PUT/DELETE | ✅ | ✅ |
| Loading state | Manual | Built-in query state |
| Error state | Manual | Built-in query state |
| Caching | Manual | Built-in |
| Cache invalidation | Manual | Tags |
| Generated React hooks | ❌ | ✅ |
| Redux integration | Manual | Built for Redux Toolkit |
| Automatic refetching | Manual | Supported |
| API data management | Mostly manual | Designed for it |

---

# 46. 🧠 Important Understanding

Do **not** think:

```text
Axios is bad
RTK Query is good
```

Instead:

```text
Axios
 ↓
HTTP client


RTK Query
 ↓
API data fetching + caching solution
```

You choose based on the architecture and requirements of the application.

---

# 47. ⚠️ Common Mistake #1

Forgetting the API reducer:

```js
[userApi.reducerPath]: userApi.reducer
```

Without the API reducer, RTK Query's Redux state isn't configured correctly.

---

# 48. ⚠️ Common Mistake #2

Forgetting middleware:

```js
middleware: (getDefaultMiddleware) =>
    getDefaultMiddleware().concat(
        userApi.middleware
    )
```

The API middleware is an important part of RTK Query setup.

---

# 49. ⚠️ Common Mistake #3

Confusing query and mutation.

Remember:

```text
GET
 ↓
Query


POST / PUT / PATCH / DELETE
 ↓
Mutation
```

---

# 50. ⚠️ Common Mistake #4

Using the wrong generated hook name.

Endpoint:

```js
getProducts
```

Hook:

```js
useGetProductsQuery()
```

Endpoint:

```js
createProduct
```

Hook:

```js
useCreateProductMutation()
```

---

# 51. ⚠️ Common Mistake #5

Forgetting optional chaining while data may initially be unavailable.

Instead of:

```jsx
data.map(...)
```

you can often safely use:

```jsx
data?.map(...)
```

depending on your component's rendering logic.

---

# 52. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is RTK Query?

RTK Query is a data-fetching and caching solution included with Redux Toolkit.

### Q2. What is an API slice?

An API slice defines API endpoints and manages their request-related state and caching behavior.

### Q3. What is a query?

A query is generally used to retrieve data.

### Q4. What is a mutation?

A mutation is generally used to create, update, or delete server-side data.

---

# 53. 🟡 Intermediate

### Q5. What is `createApi()`?

It is used to create an RTK Query API slice.

### Q6. What is `fetchBaseQuery()`?

It is a lightweight request base query implementation provided by RTK Query, built around `fetch()`.

### Q7. What does RTK Query provide automatically?

It can provide generated hooks and manage request state, caching, subscriptions, and related API-data behavior.

### Q8. What are `providesTags` and `invalidatesTags`?

They are used to associate cached query data with tags and invalidate related cached data after mutations.

---

# 54. 🔴 Advanced

### Q9. What is the difference between Axios and RTK Query?

Axios is primarily an HTTP client. RTK Query is designed to manage API data fetching and caching as part of Redux Toolkit.

### Q10. Why use RTK Query instead of manually writing `createAsyncThunk()` for every API request?

RTK Query can reduce repetitive API-state management code by providing standardized query/mutation hooks, caching, invalidation, and request-state handling.

---

# 55. 📝 Practice

## Practice 1 — Product API

Create:

```text
productApi.js
```

with:

```text
getProducts
```

Use:

```js
builder.query()
```

Display:

```text
Product Name
Price
```

---

## Practice 2 — Single Product

Create:

```text
getProduct
```

that accepts an ID.

Concept:

```js
getProduct: builder.query({
    query: (id) => `/products/${id}`
})
```

Then:

```js
useGetProductQuery(5)
```

---

## Practice 3 — Create Product

Create:

```text
createProduct
```

using:

```js
builder.mutation()
```

Send:

```js
{
    name: "Laptop",
    price: 55000
}
```

---

## Practice 4 — Update Product

Create:

```text
updateProduct
```

using:

```text
PUT
```

---

## Practice 5 — Delete Product

Create:

```text
deleteProduct
```

using:

```text
DELETE
```

---

# 56. 🧩 MINI CHALLENGE

Build:

# 🛒 RTK Query Product Management App

Your application should support:

```text
GET
POST
PUT
DELETE
```

Architecture:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   └── products/
│       └── productApi.js
│
├── App.jsx
└── main.jsx
```

Endpoints:

```text
getProducts
getProduct
createProduct
updateProduct
deleteProduct
```

Use:

```text
builder.query()
builder.mutation()
```

Add:

```text
providesTags
invalidatesTags
```

UI should show:

```text
⏳ Loading...

❌ Error...

📦 Products
```

---

# 57. 📝 10-Question Quiz

### Q1. What does RTK Query mainly help with?

**Answer:** API data fetching, caching, request state, and related server-data management.

---

### Q2. Which function creates an RTK Query API slice?

**Answer:**

```js
createApi()
```

---

### Q3. Which helper is commonly used for basic HTTP requests?

**Answer:**

```js
fetchBaseQuery()
```

---

### Q4. What is `builder.query()` generally used for?

**Answer:** Reading/fetching data.

---

### Q5. What is `builder.mutation()` generally used for?

**Answer:** Creating, updating, or deleting data.

---

### Q6. What does RTK Query caching mean?

**Answer:** Previously fetched API data can be retained and reused according to RTK Query's cache and subscription behavior.

---

### Q7. What does `providesTags` do?

**Answer:** It associates query results with cache tags.

---

### Q8. What does `invalidatesTags` do?

**Answer:** It marks related cached data as invalid so affected queries can be refetched when appropriate.

---

### Q9. What is the generated hook for:

```js
getUsers
```

**Answer:**

```js
useGetUsersQuery()
```

---

### Q10. What is the main difference between Axios and RTK Query?

**Answer:**

```text
Axios
→ HTTP client

RTK Query
→ API data fetching + caching solution
```

---

# 🧠 FINAL MEMORY TRICK

Remember this:

```text
🏗️ createApi()
      ↓
🌐 API Slice
      ↓
┌───────────────┐
│               │
↓               ↓
🔍 Query       ✏️ Mutation
│               │
GET             POST/PUT/PATCH/DELETE
│               │
↓               ↓
📦 Data         🔄 Change Data
│               │
└───────┬───────┘
        ↓
     💾 Cache
        ↓
   🔄 Refetch /
   Invalidation
        ↓
      ⚛️ React
```

### ⭐ Most important rules

```text
🔍 Query
   ↓
Read data


✏️ Mutation
   ↓
Change data


💾 Cache
   ↓
Reuse API data


🏷️ providesTags
   ↓
Identify cached data


♻️ invalidatesTags
   ↓
Invalidate related cached data
```

### ⭐ One-line memory

> **RTK Query gives React + Redux a structured way to fetch, cache, update, and refetch API data without writing all the API-state management manually.**

---

# ✅ CHAPTER 35 COMPLETE

You now understand:

- 🧠 RTK Query
- 🏗️ API slices
- `createApi()`
- `fetchBaseQuery()`
- 🔍 Queries
- ✏️ Mutations
- ⏳ Loading states
- ❌ Error states
- 💾 Caching
- 🔄 Automatic refetching
- 🏷️ Tags
- `providesTags`
- `invalidatesTags`
- 🆚 RTK Query vs Axios
- 🧪 CRUD API structure

---

# 🚀 NEXT CHAPTER

## Chapter 36 — Custom Hooks

We will learn:

```text
🪝 What is a Custom Hook?
🤔 Why Custom Hooks?
📜 Rules of Hooks
📡 useFetch
💾 useLocalStorage
⏱️ useDebounce
🔘 useToggle
📝 useForm
♻️ Reusable Logic
```

Type **NEXT** to continue.