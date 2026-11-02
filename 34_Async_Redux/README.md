# ⏳ ReactJS — Chapter 34
# Async Redux

---

# 🎯 What You Will Learn

In this chapter:

1. 🧠 What is Async Redux?
2. 🤔 Why do we need async Redux?
3. ⏳ What is `createAsyncThunk()`?
4. 📡 Making API calls
5. 🔄 Loading state
6. ❌ Error state
7. ✅ Success state
8. 🔍 `pending`, `fulfilled`, `rejected`
9. 🚀 `useDispatch()`
10. 🔎 `useSelector()`
11. 🌐 Redux + Axios
12. 🔐 Authentication with Redux
13. 🧪 Complete API example
14. ⚠️ Common mistakes
15. 💼 Interview questions
16. 📝 Practice
17. 🧩 Mini challenge
18. 📝 Quiz
19. 🧠 Final memory trick

---

# 1. 🧠 What is Async Redux?

In the previous chapter, we worked with synchronous actions.

For example:

```js
dispatch(increment());
```

This happens immediately.

But real applications need to communicate with APIs.

For example:

```text
React
  ↓
API
  ↓
Database
  ↓
Response
```

An API request takes time.

Therefore, we need to handle:

```text
⏳ Loading
✅ Success
❌ Error
```

This is where **Async Redux** becomes useful.

---

# 2. 🌐 Real-World Example

Suppose our React application needs to load users.

```text
React Application
       ↓
   API Request
       ↓
    Server
       ↓
   Database
       ↓
    Response
       ↓
React Application
```

During the request:

```text
Loading...
```

If successful:

```text
Users displayed
```

If something goes wrong:

```text
Failed to load users
```

So we need three states:

```js
{
    data: [],
    loading: false,
    error: null
}
```

---

# 3. 🔄 Async Redux Flow

The basic flow becomes:

```text
Component
    ↓
dispatch(fetchUsers())
    ↓
createAsyncThunk
    ↓
API request
    ↓
┌─────────────┬──────────────┐
↓             ↓              ↓
pending     fulfilled      rejected
↓             ↓              ↓
loading     success        error
```

This is one of the most important concepts in this chapter.

---

# 4. 🚀 What is `createAsyncThunk()`?

`createAsyncThunk()` is a Redux Toolkit function for handling asynchronous operations such as API requests.

Example:

```js
const fetchUsers = createAsyncThunk(
    "users/fetchUsers",
    async () => {
        // API request
    }
);
```

Think:

```text
createAsyncThunk()
       ↓
Async operation
       ↓
API request
```

---

# 5. 🧠 Why Use `createAsyncThunk()`?

Without a structured async pattern, you may have to manually manage:

```text
Start request
Set loading
Call API
Handle success
Set data
Handle error
Set error
```

`createAsyncThunk()` gives us a standard Redux Toolkit pattern.

It automatically generates three lifecycle action states:

```text
pending
fulfilled
rejected
```

Remember:

```text
⏳ pending
   ↓
Request is running

✅ fulfilled
   ↓
Request succeeded

❌ rejected
   ↓
Request failed
```

---

# 6. 📦 Install Axios

For this chapter, we will also see Redux + Axios.

Install Axios:

```bash
npm install axios
```

Then:

```js
import axios from "axios";
```

---

# 7. 📁 Project Structure

A simple project:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   └── users/
│       └── userSlice.js
│
├── App.jsx
└── main.jsx
```

We will put the async Redux logic inside:

```text
features/users/userSlice.js
```

---

# 8. 🧱 Initial State

Let's start with:

```js
const initialState = {
    users: [],
    loading: false,
    error: null
};
```

Understand it carefully.

### `users`

```js
users: []
```

Stores API data.

### `loading`

```js
loading: false
```

Tells us whether the API request is running.

### `error`

```js
error: null
```

Stores error information.

So:

```text
State
│
├── users
├── loading
└── error
```

---

# 9. 📡 Create `fetchUsers`

Import:

```js
import {
    createAsyncThunk,
    createSlice
} from "@reduxjs/toolkit";

import axios from "axios";
```

Now:

```js
export const fetchUsers = createAsyncThunk(
    "users/fetchUsers",

    async () => {

        const response = await axios.get(
            "https://jsonplaceholder.typicode.com/users"
        );

        return response.data;
    }
);
```

---

# 10. 🔍 Understand This Code

```js
createAsyncThunk(
```

Creates an async Redux operation.

---

### First argument

```js
"users/fetchUsers"
```

This is the action type prefix.

Think:

```text
users
  /
fetchUsers
```

---

### Second argument

```js
async () => {
```

This function performs the asynchronous operation.

---

### API request

```js
const response = await axios.get(
    "https://jsonplaceholder.typicode.com/users"
);
```

Axios sends a GET request.

---

### Return data

```js
return response.data;
```

The returned value becomes available to the fulfilled action.

---

# 11. 🧠 Important

`createAsyncThunk()` automatically creates lifecycle actions based on the thunk name.

For:

```js
"users/fetchUsers"
```

Redux Toolkit generates conceptually:

```text
users/fetchUsers/pending

users/fetchUsers/fulfilled

users/fetchUsers/rejected
```

We don't manually create these three actions.

---

# 12. 🔄 `extraReducers`

Now we need to tell our slice what to do when those lifecycle actions occur.

We use:

```js
extraReducers
```

Example:

```js
extraReducers: (builder) => {

    builder

        .addCase(fetchUsers.pending, (state) => {
            state.loading = true;
        })

        .addCase(fetchUsers.fulfilled, (state, action) => {
            state.loading = false;
            state.users = action.payload;
        })

        .addCase(fetchUsers.rejected, (state, action) => {
            state.loading = false;
            state.error = action.error.message;
        });
}
```

---

# 13. ⏳ Pending

When the API request starts:

```js
.addCase(fetchUsers.pending, (state) => {
    state.loading = true;
})
```

State becomes:

```js
{
    users: [],
    loading: true,
    error: null
}
```

UI can display:

```text
⏳ Loading users...
```

---

# 14. ✅ Fulfilled

If the API request succeeds:

```js
.addCase(fetchUsers.fulfilled, (state, action) => {
    state.loading = false;
    state.users = action.payload;
})
```

Suppose API returns:

```js
[
    {
        id: 1,
        name: "Leanne Graham"
    }
]
```

Then:

```text
action.payload
       ↓
users
```

State becomes:

```js
{
    users: [
        {
            id: 1,
            name: "Leanne Graham"
        }
    ],

    loading: false,

    error: null
}
```

---

# 15. ❌ Rejected

If the request fails:

```js
.addCase(fetchUsers.rejected, (state, action) => {
    state.loading = false;
    state.error = action.error.message;
});
```

State becomes something like:

```js
{
    users: [],
    loading: false,
    error: "Network Error"
}
```

UI can show:

```text
❌ Failed to load users
```

---

# 16. 🧩 Complete `userSlice.js`

Now let's combine everything.

```js
import {
    createAsyncThunk,
    createSlice
} from "@reduxjs/toolkit";

import axios from "axios";


const initialState = {
    users: [],
    loading: false,
    error: null
};


export const fetchUsers = createAsyncThunk(
    "users/fetchUsers",

    async () => {

        const response = await axios.get(
            "https://jsonplaceholder.typicode.com/users"
        );

        return response.data;
    }
);


const userSlice = createSlice({

    name: "users",

    initialState,

    reducers: {},

    extraReducers: (builder) => {

        builder

            .addCase(fetchUsers.pending, (state) => {

                state.loading = true;
                state.error = null;

            })

            .addCase(fetchUsers.fulfilled, (state, action) => {

                state.loading = false;
                state.users = action.payload;

            })

            .addCase(fetchUsers.rejected, (state, action) => {

                state.loading = false;
                state.error = action.error.message;

            });
    }
});


export default userSlice.reducer;
```

---

# 17. 🏪 Add It to the Store

Open:

```text
src/app/store.js
```

Write:

```js
import { configureStore } from "@reduxjs/toolkit";

import userReducer
    from "../features/users/userSlice";


export const store = configureStore({

    reducer: {

        users: userReducer

    }

});
```

Now our Redux state looks like:

```js
{
    users: {
        users: [],
        loading: false,
        error: null
    }
}
```

Notice the two `users`.

```text
state
 ↓
users
 ↓
users
```

The first is the reducer key.

The second is the property inside the slice state.

Therefore:

```js
state.users.users
```

gets the actual users array.

---

# 18. 🔗 Connect Redux to React

Our `main.jsx` remains:

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

# 19. 🚀 Dispatch the Async Thunk

Now open:

```text
App.jsx
```

Import:

```jsx
import {
    useDispatch,
    useSelector
} from "react-redux";

import {
    fetchUsers
} from "./features/users/userSlice";
```

Then:

```jsx
import { useEffect } from "react";


function App() {

    const dispatch = useDispatch();

    const {
        users,
        loading,
        error
    } = useSelector(
        (state) => state.users
    );


    useEffect(() => {

        dispatch(fetchUsers());

    }, [dispatch]);


    return (
        <div>

            <h1>Users</h1>

        </div>
    );
}


export default App;
```

---

# 20. 🔍 Why `useEffect()`?

We want to fetch users when the component loads.

Therefore:

```js
useEffect(() => {
    dispatch(fetchUsers());
}, [dispatch]);
```

means:

```text
Component loads
      ↓
useEffect runs
      ↓
dispatch(fetchUsers())
      ↓
API request
```

---

# 21. 🖥️ Display Loading

Let's improve the UI.

```jsx
if (loading) {
    return <h2>⏳ Loading...</h2>;
}
```

So:

```text
API request running
       ↓
loading = true
       ↓
Loading...
```

---

# 22. ❌ Display Error

Add:

```jsx
if (error) {
    return <h2>❌ {error}</h2>;
}
```

Flow:

```text
API fails
   ↓
rejected
   ↓
error stored
   ↓
UI displays error
```

---

# 23. ✅ Display Users

Now:

```jsx
return (
    <div>

        <h1>Users</h1>

        {users.map((user) => (
            <div key={user.id}>

                <h2>{user.name}</h2>

                <p>{user.email}</p>

            </div>
        ))}

    </div>
);
```

---

# 24. 🎯 Complete `App.jsx`

```jsx
import { useEffect } from "react";

import {
    useDispatch,
    useSelector
} from "react-redux";

import {
    fetchUsers
} from "./features/users/userSlice";


function App() {

    const dispatch = useDispatch();


    const {
        users,
        loading,
        error
    } = useSelector(
        (state) => state.users
    );


    useEffect(() => {

        dispatch(fetchUsers());

    }, [dispatch]);


    if (loading) {

        return (
            <h2>
                ⏳ Loading users...
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

            <h1>Users</h1>

            {users.map((user) => (

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

# 25. 🧪 Complete Dry Run

Let's understand exactly what happens.

Initial state:

```js
{
    users: [],
    loading: false,
    error: null
}
```

---

## STEP 1 — Component renders

```text
App
 ↓
useEffect()
```

---

## STEP 2 — Dispatch

```js
dispatch(fetchUsers());
```

---

## STEP 3 — Pending

Redux Toolkit dispatches:

```text
users/fetchUsers/pending
```

Reducer:

```js
state.loading = true;
```

State:

```js
{
    users: [],
    loading: true,
    error: null
}
```

UI:

```text
⏳ Loading users...
```

---

# 26. 📡 STEP 4 — API Request

Axios sends:

```text
GET
 ↓
https://jsonplaceholder.typicode.com/users
```

Server processes the request.

---

# 27. ✅ STEP 5 — Success

Suppose API returns:

```text
10 users
```

Then the thunk resolves.

Redux Toolkit dispatches:

```text
users/fetchUsers/fulfilled
```

Reducer:

```js
state.loading = false;
state.users = action.payload;
```

State becomes:

```text
users = [10 users]
loading = false
error = null
```

UI:

```text
Leanne Graham
Ervin Howell
Clementine Bauch
...
```

---

# 28. ❌ What If the API Fails?

Suppose:

```text
Server unavailable
```

Then:

```text
fetchUsers
   ↓
rejected
```

Reducer:

```js
state.loading = false;
state.error = action.error.message;
```

UI:

```text
❌ Network Error
```

---

# 29. 🧠 The Three States

This is extremely important.

## ⏳ Pending

```text
Request started
```

State:

```js
loading: true
```

---

## ✅ Fulfilled

```text
Request succeeded
```

State:

```js
loading: false
data: [...]
```

---

## ❌ Rejected

```text
Request failed
```

State:

```js
loading: false
error: "..."
```

Memory:

```text
P → F / R

Pending
   ↓
Fulfilled OR Rejected
```

---

# 30. 📊 State Machine

You can visualize it like this:

```text
             API REQUEST
                  │
                  ↓
             ⏳ PENDING
             loading=true
                  │
          ┌───────┴────────┐
          ↓                ↓
     ✅ FULFILLED      ❌ REJECTED
          │                │
          ↓                ↓
     data stored       error stored
          │                │
          ↓                ↓
     loading=false     loading=false
```

---

# 31. 🎁 Understanding `action.payload`

In the fulfilled reducer:

```js
.addCase(
    fetchUsers.fulfilled,
    (state, action) => {

        state.users = action.payload;

    }
)
```

What is:

```js
action.payload
```

?

It is the value returned by:

```js
return response.data;
```

So:

```text
API response
     ↓
response.data
     ↓
return
     ↓
action.payload
     ↓
state.users
```

This connection is very important.

---

# 32. 🛠️ Better Error Handling

We can make the thunk explicitly handle errors.

Example:

```js
export const fetchUsers = createAsyncThunk(
    "users/fetchUsers",

    async (_, thunkAPI) => {

        try {

            const response = await axios.get(
                "https://jsonplaceholder.typicode.com/users"
            );

            return response.data;

        } catch (error) {

            return thunkAPI.rejectWithValue(
                error.message
            );

        }
    }
);
```

Then:

```js
.addCase(fetchUsers.rejected, (state, action) => {

    state.loading = false;

    state.error =
        action.payload || action.error.message;

});
```

This gives us more control over the rejected value.

---

# 33. 🧠 What is `rejectWithValue()`?

`rejectWithValue()` lets us send a custom value when an async operation fails.

Example:

```js
return thunkAPI.rejectWithValue(
    "Unable to load users"
);
```

Then:

```js
action.payload
```

contains:

```text
Unable to load users
```

Think:

```text
API error
   ↓
rejectWithValue()
   ↓
action.payload
   ↓
Redux state
```

---

# 34. 🌐 Redux + Axios

The combination is common:

```text
React
 ↓
Redux Toolkit
 ↓
createAsyncThunk
 ↓
Axios
 ↓
REST API
```

Example:

```js
const response = await axios.get("/api/users");
```

POST:

```js
const response = await axios.post(
    "/api/users",
    userData
);
```

PUT:

```js
const response = await axios.put(
    `/api/users/${id}`,
    userData
);
```

DELETE:

```js
await axios.delete(
    `/api/users/${id}`
);
```

---

# 35. 🔐 Authentication with Redux

Redux can also manage authentication-related application state.

Example:

```js
const initialState = {
    user: null,
    token: null,
    loading: false,
    error: null
};
```

Possible async operation:

```text
login
```

Flow:

```text
Login Form
    ↓
dispatch(loginUser(credentials))
    ↓
createAsyncThunk
    ↓
POST /login
    ↓
Backend
    ↓
Token
    ↓
fulfilled
    ↓
Redux Auth State
```

---

# 36. 🔐 Login Example

Conceptually:

```js
export const loginUser = createAsyncThunk(
    "auth/login",

    async (credentials) => {

        const response = await axios.post(
            "/api/login",
            credentials
        );

        return response.data;
    }
);
```

Suppose the server returns:

```js
{
    user: {
        id: 1,
        name: "Ramesh"
    },
    token: "..."
}
```

Then:

```js
.addCase(loginUser.fulfilled, (state, action) => {

    state.user = action.payload.user;
    state.token = action.payload.token;

});
```

---

# 37. 🔄 Authentication Flow

```text
Login Form
    ↓
Email + Password
    ↓
dispatch(loginUser())
    ↓
createAsyncThunk
    ↓
Axios POST
    ↓
Backend
    ↓
Authentication
    ↓
Token + User
    ↓
Redux State
    ↓
Application
```

This pattern will become especially useful when you later connect React with your **FastAPI / Node.js backend**.

---

# 38. ⚠️ Important Security Note

Be careful with authentication tokens.

Do not assume that storing sensitive authentication information in browser storage is automatically secure.

For production authentication, token storage and cookie strategy should be designed according to the backend architecture and security requirements.

We will study authentication more deeply in:

```text
Chapter 27 — Authentication
```

and later in:

```text
Chapter 41 — React + Node.js
Chapter 44 — React + FastAPI
```

---

# 39. ⚠️ Common Mistake #1

Forgetting to dispatch the thunk.

Wrong:

```js
fetchUsers();
```

Correct:

```js
dispatch(fetchUsers());
```

---

# 40. ⚠️ Common Mistake #2

Forgetting `extraReducers`.

You created:

```js
createAsyncThunk()
```

but didn't handle:

```text
pending
fulfilled
rejected
```

Then your UI won't properly reflect loading/error/success state.

---

# 41. ⚠️ Common Mistake #3

Wrong selector.

If store contains:

```js
reducer: {
    users: userReducer
}
```

and slice state contains:

```js
{
    users: [],
    loading: false
}
```

then:

```js
state.users.users
```

is the users array.

---

# 42. ⚠️ Common Mistake #4

Forgetting `key` in `.map()`.

Wrong:

```jsx
users.map((user) => (
    <h2>{user.name}</h2>
))
```

Correct:

```jsx
users.map((user) => (
    <h2 key={user.id}>
        {user.name}
    </h2>
))
```

---

# 43. ⚠️ Common Mistake #5

Showing the old data while a new request is loading without thinking about the desired UX.

For example, when fetching again:

```js
state.loading = true;
```

You should decide whether the UI should:

```text
Show spinner
```

or:

```text
Keep old data + show small loading indicator
```

Both can be valid depending on the application.

---

# 44. 🧠 Important Pattern

For most API-based Redux state, think:

```js
{
    data: [],
    loading: false,
    error: null
}
```

This simple pattern is extremely useful.

For example:

### Users

```js
{
    users: [],
    loading: false,
    error: null
}
```

### Products

```js
{
    products: [],
    loading: false,
    error: null
}
```

### Orders

```js
{
    orders: [],
    loading: false,
    error: null
}
```

---

# 45. 💼 Interview Questions

## 🟢 Beginner

### Q1. What is Async Redux?

Async Redux refers to managing asynchronous operations such as API requests through Redux state management.

### Q2. What is `createAsyncThunk()`?

It is a Redux Toolkit utility for creating async logic and automatically generating `pending`, `fulfilled`, and `rejected` lifecycle actions.

### Q3. What are the three states of an async thunk?

```text
pending
fulfilled
rejected
```

---

## 🟡 Intermediate

### Q4. What is `extraReducers`?

`extraReducers` lets a slice respond to actions created elsewhere, including the lifecycle actions generated by `createAsyncThunk()`.

### Q5. What is `action.payload` in `fulfilled`?

It is typically the value returned by the async thunk.

Example:

```js
return response.data;
```

becomes available as:

```js
action.payload
```

### Q6. How do you manage loading state?

For example:

```js
.addCase(fetchUsers.pending, (state) => {
    state.loading = true;
})
```

---

## 🔴 Advanced

### Q7. How do you handle errors with `createAsyncThunk()`?

Use the `rejected` lifecycle case and, when needed, `rejectWithValue()` for custom error payloads.

### Q8. Why use Redux for API state?

Redux can provide a centralized and predictable place to manage shared server-related state, including data, loading status, and errors.

### Q9. What is the difference between `pending`, `fulfilled`, and `rejected`?

```text
pending
 → request started

fulfilled
 → request succeeded

rejected
 → request failed
```

### Q10. What is the role of Axios?

Axios is an HTTP client used to send requests to APIs. It can be used inside async Redux logic such as `createAsyncThunk()`.

---

# 46. 📝 Practice

## Practice 1 — Products API

Create:

```text
productSlice.js
```

State:

```js
{
    products: [],
    loading: false,
    error: null
}
```

Create:

```text
fetchProducts
```

using:

```js
createAsyncThunk()
```

Handle:

```text
pending
fulfilled
rejected
```

---

## Practice 2 — Single Product

Create:

```text
fetchProductById
```

The thunk should receive:

```js
id
```

Example:

```js
dispatch(fetchProductById(5));
```

---

## Practice 3 — POST

Create:

```text
createProduct
```

The thunk should receive product data.

Example:

```js
dispatch(
    createProduct({
        name: "Laptop",
        price: 55000
    })
);
```

---

# 47. 🧩 MINI CHALLENGE

Build a:

# 👥 User Management Redux App

Requirements:

```text
Fetch Users
Loading
Error
Success
```

State:

```js
{
    users: [],
    loading: false,
    error: null
}
```

Create:

```text
fetchUsers
```

using:

```js
createAsyncThunk()
```

Architecture:

```text
src/
│
├── app/
│   └── store.js
│
├── features/
│   └── users/
│       └── userSlice.js
│
├── App.jsx
└── main.jsx
```

UI:

```text
👥 Users

⏳ Loading...
```

or:

```text
❌ Error
```

or:

```text
👤 User 1
📧 Email

👤 User 2
📧 Email
```

---

# 48. 📝 10-Question Quiz

### Q1. What function is commonly used for async Redux logic?

**Answer:**

```js
createAsyncThunk()
```

---

### Q2. What happens in `pending`?

**Answer:** The asynchronous operation has started and is still running.

---

### Q3. What happens in `fulfilled`?

**Answer:** The asynchronous operation completed successfully.

---

### Q4. What happens in `rejected`?

**Answer:** The asynchronous operation failed.

---

### Q5. Which property commonly stores API data?

**Answer:** A state property such as:

```js
data: []
```

or:

```js
users: []
```

---

### Q6. Where do we usually handle thunk lifecycle actions inside a slice?

**Answer:**

```js
extraReducers
```

---

### Q7. What does `action.payload` usually contain in a fulfilled thunk?

**Answer:** The value returned by the thunk.

---

### Q8. How do we start an async thunk from a component?

**Answer:**

```js
dispatch(fetchUsers());
```

---

### Q9. What can Axios be used for?

**Answer:** Sending HTTP requests such as GET, POST, PUT, and DELETE requests.

---

### Q10. What is the most important async Redux pattern?

**Answer:**

```text
⏳ Pending
   ↓
   ├──→ ✅ Fulfilled
   │
   └──→ ❌ Rejected
```

---

# 🧠 FINAL MEMORY TRICK

Remember:

```text
createAsyncThunk()
        ↓
     API CALL
        ↓
   ┌────┼────┐
   ↓    ↓    ↓
Pending Fulfilled Rejected
   ↓    ↓    ↓
Loading Data   Error
```

And the complete React flow:

```text
👤 USER
   ↓
⚛️ REACT COMPONENT
   ↓
🚀 dispatch(fetchUsers())
   ↓
⏳ createAsyncThunk
   ↓
🌐 AXIOS
   ↓
🖥️ API SERVER
   ↓
┌───────────────┐
│               │
↓               ↓
✅ Success     ❌ Error
│               │
↓               ↓
fulfilled      rejected
│               │
↓               ↓
Redux State    Redux State
│               │
└───────┬───────┘
        ↓
   🔍 useSelector()
        ↓
      🖥️ UI
```

### ⭐ One-line memory

> **`createAsyncThunk()` handles async work, while `pending`, `fulfilled`, and `rejected` manage loading, success, and error states.**

---

# ✅ CHAPTER 34 COMPLETE

You now understand:

- ⏳ Async Redux
- `createAsyncThunk()`
- API calls
- `pending`
- `fulfilled`
- `rejected`
- Loading state
- Success state
- Error state
- `extraReducers`
- `action.payload`
- `rejectWithValue()`
- Redux + Axios
- Authentication flow with Redux
- Complete API example

---

# 🚀 NEXT CHAPTER

## Chapter 35 — RTK Query

We will learn:

```text
📡 What is RTK Query?
🏗️ API Slice
🔍 Queries
✏️ Mutations
💾 Caching
⏳ Loading states
🔄 Automatic refetching
🆚 RTK Query vs Axios
```

This is an important step because **RTK Query can remove much of the manual API-loading/error/cache code we just wrote.**

Type **NEXT** to continue.