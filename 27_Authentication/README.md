# 🔐 CHAPTER 27 — AUTHENTICATION

## 🎯 Goal of This Chapter

Authentication is one of the most important topics in a real-world React application.

You already learned:

```text
React
 ↓
Forms
 ↓
Validation
 ↓
API
```

Now we connect everything:

```text
Login Form
    ↓
Backend API
    ↓
Verify User
    ↓
Token
    ↓
React
    ↓
Remember Login
    ↓
Protected Pages
```

For example:

```text
E-Commerce Website

Login
 ↓
Products
 ↓
Cart
 ↓
Checkout
 ↓
Orders
```

A user should not be able to access every page without logging in.

---

# 📚 CHAPTER TOPICS

We will learn:

1. What is authentication?
2. Authentication vs authorization
3. Register
4. Login
5. Logout
6. Authentication flow
7. JWT
8. Access token
9. Refresh token
10. LocalStorage
11. Cookies
12. Protected routes
13. Auth Context
14. Role-based authentication
15. Complete authentication architecture
16. Common mistakes
17. Best practices
18. Interview questions
19. Practice tasks
20. Mini challenge
21. Quiz
22. Final revision

---

# 1️⃣ WHAT IS AUTHENTICATION?

Authentication means:

> **Checking who the user is.**

Simple example:

You enter:

```text
Email: ramesh@gmail.com
Password: ********
```

The backend checks:

```text
Is this user real?
Are the credentials correct?
```

If yes:

```text
✅ Login successful
```

---

# 2️⃣ REAL-WORLD ANALOGY 🏢

Imagine entering an office.

At the security desk:

```text
You
 ↓
Show ID card
 ↓
Security checks ID
 ↓
Valid?
 ┌───┴───┐
YES     NO
 ↓       ↓
Enter   Reject
```

This is authentication.

---

# 3️⃣ AUTHENTICATION vs AUTHORIZATION

These two words are very important.

## 🔐 Authentication

Means:

> **Who are you?**

Example:

```text
Login with email/password
```

---

## 🛡️ Authorization

Means:

> **What are you allowed to do?**

Example:

```text
Admin
 ↓
Can delete users

Normal User
 ↓
Cannot delete users
```

Remember:

```text
Authentication
= Who are you?

Authorization
= What can you do?
```

---

# 4️⃣ REGISTER 📝

Registration means creating a new user account.

Example:

```text
┌─────────────────────────┐
│      Register           │
│                         │
│ Name     [__________]   │
│ Email    [__________]   │
│ Password [__________]   │
│                         │
│       [ Register ]      │
└─────────────────────────┘
```

Flow:

```text
User
 ↓
Register Form
 ↓
Validation
 ↓
POST /register
 ↓
Backend
 ↓
Database
 ↓
User created
```

---

# 5️⃣ LOGIN 🔑

Login means verifying an existing user.

Example:

```text
Email
Password
```

Flow:

```text
Login Form
    ↓
POST /login
    ↓
Backend
    ↓
Check email/password
    ↓
Correct?
 ┌───┴────┐
YES       NO
 ↓         ↓
Token     Error
```

---

# 6️⃣ LOGOUT 🚪

Logout means ending the user's authenticated session from the application's perspective.

For example:

```text
User logged in
      ↓
Token/session available
      ↓
User clicks Logout
      ↓
Authentication information removed/invalidated
      ↓
User becomes logged out
```

Then the user may need to log in again to access protected resources.

---

# 7️⃣ COMPLETE AUTHENTICATION FLOW

This is one of the most important diagrams in this chapter.

```text
                USER
                  │
                  ↓
             Login Form
                  │
                  ↓
             React App
                  │
                  ↓
           POST /login
                  │
                  ↓
              Backend
                  │
          ┌───────┴────────┐
          ↓                ↓
       Valid             Invalid
          ↓                ↓
       Token             Error
          ↓
       React
          ↓
 Store authentication information
          ↓
 Access protected resources
```

---

# 8️⃣ WHAT IS JWT? 🔐

JWT means:

> **JSON Web Token**

A JWT is a token commonly used to represent authenticated information between a client and server.

After successful login, a backend may return a token.

Example:

```text
Login
 ↓
Backend verifies credentials
 ↓
JWT generated
 ↓
React receives token
```

The token can then be sent with requests to protected backend APIs.

---

# 9️⃣ JWT SIMPLE ANALOGY 🎫

Think about an event.

You buy a ticket.

```text
Buy ticket
   ↓
Ticket received
   ↓
Enter event
   ↓
Security checks ticket
   ↓
Valid?
```

A token works similarly as a credential presented to the backend.

---

# 🔟 JWT STRUCTURE

A JWT commonly contains three parts:

```text
HEADER.PAYLOAD.SIGNATURE
```

For example:

```text
xxxxx.yyyyy.zzzzz
```

### Header

Contains information about the token type and signing algorithm.

### Payload

Contains claims/data.

### Signature

Used to verify that the token has not been tampered with.

---

# 1️⃣1️⃣ IMPORTANT JWT POINT ⚠️

JWT payload is **not the same as encrypted secret storage**.

A JWT can commonly be decoded to inspect its payload.

Therefore:

> **Do not put passwords or sensitive secrets inside the JWT payload.**

---

# 1️⃣2️⃣ ACCESS TOKEN

An access token is used to access protected resources.

Example:

```text
React
 ↓
GET /profile
 ↓
Access Token
 ↓
Backend
 ↓
Verify token
 ↓
Return profile
```

A request may look conceptually like:

```http
Authorization: Bearer ACCESS_TOKEN
```

---

# 1️⃣3️⃣ WHAT IS `Bearer`?

You may see:

```http
Authorization: Bearer eyJ...
```

The general format is:

```text
Authorization: Bearer <token>
```

Meaning:

```text
Authorization
    ↓
Bearer
    ↓
Token
```

The backend uses this information to authenticate the request.

---

# 1️⃣4️⃣ REFRESH TOKEN

An access token is often designed to have a relatively short lifetime.

A refresh token can be used to obtain a new access token without requiring the user to enter their credentials again.

Conceptually:

```text
Access Token
     ↓
Expires
     ↓
Refresh Token
     ↓
Backend
     ↓
New Access Token
```

---

# 1️⃣5️⃣ ACCESS TOKEN vs REFRESH TOKEN

| Access Token | Refresh Token |
|---|---|
| Used for API access | Used to obtain a new access token |
| Usually shorter lifetime | Often longer lifetime |
| Sent with protected API requests | Used during token renewal |
| Protects access to resources | Helps maintain the session |

### 🧠 Memory trick

```text
Access
= Access API

Refresh
= Refresh access
```

---

# 1️⃣6️⃣ AUTHENTICATION FLOW WITH BOTH TOKENS

```text
Login
  ↓
Backend
  ↓
Access Token + Refresh Token
  ↓
React / Browser
  ↓
API requests use Access Token
  ↓
Access Token expires
  ↓
Refresh request
  ↓
Backend validates Refresh Token
  ↓
New Access Token
  ↓
Continue using application
```

---

# 1️⃣7️⃣ LOCALSTORAGE

`localStorage` is browser storage.

Example:

```javascript
localStorage.setItem(
    "token",
    token
);
```

Read:

```javascript
const token =
    localStorage.getItem("token");
```

Remove:

```javascript
localStorage.removeItem("token");
```

---

# 1️⃣8️⃣ LOCALSTORAGE AUTHENTICATION EXAMPLE

After login:

```javascript
localStorage.setItem(
    "token",
    data.accessToken
);
```

Later:

```javascript
const token =
    localStorage.getItem("token");
```

Then:

```javascript
fetch("/profile", {
    headers: {
        Authorization:
            `Bearer ${token}`
    }
});
```

---

# 1️⃣9️⃣ LOCALSTORAGE FLOW

```text
Login
 ↓
Backend
 ↓
Access Token
 ↓
localStorage
 ↓
Browser remembers token
 ↓
API request
 ↓
Authorization header
```

---

# 2️⃣0️⃣ IMPORTANT SECURITY WARNING ⚠️

Be careful with storing sensitive authentication credentials in browser storage.

If an attacker can execute malicious JavaScript in your application through an XSS vulnerability, data stored in JavaScript-accessible storage can potentially be exposed.

Therefore, authentication storage should be designed carefully.

For many production authentication systems, **HttpOnly cookies** are preferred for refresh/session credentials.

---

# 2️⃣1️⃣ COOKIES 🍪

A cookie is another browser storage mechanism.

The browser can store cookies and send applicable cookies with requests.

Example concept:

```text
Browser
 ↓
Cookie
 ↓
Request
 ↓
Backend
```

---

# 2️⃣2️⃣ HTTPONLY COOKIE

An `HttpOnly` cookie cannot be read directly by normal JavaScript running in the browser.

This can reduce the risk of JavaScript directly stealing the cookie through certain XSS scenarios.

Conceptually:

```text
Backend
 ↓
Set-Cookie
 ↓
Browser
 ↓
HttpOnly Cookie
```

The browser can send it with appropriate requests.

---

# 2️⃣3️⃣ LOCALSTORAGE vs COOKIE

| localStorage | Cookie |
|---|---|
| JavaScript can access it | HttpOnly cookies cannot be read by JS |
| Simple API | Browser manages cookie sending |
| Common for client-side storage | Common for session/refresh credentials |
| XSS can expose stored values | HttpOnly reduces direct JS access |
| Developer manually adds token to requests | Browser can send cookie automatically |

### 🧠 Beginner memory

```text
localStorage
→ JavaScript can read

HttpOnly Cookie
→ JavaScript cannot read
```

Authentication architecture should be chosen carefully based on your backend, threat model, and application requirements.

---

# 2️⃣4️⃣ PROTECTED ROUTES 🛡️

Suppose your application has:

```text
/
 /login
 /register
 /dashboard
 /profile
 /orders
```

You may want:

```text
Everyone
 ↓
Login
Register

Authenticated users
 ↓
Dashboard
Profile
Orders
```

That is where protected routes come in.

---

# 2️⃣5️⃣ PROTECTED ROUTE IDEA

```text
User requests /dashboard
          ↓
Is user authenticated?
       /       \
     YES        NO
      ↓          ↓
 Dashboard      Login
```

---

# 2️⃣6️⃣ SIMPLE PROTECTED ROUTE

For a simple learning example:

```jsx
import { Navigate } from "react-router-dom";

function ProtectedRoute({ children }) {

    const token =
        localStorage.getItem("token");

    if (!token) {
        return <Navigate to="/login" />;
    }

    return children;
}

export default ProtectedRoute;
```

---

# 2️⃣7️⃣ USING PROTECTED ROUTE

```jsx
<Route
    path="/dashboard"
    element={
        <ProtectedRoute>
            <Dashboard />
        </ProtectedRoute>
    }
/>
```

Flow:

```text
/dashboard
    ↓
ProtectedRoute
    ↓
Token exists?
 ┌────┴────┐
YES       NO
 ↓         ↓
Dashboard Login
```

---

# 2️⃣8️⃣ IMPORTANT LIMITATION ⚠️

Checking only:

```javascript
localStorage.getItem("token")
```

does **not prove that the token is valid**.

The token could be:

```text
Expired
Invalid
Revoked
Tampered with
```

So the backend must still protect its APIs.

### Very important:

> **React protected routes improve navigation and user experience. Backend authorization is the real security boundary.**

---

# 2️⃣9️⃣ AUTH CONTEXT

Suppose many components need to know:

```text
Is user logged in?
Who is the user?
What role does the user have?
How do we logout?
```

Passing these values through many components can create prop drilling.

You already learned Context.

So we can create:

```text
AuthContext
```

---

# 3️⃣0️⃣ AUTH CONTEXT ARCHITECTURE

```text
                 AuthProvider
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Header      Dashboard     Profile
        │            │            │
        └────────────┼────────────┘
                     ↓
                Auth State
```

---

# 3️⃣1️⃣ BASIC AUTH CONTEXT

```jsx
import {
    createContext,
    useContext,
    useState
} from "react";

const AuthContext =
    createContext();

export function AuthProvider({ children }) {

    const [user, setUser] =
        useState(null);

    function login(userData) {

        setUser(userData);
    }

    function logout() {

        setUser(null);
    }

    return (
        <AuthContext.Provider
            value={{
                user,
                login,
                logout
            }}
        >
            {children}
        </AuthContext.Provider>
    );
}

export function useAuth() {

    return useContext(AuthContext);
}
```

---

# 3️⃣2️⃣ USING AUTH CONTEXT

A component can do:

```jsx
import { useAuth } from "./AuthContext";

function Header() {

    const {
        user,
        logout
    } = useAuth();

    return (
        <header>

            {user ? (
                <>
                    <span>
                        Welcome {user.name}
                    </span>

                    <button
                        onClick={logout}
                    >
                        Logout
                    </button>
                </>
            ) : (
                <span>
                    Please login
                </span>
            )}

        </header>
    );
}

export default Header;
```

---

# 3️⃣3️⃣ AUTH CONTEXT DRY RUN

Initially:

```javascript
user = null
```

UI:

```text
Please login
```

User logs in.

```text
login(userData)
      ↓
setUser(userData)
      ↓
Context state changes
      ↓
Components re-render
```

Now:

```text
Welcome Ramesh
Logout
```

User clicks Logout:

```text
logout()
 ↓
setUser(null)
 ↓
Components re-render
 ↓
Please login
```

---

# 3️⃣4️⃣ REGISTER + LOGIN + LOGOUT

A real authentication system may have:

```text
REGISTER
   ↓
Create account
   ↓
LOGIN
   ↓
Verify credentials
   ↓
Receive authentication credentials
   ↓
Store/use session information
   ↓
Access protected pages
   ↓
LOGOUT
   ↓
Clear session/authentication state
```

---

# 3️⃣5️⃣ ROLE-BASED AUTHENTICATION 👨‍💼

Now we come to authorization.

Suppose we have:

```text
Admin
User
Manager
```

Different roles can have different permissions.

Example:

```text
Admin
 ├── Dashboard
 ├── Users
 ├── Products
 └── Reports

User
 ├── Dashboard
 └── Profile
```

---

# 3️⃣6️⃣ USER ROLE EXAMPLE

Backend may return user information like:

```javascript
{
    id: 101,
    name: "Ramesh",
    role: "admin"
}
```

Another user:

```javascript
{
    id: 102,
    name: "John",
    role: "user"
}
```

React can use the role to decide what UI/navigation to show.

But remember:

> The backend must enforce permissions too.

---

# 3️⃣7️⃣ SIMPLE ROLE CHECK

```jsx
function AdminPage({ user }) {

    if (user?.role !== "admin") {

        return <h1>Access Denied</h1>;
    }

    return (
        <h1>
            Admin Dashboard
        </h1>
    );
}
```

Flow:

```text
User
 ↓
Check role
 ↓
admin?
 ┌───┴────┐
YES       NO
 ↓         ↓
Page      Denied
```

---

# 3️⃣8️⃣ ROLE-BASED ROUTE IDEA

```jsx
function AdminRoute({ children }) {

    const { user } = useAuth();

    if (!user) {
        return <Navigate to="/login" />;
    }

    if (user.role !== "admin") {
        return <Navigate to="/forbidden" />;
    }

    return children;
}
```

Then:

```jsx
<Route
    path="/admin"
    element={
        <AdminRoute>
            <AdminDashboard />
        </AdminRoute>
    }
/>
```

---

# 3️⃣9️⃣ COMPLETE AUTH ARCHITECTURE 🚀

Think of a real application like this:

```text
                     React App
                         │
                         ↓
                    AuthProvider
                         │
             ┌───────────┼───────────┐
             ↓           ↓           ↓
           Header     Routes       Pages
                         │
                         ↓
                  ProtectedRoute
                         │
                         ↓
                       API
                         │
                         ↓
                      Backend
                         │
             ┌───────────┴───────────┐
             ↓                       ↓
       Authentication          Authorization
             │                       │
             ↓                       ↓
        Token/session          User role
```

---

# 4️⃣0️⃣ LOGIN API EXAMPLE

A React login function could look like:

```jsx
async function loginUser(email, password) {

    const response =
        await fetch(
            "http://localhost:8000/login",
            {
                method: "POST",

                headers: {
                    "Content-Type":
                        "application/json"
                },

                body: JSON.stringify({
                    email,
                    password
                })
            }
        );

    if (!response.ok) {
        throw new Error(
            "Login failed"
        );
    }

    const data =
        await response.json();

    return data;
}
```

Possible response:

```javascript
{
    accessToken: "...",
    user: {
        id: 1,
        name: "Ramesh",
        role: "user"
    }
}
```

---

# 4️⃣1️⃣ LOGIN FLOW WITH REACT

```text
Login Form
     ↓
handleSubmit()
     ↓
loginUser(email, password)
     ↓
POST /login
     ↓
Backend
     ↓
Verify credentials
     ↓
Response
     ↓
Access token + user
     ↓
Auth state
     ↓
Navigate to Dashboard
```

---

# 4️⃣2️⃣ LOGOUT FLOW

A simple client-side logout might do:

```javascript
function logout() {

    localStorage.removeItem("token");

    setUser(null);
}
```

Then:

```text
Logout
 ↓
Clear client auth state
 ↓
Clear stored credential if used
 ↓
User becomes unauthenticated
 ↓
Navigate to Login
```

In cookie/session-based systems, logout may also require a backend request to invalidate the server-side session or refresh credential.

---

# 4️⃣3️⃣ COMMON MISTAKE ❌

## Mistake 1 — Only protecting React routes

Wrong idea:

```text
Hide Dashboard
= Secure Dashboard
```

Not true.

A user could call the backend API directly.

Correct architecture:

```text
React route protection
        +
Backend authentication
        +
Backend authorization
```

---

# 4️⃣4️⃣ COMMON MISTAKE — STORING PASSWORD

Never do:

```javascript
localStorage.setItem(
    "password",
    password
);
```

Do not store plain-text passwords in frontend storage.

The password should be sent securely to the authentication backend and handled according to secure backend authentication practices.

---

# 4️⃣5️⃣ COMMON MISTAKE — PUTTING SECRET KEYS IN REACT

Never put private backend secrets directly into frontend code.

Remember:

```text
React frontend
= public/client-side code
```

Users can inspect the built JavaScript.

Secret keys belong on the server.

---

# 4️⃣6️⃣ COMMON MISTAKE — TRUSTING ROLE FROM FRONTEND

Suppose:

```javascript
localStorage.setItem(
    "role",
    "admin"
);
```

Don't assume this makes the user an admin.

A malicious user can manipulate browser storage.

Correct:

```text
Frontend
→ display appropriate UI

Backend
→ enforce permissions
```

---

# 4️⃣7️⃣ BEST PRACTICES ⭐

### ✅ 1. Use HTTPS

Authentication credentials should be transported over secure HTTPS connections.

### ✅ 2. Validate on backend

Never trust frontend validation alone.

### ✅ 3. Protect backend APIs

Every sensitive API should verify authentication and authorization.

### ✅ 4. Keep access tokens short-lived where appropriate

This limits the lifetime of a compromised access credential.

### ✅ 5. Protect refresh/session credentials

Use a carefully designed secure cookie/session strategy where appropriate.

### ✅ 6. Never store passwords in frontend storage

Never.

### ✅ 7. Separate authentication and authorization

```text
Authentication
= Who?

Authorization
= Allowed to do what?
```

### ✅ 8. Use Auth Context for shared client auth state

This avoids passing authentication information through many unrelated components.

---

# 4️⃣8️⃣ REAL-WORLD PROJECT STRUCTURE

A simple React authentication structure can look like:

```text
src/
│
├── components/
│   ├── ProtectedRoute.jsx
│   └── Header.jsx
│
├── context/
│   └── AuthContext.jsx
│
├── pages/
│   ├── Login.jsx
│   ├── Register.jsx
│   ├── Dashboard.jsx
│   └── Profile.jsx
│
├── services/
│   └── authService.js
│
├── App.jsx
└── main.jsx
```

---

# 4️⃣9️⃣ AUTHENTICATION FLOW — FINAL DRY RUN 🧠

Imagine Ramesh opens:

```text
/dashboard
```

### Step 1

React checks authentication state.

```text
Is user logged in?
```

### Step 2

If not:

```text
/dashboard
 ↓
/login
```

### Step 3

Ramesh enters:

```text
Email
Password
```

### Step 4

React sends:

```text
POST /login
```

### Step 5

Backend checks credentials.

### Step 6

Backend returns authentication information.

### Step 7

React updates Auth Context.

```text
user = {
    name: "Ramesh",
    role: "user"
}
```

### Step 8

React allows the dashboard route.

```text
/login
 ↓
/dashboard
```

### Step 9

Dashboard requests protected data.

```text
GET /profile
Authorization: Bearer ...
```

### Step 10

Backend verifies authentication.

### Step 11

Backend returns data.

```text
Dashboard displayed
```

---

# 5️⃣0️⃣ INTERVIEW QUESTIONS 🎤

## Q1. What is authentication?

**Answer:**  
Authentication verifies the identity of a user.

---

## Q2. What is authorization?

**Answer:**  
Authorization determines what an authenticated user is allowed to access or perform.

---

## Q3. What is JWT?

**Answer:**  
JWT stands for JSON Web Token. It is a token format commonly used to carry claims between a client and server.

---

## Q4. What is an access token?

**Answer:**  
An access token is a credential used to access protected resources.

---

## Q5. What is a refresh token?

**Answer:**  
A refresh token can be used to obtain a new access token after the access token expires, depending on the authentication architecture.

---

## Q6. What is a protected route?

**Answer:**  
A route that only authenticated users should be allowed to access in the frontend application.

---

## Q7. Is a React protected route enough for security?

**Answer:**  
No. Backend APIs must also authenticate requests and enforce authorization.

---

## Q8. What is Auth Context?

**Answer:**  
Auth Context is a React Context used to share authentication-related state and functions across components.

---

## Q9. Authentication vs authorization?

**Answer:**

```text
Authentication
→ Who are you?

Authorization
→ What are you allowed to do?
```

---

## Q10. Why shouldn't passwords be stored in localStorage?

**Answer:**  
Passwords are highly sensitive credentials and should never be stored as plain text in browser storage.

---

## Q11. What is role-based authentication?

**Answer:**  
In the course terminology, role-based authentication means using user roles such as admin and user to control access. More precisely, the permission decision is an authorization concern.

---

## Q12. Can frontend code securely decide that someone is an admin?

**Answer:**  
No. The backend must enforce admin permissions.

---

# 5️⃣1️⃣ PRACTICE TASKS 📝

## Task 1 — Login Page

Create:

```text
Login.jsx
```

Fields:

```text
Email
Password
Login Button
```

---

## Task 2 — Register Page

Create:

```text
Register.jsx
```

Fields:

```text
Name
Email
Password
Confirm Password
```

---

## Task 3 — Auth Context

Create:

```text
AuthContext.jsx
```

Store:

```text
user
login()
logout()
```

---

## Task 4 — Protected Route

Create:

```text
ProtectedRoute.jsx
```

Requirement:

```text
No user
 ↓
Login

User exists
 ↓
Dashboard
```

---

## Task 5 — Role-Based Access

Create:

```text
AdminDashboard
UserDashboard
```

Rules:

```text
admin → AdminDashboard
user  → UserDashboard
```

---

# 5️⃣2️⃣ MINI CHALLENGE 🚀

## 🔐 BUILD A COMPLETE LOGIN SYSTEM

Create:

```text
/pages
    Login.jsx
    Register.jsx
    Dashboard.jsx
    AdminDashboard.jsx

/context
    AuthContext.jsx

/components
    ProtectedRoute.jsx
```

### Requirements

```text
1. Register
2. Login
3. Logout
4. Auth Context
5. Protected Dashboard
6. Admin route
7. User route
8. Authentication state
9. Error message
10. Loading state
```

Architecture:

```text
                 APP
                  │
                  ↓
             AuthProvider
                  │
        ┌─────────┴─────────┐
        ↓                   ↓
      Login              Routes
                            │
                    ┌───────┴────────┐
                    ↓                ↓
               Protected         Protected
                 User               Admin
                    ↓                ↓
               Dashboard       Admin Dashboard
```

Try to build the basic version yourself before looking for a complete solution.

---

# 5️⃣3️⃣ QUIZ 🧠

### Q1. What does authentication answer?

**Answer:**  
"Who are you?"

---

### Q2. What does authorization answer?

**Answer:**  
"What are you allowed to do?"

---

### Q3. What does JWT stand for?

**Answer:**

```text
JSON Web Token
```

---

### Q4. What is an access token used for?

**Answer:**  
Accessing protected resources/APIs.

---

### Q5. What is a refresh token used for?

**Answer:**  
Obtaining a new access token after the current access token expires, depending on the authentication architecture.

---

### Q6. What is localStorage?

**Answer:**  
Browser storage that can be accessed by JavaScript.

---

### Q7. What is an HttpOnly cookie?

**Answer:**  
A cookie that browser JavaScript cannot directly read.

---

### Q8. What is a protected route?

**Answer:**  
A frontend route intended only for authenticated users.

---

### Q9. Is frontend route protection enough?

**Answer:**  
No. Backend APIs must also enforce authentication and authorization.

---

### Q10. What React feature is useful for sharing authentication state?

**Answer:**

```text
Context API
```

---

# 🧠 FINAL MEMORY TRICK

Remember the complete authentication chain:

```text
REGISTER
   ↓
LOGIN
   ↓
VERIFY
   ↓
TOKEN / SESSION
   ↓
AUTH STATE
   ↓
PROTECTED ROUTE
   ↓
API
   ↓
BACKEND VERIFICATION
   ↓
DATA
```

And:

```text
🔐 Authentication
= WHO?

🛡️ Authorization
= WHAT?

🎫 Access Token
= ACCESS

🔄 Refresh Token
= GET NEW ACCESS

🍪 Cookie
= Browser-managed credential storage

🧠 Auth Context
= Share auth state

🚪 Logout
= End/clear authentication state
```

---

# ✅ CHAPTER 27 CHECKLIST

Before moving to the next chapter, make sure you understand:

- [ ] Authentication
- [ ] Authorization
- [ ] Register
- [ ] Login
- [ ] Logout
- [ ] Authentication flow
- [ ] JWT
- [ ] JWT header/payload/signature
- [ ] Access token
- [ ] Refresh token
- [ ] localStorage
- [ ] Cookies
- [ ] HttpOnly cookies
- [ ] Protected routes
- [ ] Auth Context
- [ ] Role-based access
- [ ] Frontend vs backend security
- [ ] Authentication architecture

# 🎉 CHAPTER 27 COMPLETE

You have now learned the **basic architecture behind authentication in React applications**.

The next chapter is a very practical one:

# 🛠️ CHAPTER 28 — CRUD APPLICATION

We will build around:

```text
CREATE
READ
UPDATE
DELETE
SEARCH
FILTER
PAGINATION
FORM VALIDATION
LOADING
ERROR HANDLING
API INTEGRATION
```

These are exactly the CRUD topics specified in your course syllabus.

Type **`NEXT`** to start Chapter 28.