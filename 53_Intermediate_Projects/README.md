# 🚀 CHAPTER 53 — INTERMEDIATE REACT PROJECTS

## 🟡 PART 1 — WEATHER APP

We are now moving from **beginner projects** to **intermediate projects**.

Beginner projects taught us individual React concepts.

Intermediate projects teach us how to **combine those concepts into real applications**.

---

# 🎯 INTERMEDIATE PROJECT ROADMAP

According to your course:

```text
Chapter 53 — Intermediate Projects

1. 🌤️ Weather App
2. 🎬 Movie Search App
3. 💰 Expense Tracker
4. 🧠 Quiz App
5. 🍳 Recipe App
6. 🛍️ Product Listing App
```

These projects are expected to include:

```text
🌐 API
🔍 Search
🔎 Filter
↕️ Sorting
📄 Pagination
⏳ Loading
❌ Error handling
```



We will build them progressively.

---

# 🌤️ PROJECT 1 — WEATHER APP

## 🎯 What Are We Building?

We want a React application where the user enters a city:

```text
┌──────────────────────────────────┐
│         🌤️ Weather App           │
│                                  │
│  [ Hyderabad             ] [🔍]  │
│                                  │
│  Hyderabad                       │
│  🌡️ 28°C                         │
│  ☁️ Cloudy                       │
│  💧 Humidity: 72%                │
│  💨 Wind: 12 km/h                │
│                                  │
└──────────────────────────────────┘
```

The user enters:

```text
Hyderabad
```

Then:

```text
React
   ↓
API request
   ↓
Weather API
   ↓
JSON response
   ↓
React state
   ↓
Weather UI
```

---

# 🧠 WHY IS THIS AN INTERMEDIATE PROJECT?

The Counter App had:

```text
State
+
Button
```

The Todo App had:

```text
State
+
Arrays
+
Events
```

The Weather App introduces:

```text
State
+
Forms
+
API
+
async/await
+
useEffect
+
Loading
+
Error handling
+
Conditional rendering
+
Environment variables
```

So this is much closer to a real-world application.

---

# 1. 📖 WHAT IS AN API?

API means:

> **Application Programming Interface**

In simple words:

> An API allows one application to communicate with another application.

Think about a restaurant.

```text
You
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

In an application:

```text
React
 ↓
API Request
 ↓
Server
 ↓
Database / External Service
 ↓
API Response
 ↓
React
```

---

# 2. 🌐 WEATHER API

Our React application does not know the weather itself.

It asks a weather service:

```text
"What is the weather in Hyderabad?"
```

The server responds with data.

Example JSON:

```json
{
    "city": "Hyderabad",
    "temperature": 28,
    "humidity": 72,
    "condition": "Cloudy",
    "windSpeed": 12
}
```

React receives this data and displays it.

---

# 3. 🧠 FIRST THINK ABOUT THE DATA

Before writing code, ask:

> What data do we need?

We need:

```text
city
temperature
humidity
condition
wind speed
```

We also need application state.

---

# 4. 🔄 WHAT SHOULD BE STATE?

Think carefully.

### Search input

```text
city
```

### Weather result

```text
weather
```

### API loading status

```text
loading
```

### API error

```text
error
```

Therefore:

```jsx
const [city, setCity] =
    useState("");

const [weather, setWeather] =
    useState(null);

const [loading, setLoading] =
    useState(false);

const [error, setError] =
    useState("");
```

---

# 5. 🧠 WHY FOUR STATES?

Because these represent different things.

```text
city
 ↓
What user typed

weather
 ↓
What API returned

loading
 ↓
Is API request running?

error
 ↓
Did something go wrong?
```

This is a very common React pattern.

---

# 6. 🌐 API REQUEST FLOW

Suppose user enters:

```text
Hyderabad
```

Then:

```text
User
 ↓
Search button
 ↓
handleSearch()
 ↓
setLoading(true)
 ↓
API request
 ↓
Response
 ↓
setWeather(data)
 ↓
setLoading(false)
 ↓
Display weather
```

If something fails:

```text
API request
 ↓
ERROR
 ↓
setError(...)
 ↓
setLoading(false)
 ↓
Display error
```

---

# 7. 🧠 IMPORTANT API STATE PATTERN

Remember this:

```text
        API REQUEST
             │
       ┌─────┴─────┐
       ↓           ↓
    SUCCESS      ERROR
       │           │
       ↓           ↓
   setWeather   setError
       │           │
       └─────┬─────┘
             ↓
       setLoading(false)
```

---

# 8. 📁 PROJECT STRUCTURE

Let's create a realistic structure.

```text
weather-app/
│
├── src/
│   │
│   ├── components/
│   │   ├── SearchBar.jsx
│   │   ├── WeatherCard.jsx
│   │   ├── Loading.jsx
│   │   └── ErrorMessage.jsx
│   │
│   ├── services/
│   │   └── weatherApi.js
│   │
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
│
├── .env
├── package.json
└── vite.config.js
```

---

# 9. 🧠 WHY CREATE `services/`?

Instead of putting API code directly inside our UI component:

```text
App.jsx
   ↓
fetch()
```

we can separate responsibilities:

```text
App.jsx
   ↓
weatherApi.js
   ↓
API
```

This makes the application easier to maintain.

---

# 10. 🔐 ENVIRONMENT VARIABLE

If an API requires a key, don't write it directly into your component.

For a Vite frontend, a variable intended for browser-side code typically uses the `VITE_` prefix.

Example:

```env
VITE_WEATHER_API_KEY=your_api_key
```

Then:

```javascript
const API_KEY =
    import.meta.env.VITE_WEATHER_API_KEY;
```

### ⚠️ Important

A `VITE_` variable is exposed to frontend code.

Therefore:

> **Never treat a frontend environment variable as a secret.**

Private API credentials should normally stay on a backend.

---

# 11. 🧩 WEATHER API SERVICE

Create:

```text
src/services/weatherApi.js
```

Example structure:

```javascript
const API_KEY =
    import.meta.env.VITE_WEATHER_API_KEY;


export async function getWeather(city) {

    const response =
        await fetch(
            `https://example.com/weather?q=${encodeURIComponent(city)}&key=${API_KEY}`
        );


    if (!response.ok) {

        throw new Error(
            "Unable to fetch weather"
        );

    }


    return response.json();

}
```

---

# 12. 🧠 WHY `encodeURIComponent()`?

Suppose the user enters:

```text
New York
```

A URL contains special characters.

We can safely encode the city:

```javascript
encodeURIComponent(city)
```

So:

```text
New York
```

becomes something like:

```text
New%20York
```

This helps construct a valid query string.

---

# 13. 🧠 WHY CHECK `response.ok`?

This:

```javascript
if (!response.ok)
```

checks whether the HTTP response indicates success.

If the API returns an unsuccessful status:

```text
400
404
500
```

we can throw an error.

---

# 14. 🏠 APP COMPONENT

Now:

```jsx
import { useState } from "react";

import { getWeather } from "./services/weatherApi";


function App() {

    const [city, setCity] =
        useState("");


    const [weather, setWeather] =
        useState(null);


    const [loading, setLoading] =
        useState(false);


    const [error, setError] =
        useState("");


    async function handleSearch() {

        if (city.trim() === "") {
            return;
        }


        try {

            setLoading(true);

            setError("");

            setWeather(null);


            const data =
                await getWeather(city);


            setWeather(data);

        } catch (error) {

            setError(
                error.message
            );

        } finally {

            setLoading(false);

        }

    }


    return (
        <div>

            <h1>
                🌤️ Weather App
            </h1>


            <input
                type="text"
                value={city}
                onChange={event =>
                    setCity(
                        event.target.value
                    )
                }
                placeholder="Enter city"
            />


            <button
                onClick={handleSearch}
            >
                Search
            </button>


            {loading && (
                <p>
                    ⏳ Loading...
                </p>
            )}


            {error && (
                <p>
                    ❌ {error}
                </p>
            )}


            {weather && (
                <div>

                    <h2>
                        {weather.city}
                    </h2>

                    <p>
                        🌡️
                        {weather.temperature}
                        °C
                    </p>

                    <p>
                        ☁️
                        {weather.condition}
                    </p>

                    <p>
                        💧
                        {weather.humidity}%
                    </p>

                    <p>
                        💨
                        {weather.windSpeed}
                        km/h
                    </p>

                </div>
            )}

        </div>
    );
}


export default App;
```

---

# 15. 🔍 UNDERSTAND `handleSearch()`

This is the most important function.

```javascript
async function handleSearch() {
```

We use `async` because we are going to wait for an API response.

---

## Step 1 — Validate Input

```javascript
if (city.trim() === "") {
    return;
}
```

If user enters:

```text
"     "
```

we don't send the request.

---

# 16. ⏳ Start Loading

```javascript
setLoading(true);
```

Now:

```text
loading = true
```

The UI displays:

```text
⏳ Loading...
```

---

# 17. 🧹 Clear Previous Error

```javascript
setError("");
```

Suppose the previous search produced:

```text
❌ City not found
```

When starting a new request, we don't want the old error to remain.

---

# 18. 🌐 Call API

```javascript
const data =
    await getWeather(city);
```

Suppose:

```text
city = Hyderabad
```

Then:

```text
getWeather("Hyderabad")
```

sends the API request.

---

# 19. 📦 Store API Response

```javascript
setWeather(data);
```

Suppose API returns:

```json
{
    "city": "Hyderabad",
    "temperature": 28,
    "condition": "Cloudy",
    "humidity": 72,
    "windSpeed": 12
}
```

Then:

```text
weather = response data
```

React re-renders.

---

# 20. ❌ ERROR HANDLING

If the API fails:

```javascript
catch (error) {

    setError(
        error.message
    );

}
```

Example:

```text
API
 ↓
404
 ↓
Error
 ↓
catch
 ↓
setError()
 ↓
UI
```

UI:

```text
❌ Unable to fetch weather
```

---

# 21. 🧹 FINALLY

This:

```javascript
finally {

    setLoading(false);

}
```

runs after the request finishes whether it succeeded or failed.

Therefore:

```text
Success
  ↓
loading = false

OR

Error
  ↓
loading = false
```

This prevents the loading indicator from staying forever.

---

# 22. 🧠 API REQUEST STATE MACHINE

A useful mental model:

```text
                  START
                    ↓
                 IDLE
                    │
               Search
                    ↓
                LOADING
                /      \
               /        \
              ↓          ↓
          SUCCESS       ERROR
              │            │
              ↓            ↓
         Show data     Show error
              \            /
               \          /
                ↓        ↓
                   IDLE
```

This pattern appears in many React applications.

---

# 23. 🔍 SEARCH WITH ENTER KEY

A professional search box should allow:

```text
Type city
   ↓
Press Enter
   ↓
Search
```

Instead of only:

```text
Click Search
```

Use:

```jsx
<input
    value={city}
    onChange={event =>
        setCity(event.target.value)
    }
    onKeyDown={event => {

        if (event.key === "Enter") {
            handleSearch();
        }

    }}
/>
```

Now:

```text
Enter
 ↓
handleSearch()
```

---

# 24. 🧩 COMPONENT ARCHITECTURE

Our `App.jsx` can eventually become:

```text
App
│
├── SearchBar
│
├── Loading
│
├── ErrorMessage
│
└── WeatherCard
```

Visual:

```text
App
 │
 ├───────────────┐
 ↓               ↓
SearchBar      WeatherCard
 │
 └── search
```

---

# 25. 🔹 SearchBar COMPONENT

Create:

```text
src/components/SearchBar.jsx
```

```jsx
function SearchBar({
    city,
    setCity,
    onSearch
}) {

    function handleSubmit(event) {

        event.preventDefault();

        onSearch();

    }


    return (
        <form
            onSubmit={handleSubmit}
        >

            <input
                value={city}
                onChange={event =>
                    setCity(
                        event.target.value
                    )
                }
                placeholder="Enter city"
            />

            <button type="submit">
                🔍 Search
            </button>

        </form>
    );
}


export default SearchBar;
```

---

# 26. 🔹 WeatherCard COMPONENT

```jsx
function WeatherCard({
    weather
}) {

    return (
        <div>

            <h2>
                {weather.city}
            </h2>

            <p>
                🌡️
                {weather.temperature}°C
            </p>

            <p>
                ☁️
                {weather.condition}
            </p>

            <p>
                💧
                {weather.humidity}%
            </p>

            <p>
                💨
                {weather.windSpeed}
                km/h
            </p>

        </div>
    );
}


export default WeatherCard;
```

---

# 27. 🧠 Why Components?

Imagine the application becomes bigger.

You may eventually have:

```text
Weather App
│
├── Header
├── SearchBar
├── WeatherCard
├── Forecast
├── HourlyForecast
├── WeatherDetails
├── Loading
└── ErrorMessage
```

If everything is inside `App.jsx`, it becomes difficult to maintain.

Components divide the application into smaller responsibilities.

---

# 28. 🧠 PROPS FLOW

Parent:

```text
App
```

passes data:

```text
weather
```

to:

```text
WeatherCard
```

Flow:

```text
App
 │
 │ weather
 ↓
WeatherCard
```

For search:

```text
App
 │
 │ onSearch
 ↓
SearchBar
 │
 │ user clicks
 ↓
onSearch()
 │
 ↓
App
```

This combines:

```text
Props
+
Callback functions
+
Component communication
```

---

# 29. 🔄 COMPLETE WEATHER ARCHITECTURE

```text
                    App
                     │
          ┌──────────┼──────────┐
          ↓          ↓          ↓
      SearchBar   Loading    ErrorMessage
          │
          │ onSearch
          ↓
     handleSearch()
          │
          ↓
     weatherApi.js
          │
          ↓
       Weather API
          │
          ↓
       JSON data
          │
          ↓
     setWeather()
          │
          ↓
     WeatherCard
```

---

# 30. ⏳ LOADING STATE

When API is running:

```text
⏳ Loading weather...
```

Don't display:

```text
Weather
+
Loading
+
Old Error
```

at the same time.

A clean UI uses conditional rendering.

Example:

```jsx
{loading && (
    <p>⏳ Loading weather...</p>
)}
```

---

# 31. ❌ ERROR STATE

```jsx
{error && (
    <p>
        ❌ {error}
    </p>
)}
```

If:

```text
error = ""
```

nothing appears.

If:

```text
error =
"City not found"
```

then:

```text
❌ City not found
```

appears.

---

# 32. 📭 EMPTY STATE

What should the application show before the first search?

For example:

```text
🌤️ Search for a city to see its weather.
```

This is called an **empty state**.

```jsx
{!weather &&
 !loading &&
 !error && (
    <p>
        🌤️ Search for a city.
    </p>
)}
```

---

# 33. 🧠 THREE IMPORTANT UI STATES

Almost every API application needs:

```text
⏳ Loading
```

```text
✅ Success
```

```text
❌ Error
```

And often:

```text
📭 Empty
```

Remember:

```text
API UI =
Loading
+
Success
+
Error
+
Empty
```

---

# 34. 🔍 SEARCH VALIDATION

User clicks Search without entering a city.

We should stop:

```javascript
if (city.trim() === "") {
    return;
}
```

Better UX:

```javascript
if (city.trim() === "") {

    setError(
        "Please enter a city name."
    );

    return;
}
```

Now the user knows what went wrong.

---

# 35. 🧠 REAL-WORLD API FLOW

Imagine:

```text
User:
Hyderabad
```

Then:

```text
React
 ↓
GET /weather?city=Hyderabad
 ↓
Weather Server
 ↓
Weather Data
 ↓
JSON
```

Response:

```json
{
    "city": "Hyderabad",
    "temperature": 28,
    "condition": "Cloudy"
}
```

React:

```text
setWeather(data)
```

UI:

```text
Hyderabad
28°C
Cloudy
```

---

# 36. 🚨 COMMON MISTAKES

## Mistake 1 — Forgetting `await`

Bad:

```javascript
const data =
    getWeather(city);
```

This gives a Promise.

Correct:

```javascript
const data =
    await getWeather(city);
```

inside an `async` function.

---

## Mistake 2 — Forgetting `response.json()`

Bad:

```javascript
return response;
```

Usually you need the parsed response body:

```javascript
return response.json();
```

---

## Mistake 3 — No error handling

Bad:

```javascript
const data =
    await getWeather(city);

setWeather(data);
```

Better:

```javascript
try {

    const data =
        await getWeather(city);

    setWeather(data);

} catch (error) {

    setError(
        error.message
    );

}
```

---

## Mistake 4 — Loading never stops

Bad:

```javascript
setLoading(true);

// API

setWeather(data);
```

If the request fails, loading might remain true.

Better:

```javascript
try {
    ...
} catch {
    ...
} finally {
    setLoading(false);
}
```

---

# 37. 🧪 DRY RUN

User enters:

```text
Hyderabad
```

### State

```text
city = "Hyderabad"
weather = null
loading = false
error = ""
```

Click Search.

### Step 1

```text
loading = true
```

UI:

```text
⏳ Loading...
```

### Step 2

API request:

```text
getWeather("Hyderabad")
```

### Step 3

API responds:

```text
temperature = 28
humidity = 72
```

### Step 4

```text
setWeather(data)
```

### Step 5

```text
loading = false
```

### Step 6

UI:

```text
Hyderabad

🌡️ 28°C
☁️ Cloudy
💧 72%
💨 12 km/h
```

---

# 38. 🧪 ERROR DRY RUN

User enters:

```text
ABCDEFG
```

API:

```text
404
```

Then:

```text
catch
 ↓
setError("Unable to fetch weather")
 ↓
setLoading(false)
```

UI:

```text
❌ Unable to fetch weather
```

---

# 39. 🎯 INTERMEDIATE SKILLS WE JUST USED

This one project combines:

```text
React
│
├── useState
├── Events
├── Forms
├── Props
├── Callbacks
├── Conditional Rendering
├── async/await
├── fetch()
├── API
├── Environment Variables
├── Loading State
├── Error State
├── Empty State
└── Component Architecture
```

This is why project-based learning is powerful.

---

# 🧪 MINI CHALLENGE

Don't copy the solution immediately.

Build a Weather Search UI with:

```text
1. City input
2. Search button
3. Loading message
4. Error message
5. Weather result
6. Enter-key search
```

### Think first

#### What is the state?

```text
?
```

#### What is the event?

```text
?
```

#### What API function do we need?

```text
?
```

#### What happens while waiting?

```text
?
```

#### What happens on success?

```text
?
```

#### What happens on error?

```text
?
```

---

# 💼 INTERVIEW QUESTIONS

### Q1. How do you handle API loading state in React?

**Answer:**

I maintain a loading state, set it to `true` before the request, and set it back to `false` after the request using `finally`.

---

### Q2. Why do we use `async/await`?

**Answer:**

To work with asynchronous operations such as API requests in a readable way.

---

### Q3. Why use `try/catch` around an API call?

**Answer:**

To handle failures and show an appropriate error state to the user.

---

### Q4. What is an empty state?

**Answer:**

The UI shown when there is currently no data to display, such as before the first search.

---

### Q5. Why separate API code into a service file?

**Answer:**

It separates data-fetching logic from UI logic, making the application easier to maintain and test.

---

### Q6. Why should API keys not be treated as secrets in a React frontend?

**Answer:**

Frontend environment variables are ultimately exposed to browser-side code. Sensitive secrets should be kept on a backend.

---

# 📝 10-QUESTION QUIZ

### Q1. What does API stand for?

**Answer:** Application Programming Interface.

### Q2. Which Hook commonly stores API response data?

**Answer:** `useState`.

### Q3. Which keyword is used with an asynchronous function?

**Answer:** `async`.

### Q4. Which keyword waits for a Promise?

**Answer:** `await`.

### Q5. Which block handles an API error?

**Answer:** `catch`.

### Q6. Which block is useful for stopping loading regardless of success or failure?

**Answer:** `finally`.

### Q7. What should the UI display while waiting for an API?

**Answer:** A loading state.

### Q8. What should the UI display when the API fails?

**Answer:** An error state.

### Q9. What is `response.json()` used for?

**Answer:** To parse the response body as JSON.

### Q10. What are the main API UI states?

**Answer:**

```text
⏳ Loading
✅ Success
❌ Error
📭 Empty
```

---

# 🧠 FINAL MEMORY TRICK

For every React API project, remember:

```text
🔍 SEARCH
   ↓
⏳ LOADING
   ↓
🌐 API REQUEST
   ↓
   ┌───────────────┐
   ↓               ↓
✅ SUCCESS       ❌ ERROR
   ↓               ↓
setData()       setError()
   │               │
   └───────┬───────┘
           ↓
   setLoading(false)
           ↓
         UI
```

And remember:

```text
UI
 ↓
Event
 ↓
Function
 ↓
API
 ↓
Response
 ↓
State
 ↓
Re-render
```

---

# ✅ CHAPTER 53 — PART 1 COMPLETE

We completed the first intermediate project:

```text
🌤️ WEATHER APP
```

You learned:

```text
✅ API integration
✅ fetch()
✅ async/await
✅ useState
✅ Forms
✅ Search
✅ Loading
✅ Error handling
✅ Empty state
✅ Environment variables
✅ Service files
✅ Props
✅ Callback functions
✅ Component architecture
```

According to your original syllabus, the remaining Chapter 53 projects are: **Movie Search App, Expense Tracker, Quiz App, Recipe App, and Product Listing App**.

# 🚀 NEXT

## CHAPTER 53 — PART 2

### 🎬 MOVIE SEARCH APP

We will build:

```text
Search movies
     ↓
API
     ↓
Movie results
     ↓
Movie cards
     ↓
Search
     ↓
Loading
     ↓
Error
     ↓
Movie details
```

Then we will move to the **Expense Tracker**.

Type **NEXT** to continue.