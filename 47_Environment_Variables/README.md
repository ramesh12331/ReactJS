# 🎯 Chapter Goal

By the end of this chapter, you will understand:

- What environment variables are
- Why we use `.env`
- `.env.local`
- Vite environment variables
- `VITE_` prefix
- API URLs
- Development vs Production
- Environment-specific configuration
- What should **never** be stored in frontend environment variables
- React + FastAPI setup
- Common mistakes
- Security rules
- Interview questions
- Practical project

---

# 1. 🧠 What Is an Environment Variable?

An **environment variable** is a configuration value stored outside your main source code.

For example, suppose your React application calls:

```text
http://localhost:8000
```

You could write:

```javascript
fetch(
    "http://localhost:8000/products"
);
```

But after deployment, your backend might be:

```text
https://api.example.com
```

Now you have to change your code.

Instead, we can store the API URL in an environment variable.

```text
Environment Variable
        ↓
API URL
```

Then:

```javascript
fetch(
    `${API_URL}/products`
);
```

---

# 2. 🤔 Why Do We Need Environment Variables?

Imagine:

### Development

```text
React
localhost:5173

FastAPI
localhost:8000
```

### Production

```text
React
https://myapp.com

FastAPI
https://api.myapp.com
```

We don't want to manually change hundreds of files.

Instead:

```text
Development
     ↓
Development API URL

Production
     ↓
Production API URL
```

---

# 3. 🏠 Real-World Analogy

Think about your house.

Your application needs some information:

```text
🏠 API Address
🔧 Configuration
🌍 Environment
```

Instead of writing those values everywhere, keep them in one configuration place.

```text
.env
 ↓
Configuration
 ↓
React application
```

---

# 4. 📁 What Is `.env`?

`.env` is a file used to store environment configuration.

Example:

```text
.env
```

Inside:

```env
VITE_API_URL=http://localhost:8000
```

Now your application can read this value.

---

# 5. ⚛️ Vite Environment Variables

Because you are learning React with **Vite**, there is an important rule.

Frontend variables that should be exposed to browser-side code must use the:

```text
VITE_
```

prefix.

Example:

```env
VITE_API_URL=http://localhost:8000
```

Not:

```env
API_URL=http://localhost:8000
```

for direct client-side access.

---

# 6. 🔑 The `VITE_` Rule

Remember:

```text
VITE_API_URL
     ↑
Required prefix for Vite client-side env variables
```

Example:

```env
VITE_API_URL=http://localhost:8000
VITE_APP_NAME=My React App
VITE_API_VERSION=v1
```

---

# 7. ❌ Without `VITE_`

Suppose:

```env
API_URL=http://localhost:8000
```

Then you try:

```javascript
console.log(
    import.meta.env.API_URL
);
```

You should not expect it to be exposed to browser code by default.

For Vite client-side variables, use:

```env
VITE_API_URL=http://localhost:8000
```

and:

```javascript
import.meta.env.VITE_API_URL
```

---

# 8. 🔍 Accessing Environment Variables

In Vite React:

```javascript
import.meta.env.VITE_API_URL
```

Example:

```jsx
const apiUrl =
    import.meta.env.VITE_API_URL;

console.log(apiUrl);
```

If `.env` contains:

```env
VITE_API_URL=http://localhost:8000
```

the application can use that value.

---

# 9. 🧪 Practical Example

Create:

```text
.env
```

Add:

```env
VITE_API_URL=http://localhost:8000
```

Then:

```jsx
function App() {

    const apiUrl =
        import.meta.env.VITE_API_URL;

    return (
        <div>

            <h1>
                React Application
            </h1>

            <p>
                API: {apiUrl}
            </p>

        </div>
    );
}

export default App;
```

Browser displays:

```text
React Application

API:
http://localhost:8000
```

---

# 10. 🌐 API Example

Instead of:

```javascript
fetch(
    "http://localhost:8000/products"
);
```

Use:

```javascript
const API_URL =
    import.meta.env.VITE_API_URL;


fetch(
    `${API_URL}/products`
);
```

Now your API URL is configurable.

---

# 11. 🔄 Dry Run

`.env`:

```env
VITE_API_URL=http://localhost:8000
```

React:

```javascript
const API_URL =
    import.meta.env.VITE_API_URL;
```

Result:

```text
API_URL
   ↓
http://localhost:8000
```

Then:

```javascript
fetch(
    `${API_URL}/products`
);
```

becomes:

```text
http://localhost:8000/products
```

---

# 12. 📁 Where Should `.env` Be?

Usually put `.env` in the project root:

```text
react-project/
│
├── .env
├── package.json
├── vite.config.js
├── index.html
│
└── src/
    ├── App.jsx
    └── main.jsx
```

Not:

```text
src/.env
```

for the normal project-level Vite setup.

---

# 13. 🗂️ Common Environment Files

You may encounter:

```text
.env
.env.local
.env.development
.env.production
```

Let's understand them.

---

# 14. `.env`

General environment configuration.

Example:

```env
VITE_APP_NAME=My React App
```

---

# 15. `.env.local`

Local-only configuration.

Example:

```env
VITE_API_URL=http://localhost:8000
```

This is useful for machine-specific settings.

A common convention is to keep local environment files out of Git when they contain local/private configuration.

---

# 16. `.env.development`

Development-specific configuration.

Example:

```env
VITE_API_URL=http://localhost:8000
```

This is useful when running your application locally.

---

# 17. `.env.production`

Production-specific configuration.

Example:

```env
VITE_API_URL=https://api.example.com
```

Now you can have:

```text
Development
     ↓
http://localhost:8000

Production
     ↓
https://api.example.com
```

---

# 18. 🧠 Environment Flow

Think:

```text
             React App
                 │
        ┌────────┴────────┐
        ↓                 ↓
 Development          Production
        ↓                 ↓
 .env.development   .env.production
        ↓                 ↓
 localhost API       production API
```

---

# 19. 🚀 Development vs Production

### Development

You might run:

```bash
npm run dev
```

and use:

```text
http://localhost:8000
```

### Production

You might build:

```bash
npm run build
```

and use:

```text
https://api.example.com
```

Environment variables allow configuration to change based on the environment.

---

# 20. 🧠 Important Security Concept

This is VERY important.

A frontend environment variable is **not a secret** merely because it is inside `.env`.

Why?

Because Vite embeds exposed client-side environment values into the application during the build process.

The browser ultimately receives the client-side application code.

Therefore:

```text
VITE_API_URL
```

is okay for configuration.

But:

```text
VITE_DATABASE_PASSWORD
```

is NOT safe.

---

# 21. 🚨 NEVER Put Secrets in Frontend Variables

Never put these in a client-exposed variable:

```text
❌ Database password
❌ PostgreSQL password
❌ MongoDB password
❌ Private API key
❌ JWT signing secret
❌ Encryption private key
❌ Admin password
```

For example:

```env
VITE_DB_PASSWORD=123456
```

❌ Very bad idea.

---

# 22. 🔐 Why Is It Dangerous?

Imagine:

```text
.env
   ↓
Vite build
   ↓
React JavaScript
   ↓
Browser
```

If the value is exposed to browser code:

```text
User
 ↓
Browser DevTools
 ↓
JavaScript
 ↓
Can potentially inspect it
```

Therefore:

> **Never treat frontend environment variables as secret storage.**

---

# 23. 🛡️ Where Should Secrets Go?

Secrets should stay on the backend/server.

For example:

```text
React
  ↓
FastAPI
  ↓
Database
```

React knows:

```text
API URL
```

FastAPI knows:

```text
Database username
Database password
Private API keys
JWT secret
```

Architecture:

```text
┌──────────────┐
│    React     │
│   Browser    │
└──────┬───────┘
       │
       │ API
       ↓
┌──────────────┐
│   FastAPI    │
│   Backend    │
└──────┬───────┘
       │
       ↓
┌──────────────┐
│  PostgreSQL  │
└──────────────┘
```

---

# 24. 🐍 React + FastAPI Example

### React `.env`

```env
VITE_API_URL=http://localhost:8000
```

React:

```javascript
const API_URL =
    import.meta.env.VITE_API_URL;
```

Request:

```javascript
fetch(
    `${API_URL}/products`
);
```

---

# 25. 🐍 FastAPI `.env`

The backend can have its own environment configuration.

For example:

```env
DATABASE_URL=postgresql://...
JWT_SECRET=some-secret
```

Notice:

```text
React .env
       ↓
Public/client configuration

FastAPI .env
       ↓
Server configuration + secrets
```

These are different environments with different responsibilities.

---

# 26. 🧠 Very Important Difference

### Frontend

```env
VITE_API_URL=https://api.example.com
```

Potentially visible to users.

### Backend

```env
DATABASE_PASSWORD=secret
JWT_SECRET=secret
```

Must remain server-side.

---

# 27. 📦 Environment Variables in API Service

Instead of repeating:

```javascript
fetch(
    `${import.meta.env.VITE_API_URL}/products`
);

fetch(
    `${import.meta.env.VITE_API_URL}/users`
);

fetch(
    `${import.meta.env.VITE_API_URL}/orders`
);
```

Create:

```javascript
const API_URL =
    import.meta.env.VITE_API_URL;
```

Then:

```javascript
export async function getProducts() {

    const response =
        await fetch(
            `${API_URL}/products`
        );

    return response.json();
}
```

---

# 28. 📁 Better Project Structure

A practical structure:

```text
src/
│
├── components/
│
├── pages/
│
├── services/
│   └── api.js
│
├── hooks/
│
├── utils/
│
└── App.jsx
```

`api.js`:

```javascript
const API_URL =
    import.meta.env.VITE_API_URL;


export async function getProducts() {

    const response =
        await fetch(
            `${API_URL}/products`
        );

    if (!response.ok) {
        throw new Error(
            "Failed to fetch products"
        );
    }

    return response.json();
}
```

Now components don't need to know the API URL directly.

---

# 29. 🧠 Why Centralize API Configuration?

Without centralization:

```text
Component 1 → URL
Component 2 → URL
Component 3 → URL
Component 4 → URL
```

With centralization:

```text
              API_URL
                 ↓
            api.js
          ↙    ↓    ↘
       Page1  Page2  Page3
```

This is easier to maintain.

---

# 30. ⚠️ Common Mistake — Forgetting `VITE_`

Wrong:

```env
API_URL=http://localhost:8000
```

Then:

```javascript
import.meta.env.API_URL
```

Not exposed as a normal client-side Vite variable.

Correct:

```env
VITE_API_URL=http://localhost:8000
```

Then:

```javascript
import.meta.env.VITE_API_URL
```

---

# 31. ⚠️ Common Mistake — Not Restarting the Dev Server

You change:

```env
VITE_API_URL=http://localhost:8000
```

but the application still behaves as before.

Restart the Vite development server when needed:

```text
Stop server
   ↓
npm run dev
```

Environment values are processed by the build/dev tooling, so restarting is a good troubleshooting step after changing `.env` values.

---

# 32. ⚠️ Common Mistake — Spaces

Be careful with environment syntax.

Prefer:

```env
VITE_API_URL=http://localhost:8000
```

Avoid unnecessary formatting such as:

```env
VITE_API_URL = http://localhost:8000
```

Keep your environment files simple and consistent.

---

# 33. ⚠️ Common Mistake — Quotes

Usually:

```env
VITE_APP_NAME=MyReactApp
```

is enough.

If you need spaces, follow the environment parser's supported quoting rules, for example:

```env
VITE_APP_NAME="My React App"
```

---

# 34. ⚠️ Common Mistake — Wrong File Location

Wrong:

```text
src/
└── .env
```

Normal project-level configuration:

```text
react-project/
└── .env
```

---

# 35. ⚠️ Common Mistake — Committing Secrets

Never accidentally commit sensitive `.env` files.

For local secrets, your `.gitignore` can include:

```gitignore
.env.local
.env.*.local
```

Depending on your project's environment-file strategy, you may also choose to ignore other local `.env` files that contain secrets.

---

# 36. 📄 `.env.example`

A useful practice is to provide a template.

Create:

```text
.env.example
```

Example:

```env
VITE_API_URL=
```

This tells another developer which variables are required without giving them your private values.

---

# 37. 🧠 `.env` vs `.env.example`

### `.env`

Actual local values:

```env
VITE_API_URL=http://localhost:8000
```

### `.env.example`

Template:

```env
VITE_API_URL=
```

Think:

```text
.env
 ↓
Actual configuration

.env.example
 ↓
Configuration template
```

---

# 38. 🌍 Production Deployment

Suppose your development API is:

```text
http://localhost:8000
```

Production API:

```text
https://api.myapp.com
```

Your production environment should use:

```env
VITE_API_URL=https://api.myapp.com
```

Then build:

```bash
npm run build
```

The resulting application uses the production configuration that was supplied during the build.

---

# 39. 🏗️ Important Build Concept

Remember:

```text
.env
 ↓
Vite
 ↓
Build
 ↓
JavaScript/CSS assets
 ↓
Browser
```

Therefore:

> Frontend environment variables are configuration values, not a secure vault.

---

# 40. 🔐 API URL vs API Secret

This distinction is extremely important in interviews.

### API URL

```env
VITE_API_URL=https://api.example.com
```

Usually okay to expose.

### API Secret

```text
PRIVATE_API_KEY
```

Should stay on the backend when it is actually secret.

Architecture:

```text
React
 ↓
Your Backend
 ↓
Private External API
```

The browser doesn't need the private credential.

---

# 41. 🧪 Practical Project

Let's create:

```text
React Product App
```

Environment:

```env
VITE_API_URL=http://localhost:8000
```

API service:

```javascript
const API_URL =
    import.meta.env.VITE_API_URL;


export async function getProducts() {

    const response =
        await fetch(
            `${API_URL}/products`
        );

    if (!response.ok) {

        throw new Error(
            `HTTP Error: ${response.status}`
        );
    }

    return response.json();
}
```

Component:

```jsx
import {
    useEffect,
    useState
} from "react";

import {
    getProducts
} from "./services/api";


function Products() {

    const [products, setProducts] =
        useState([]);

    const [loading, setLoading] =
        useState(true);

    const [error, setError] =
        useState(null);


    useEffect(() => {

        async function loadProducts() {

            try {

                setLoading(true);

                const data =
                    await getProducts();

                setProducts(data);

            } catch (error) {

                setError(
                    error.message
                );

            } finally {

                setLoading(false);
            }
        }


        loadProducts();

    }, []);


    if (loading) {
        return <p>Loading...</p>;
    }


    if (error) {
        return (
            <p>
                Error: {error}
            </p>
        );
    }


    return (
        <div>

            <h1>
                Products
            </h1>

            {products.map(
                product => (
                    <p key={product.id}>
                        {product.name}
                    </p>
                )
            )}

        </div>
    );
}


export default Products;
```

---

# 42. 🔍 Practical Dry Run

`.env`:

```env
VITE_API_URL=http://localhost:8000
```

Then:

```javascript
import.meta.env.VITE_API_URL
```

returns:

```text
http://localhost:8000
```

Then:

```javascript
`${API_URL}/products`
```

becomes:

```text
http://localhost:8000/products
```

Request:

```text
React
 ↓
http://localhost:8000/products
 ↓
FastAPI
 ↓
Products
 ↓
React
```

---

# 43. 🧠 Environment Variable Flow

Remember this:

```text
.env
 ↓
Vite
 ↓
import.meta.env
 ↓
React
 ↓
API request
```

For example:

```text
VITE_API_URL
     ↓
import.meta.env.VITE_API_URL
     ↓
API_URL
     ↓
fetch()
```

---

# 44. 💼 INTERVIEW QUESTIONS

### Q1. What is an environment variable?

A configuration value provided outside the main application source code.

---

### Q2. Why are environment variables useful?

They allow configuration to change between environments without hardcoding those values throughout the application.

---

### Q3. How do you access a Vite environment variable in React?

```javascript
import.meta.env.VITE_VARIABLE_NAME
```

---

### Q4. Why does Vite use the `VITE_` prefix?

Vite exposes variables with that prefix to client-side application code.

---

### Q5. Are frontend environment variables secret?

**No.**

Values exposed to frontend code can potentially be inspected by users.

---

### Q6. Can you store a database password in `VITE_DB_PASSWORD`?

**No.**

A database password must remain on the server.

---

### Q7. What is `.env.example`?

A template showing which environment variables a project needs without containing the real secret values.

---

### Q8. What is the difference between development and production environment variables?

They allow the application to use different configuration values for development and production environments.

---

### Q9. Where should private API credentials normally live?

On the backend/server side or in a secure server-side secret management system.

---

### Q10. What is a common Vite environment-variable mistake?

Forgetting the `VITE_` prefix for a variable that needs to be accessed from client-side code.

---

# 45. 📝 PRACTICE

## Practice 1

Create:

```text
.env
```

with:

```env
VITE_APP_NAME=My React App
```

Display it in React.

---

## Practice 2

Create:

```env
VITE_API_URL=http://localhost:8000
```

Display the URL.

---

## Practice 3

Create:

```text
services/api.js
```

and centralize your API URL there.

---

## Practice 4

Create:

```text
.env.example
```

with:

```env
VITE_API_URL=
```

---

## Practice 5

Create two configurations:

```text
Development
Production
```

and understand which API URL each environment should use.

---

# 46. 🧩 MINI CHALLENGE

Build:

# 🛒 React + FastAPI Product App

### Frontend

```text
React + Vite
```

### Backend

```text
FastAPI
```

### Environment

```env
VITE_API_URL=http://localhost:8000
```

### Requirements

```text
Products
   ↓
Fetch from API
   ↓
Loading
   ↓
Success
   ↓
Error
```

Do **not** hardcode:

```text
http://localhost:8000
```

inside your components.

Instead:

```text
.env
 ↓
API service
 ↓
Components
```

---

# 47. 🚨 SECURITY CHECKLIST

Before committing your project:

```text
☐ No database passwords
☐ No JWT secrets
☐ No private API keys
☐ No admin passwords
☐ No private tokens
☐ Local secret files ignored
☐ .env.example provided
☐ Public configuration is okay to expose
```

Remember:

```text
Frontend
   ↓
PUBLIC

Backend
   ↓
PRIVATE
```

---

# 48. 📝 10-QUESTION QUIZ

### Q1. What is an environment variable?

**Answer:** A configuration value stored outside the application's normal source code.

---

### Q2. What prefix does Vite use for client-exposed environment variables?

**Answer:**

```text
VITE_
```

---

### Q3. How do you access a Vite environment variable?

**Answer:**

```javascript
import.meta.env.VITE_API_URL
```

---

### Q4. Where should `.env` normally be located in a Vite project?

**Answer:** At the project root.

---

### Q5. Is `VITE_API_URL` a secret?

**Answer:** No. Client-exposed environment variables should be considered visible to users.

---

### Q6. Should you put a PostgreSQL password inside `VITE_DB_PASSWORD`?

**Answer:** No.

---

### Q7. What is `.env.example` used for?

**Answer:** It documents the required environment variables without exposing actual secret values.

---

### Q8. What should you use for a development API URL?

**Answer:** For example:

```env
VITE_API_URL=http://localhost:8000
```

---

### Q9. Where should private database credentials normally be stored?

**Answer:** On the backend/server side, using server-side environment variables or secure secret management.

---

### Q10. Why use environment variables for API URLs?

**Answer:** So the application can use different API endpoints for development, staging, and production without hardcoding them throughout the application.

---

# 🧠 FINAL MEMORY TRICK

Remember these **5 rules**:

```text
1️⃣ .env
   ↓
   Configuration

2️⃣ VITE_
   ↓
   Client-side Vite variable

3️⃣ import.meta.env
   ↓
   Access variable

4️⃣ VITE_ ≠ Secret
   ↓
   Browser can see it

5️⃣ Secrets
   ↓
   Keep them on backend
```

### One-line memory:

> 🔐 **Environment variables configure your app; they do not magically make frontend values secret.**

---

# ✅ CHAPTER 47 COMPLETE

You learned:

- `.env`
- `.env.local`
- `.env.development`
- `.env.production`
- `VITE_`
- `import.meta.env`
- API URL configuration
- Development vs Production
- `.env.example`
- Frontend security
- Backend secrets
- React + FastAPI configuration
- API service structure
- Common mistakes
- Security checklist
- Interview questions
- Practice
- Mini challenge

---

# 🚀 NEXT CHAPTER — CHAPTER 48

## 🏗️ PRODUCTION BUILD

Next we will learn:

- What is a production build?
- Development vs production
- `npm run build`
- `dist` folder
- Build optimization
- Minification
- Source maps
- Previewing production build
- Build errors
- Environment variables during build
- Production checklist
- React deployment preparation
- Interview questions
- Practical project

Type **NEXT** to continue.