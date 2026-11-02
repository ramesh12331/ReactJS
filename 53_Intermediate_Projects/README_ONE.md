
# 🎬 CHAPTER 53 — INTERMEDIATE PROJECTS
## PART 2 — MOVIE SEARCH APP

In Part 1, we built the **Weather App** and learned how to connect React with an API.

Now we will build a **Movie Search App**.

The project will combine:

```text
React
+
API
+
Search
+
useState
+
async/await
+
Loading
+
Error handling
+
Conditional rendering
+
Reusable components
```

---

# 🎯 1. WHAT ARE WE BUILDING?

Our application will look conceptually like this:

```text
┌──────────────────────────────────────────┐
│             🎬 Movie Search              │
│                                          │
│ [ Search movie...              ] [🔍]    │
│                                          │
│ ┌──────────┐  ┌──────────┐  ┌──────────┐ │
│ │  Poster  │  │  Poster  │  │  Poster  │ │
│ │          │  │          │  │          │ │
│ │ Avatar   │  │ Batman   │  │ Inception│ │
│ │ 2022     │  │ 2024     │  │ 2010     │ │
│ └──────────┘  └──────────┘  └──────────┘ │
│                                          │
└──────────────────────────────────────────┘
```

The user searches:

```text
Inception
```

Then:

```text
User
 ↓
Search
 ↓
React
 ↓
Movie API
 ↓
JSON response
 ↓
Movie list
 ↓
Movie cards
```

---

# 🧠 2. WHAT MAKES THIS INTERMEDIATE?

Our earlier Todo App mostly worked with local data.

The Movie App works with **remote data**.

We need to handle:

```text
🔍 Search input
🌐 API request
⏳ Loading
❌ Error
📦 API response
🎬 Movie list
🧩 Movie cards
```

So we are combining multiple React concepts.

---

# 🧠 3. THINK BEFORE CODING

Before writing code, ask:

> What information does our application need?

We need:

```text
searchTerm
movies
loading
error
```

Therefore:

```jsx
const [searchTerm, setSearchTerm] =
    useState("");

const [movies, setMovies] =
    useState([]);

const [loading, setLoading] =
    useState(false);

const [error, setError] =
    useState("");
```

---

# 📦 4. UNDERSTAND EACH STATE

## `searchTerm`

Stores what the user typed.

```text
searchTerm = "Batman"
```

---

## `movies`

Stores the API result.

```javascript
[
    {
        title: "Batman",
        year: "2022"
    },
    {
        title: "Batman Begins",
        year: "2005"
    }
]
```

---

## `loading`

Tells us whether the request is running.

```text
false → request not running
true  → request running
```

---

## `error`

Stores an error message.

```text
"Movie not found"
```

---

# 📁 5. PROJECT STRUCTURE

A clean project can look like:

```text
movie-search-app/
│
├── src/
│   │
│   ├── components/
│   │   ├── SearchBar.jsx
│   │   ├── MovieCard.jsx
│   │   ├── MovieList.jsx
│   │   ├── Loading.jsx
│   │   └── ErrorMessage.jsx
│   │
│   ├── services/
│   │   └── movieApi.js
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

# 🌐 6. MOVIE API

Our React application needs movie data.

For example, an API might return:

```json
{
    "Search": [
        {
            "Title": "Inception",
            "Year": "2010",
            "Poster": "poster-url"
        },
        {
            "Title": "Inception: The Cobol Job",
            "Year": "2010",
            "Poster": "poster-url"
        }
    ]
}
```

The exact response depends on the movie API being used.

The important React concept is:

```text
API response
      ↓
JavaScript object
      ↓
React state
      ↓
map()
      ↓
MovieCard
```

---

# 🔐 7. API KEY

If your chosen movie API requires a key, create:

```text
.env
```

Example:

```env
VITE_MOVIE_API_KEY=your_api_key
```

Then:

```javascript
const API_KEY =
    import.meta.env.VITE_MOVIE_API_KEY;
```

### ⚠️ Important

Remember from Chapter 47:

```text
VITE_ variables
       ↓
Frontend
       ↓
Visible to browser code
```

Therefore, don't put private secrets in frontend environment variables.

---

# 🧩 8. CREATE API SERVICE

Create:

```text
src/services/movieApi.js
```

Example:

```javascript
const API_KEY =
    import.meta.env.VITE_MOVIE_API_KEY;


export async function searchMovies(
    searchTerm
) {

    const response =
        await fetch(
            `https://example.com/movies?search=${encodeURIComponent(searchTerm)}&apiKey=${API_KEY}`
        );


    if (!response.ok) {

        throw new Error(
            "Unable to fetch movies"
        );

    }


    const data =
        await response.json();


    return data;

}
```

The URL above is a **placeholder**. Replace it with the endpoint and response format of the movie API you choose.

---

# 🧠 9. WHY USE A SERVICE FILE?

Without a service:

```text
App.jsx
 ├── UI
 ├── state
 ├── API URL
 ├── fetch()
 ├── error handling
 └── rendering
```

This can become difficult to maintain.

With a service:

```text
App.jsx
 ↓
searchMovies()
 ↓
movieApi.js
 ↓
API
```

Much cleaner.

---

# 🔍 10. SEARCH FUNCTION

Now create the search function:

```jsx
async function handleSearch() {

    if (searchTerm.trim() === "") {

        setError(
            "Please enter a movie name."
        );

        return;
    }


    try {

        setLoading(true);

        setError("");

        setMovies([]);


        const data =
            await searchMovies(
                searchTerm
            );


        setMovies(
            data.Search || []
        );


    } catch (error) {

        setError(
            error.message
        );


    } finally {

        setLoading(false);

    }

}
```

---

# 🔍 11. UNDERSTAND THE FLOW

Suppose the user types:

```text
Batman
```

State:

```text
searchTerm = "Batman"
```

Then clicks Search.

```text
handleSearch()
      ↓
validate input
      ↓
loading = true
      ↓
clear previous error
      ↓
searchMovies("Batman")
      ↓
API
      ↓
response
      ↓
setMovies(...)
      ↓
loading = false
```

---

# 🧠 12. WHY `data.Search || []`?

Suppose the API returns:

```javascript
{
    Search: [...]
}
```

Then:

```javascript
data.Search
```

contains the movies.

But if the API does not provide `Search`, we can safely fall back to:

```javascript
[]
```

So:

```javascript
data.Search || []
```

means:

> Use `data.Search` if it exists; otherwise use an empty array.

---

# 🏠 13. APP COMPONENT

A simplified version:

```jsx
import { useState } from "react";

import { searchMovies }
    from "./services/movieApi";

import SearchBar
    from "./components/SearchBar";

import MovieList
    from "./components/MovieList";


function App() {

    const [searchTerm, setSearchTerm] =
        useState("");

    const [movies, setMovies] =
        useState([]);

    const [loading, setLoading] =
        useState(false);

    const [error, setError] =
        useState("");


    async function handleSearch() {

        if (searchTerm.trim() === "") {

            setError(
                "Please enter a movie name."
            );

            return;
        }


        try {

            setLoading(true);

            setError("");

            setMovies([]);


            const data =
                await searchMovies(
                    searchTerm
                );


            setMovies(
                data.Search || []
            );


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
                🎬 Movie Search
            </h1>


            <SearchBar
                searchTerm={searchTerm}
                setSearchTerm={setSearchTerm}
                onSearch={handleSearch}
            />


            {loading && (
                <p>
                    ⏳ Loading movies...
                </p>
            )}


            {error && (
                <p>
                    ❌ {error}
                </p>
            )}


            {!loading &&
             !error &&
             movies.length === 0 && (
                <p>
                    🔍 Search for a movie.
                </p>
            )}


            {!loading &&
             movies.length > 0 && (
                <MovieList
                    movies={movies}
                />
            )}

        </div>
    );
}


export default App;
```

---

# 🧩 14. SEARCH BAR COMPONENT

Create:

```text
src/components/SearchBar.jsx
```

```jsx
function SearchBar({
    searchTerm,
    setSearchTerm,
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
                type="text"
                value={searchTerm}
                onChange={event =>
                    setSearchTerm(
                        event.target.value
                    )
                }
                placeholder="Search movies..."
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

# 🧠 15. WHY `event.preventDefault()`?

A `<form>` normally submits the browser form.

That can cause the page to reload.

We don't want that.

So:

```javascript
event.preventDefault();
```

means:

> Stop the browser's default form submission behavior.

Then we handle the search ourselves.

---

# 🎬 16. MOVIE CARD

Create:

```text
src/components/MovieCard.jsx
```

```jsx
function MovieCard({
    movie
}) {

    return (
        <article>

            <img
                src={movie.Poster}
                alt={movie.Title}
            />

            <h2>
                {movie.Title}
            </h2>

            <p>
                📅 {movie.Year}
            </p>

        </article>
    );
}


export default MovieCard;
```

---

# 📋 17. MOVIE LIST

Create:

```text
src/components/MovieList.jsx
```

```jsx
import MovieCard
    from "./MovieCard";


function MovieList({
    movies
}) {

    return (
        <div>

            {movies.map(movie => (

                <MovieCard
                    key={movie.imdbID}
                    movie={movie}
                />

            ))}

        </div>
    );
}


export default MovieList;
```

---

# 🔑 18. WHY `key`?

React needs to identify each item in a list.

Bad:

```jsx
movies.map(movie => (
    <MovieCard
        movie={movie}
    />
))
```

Better:

```jsx
movies.map(movie => (
    <MovieCard
        key={movie.imdbID}
        movie={movie}
    />
))
```

The exact unique ID depends on the API response.

---

# 🧠 19. COMPONENT COMMUNICATION

Notice the data flow.

```text
App
 │
 ├── searchTerm
 │
 ↓
SearchBar
```

And:

```text
App
 │
 └── movies
       ↓
   MovieList
       ↓
   MovieCard
```

So:

```text
App
 │
 ├── SearchBar
 │
 └── MovieList
       │
       └── MovieCard
```

---

# 🔄 20. COMPLETE DATA FLOW

```text
                    App
                     │
        ┌────────────┴────────────┐
        ↓                         ↓
   SearchBar                  MovieList
        │                         │
        │ onSearch                │ movies
        ↓                         ↓
 handleSearch()              MovieCard
        │
        ↓
 searchMovies()
        │
        ↓
     Movie API
        │
        ↓
    JSON response
        │
        ↓
    setMovies()
        │
        └──────────────→ MovieList
```

This is a very important React architecture pattern.

---

# ⏳ 21. LOADING STATE

When searching:

```text
loading = true
```

Show:

```text
⏳ Loading movies...
```

After response:

```text
loading = false
```

Then show the results.

---

# ❌ 22. ERROR STATE

If something fails:

```text
error =
"Unable to fetch movies"
```

Display:

```text
❌ Unable to fetch movies
```

---

# 📭 23. EMPTY STATE

Before searching:

```text
🔍 Search for a movie.
```

If the API successfully returns no movies:

```text
No movies found.
```

These are actually two different situations.

### Before search

```text
No search performed yet.
```

### After search with zero results

```text
Search completed.
No movies found.
```

That distinction is important in professional UI development.

---

# 🧠 24. NO RESULTS STATE

We can handle it like:

```jsx
{!loading &&
 !error &&
 movies.length === 0 &&
 searchTerm && (
    <p>
        😕 No movies found.
    </p>
)}
```

---

# 🔍 25. SEARCH FLOW — DRY RUN

User types:

```text
Avatar
```

State:

```text
searchTerm = "Avatar"
movies = []
loading = false
error = ""
```

User presses Enter.

```text
handleSubmit()
       ↓
preventDefault()
       ↓
onSearch()
       ↓
handleSearch()
```

Then:

```text
loading = true
```

UI:

```text
⏳ Loading movies...
```

API request:

```text
searchMovies("Avatar")
```

API response:

```text
[
    Avatar,
    Avatar 2,
    Avatar 3
]
```

Then:

```text
setMovies(...)
```

React re-renders.

UI:

```text
┌─────────┐ ┌─────────┐ ┌─────────┐
│ Avatar  │ │ Avatar  │ │ Avatar  │
│ 2009    │ │ 2022    │ │ ...     │
└─────────┘ └─────────┘ └─────────┘
```

---

# ❌ 26. ERROR DRY RUN

User searches:

```text
XYZABC
```

API returns no results.

Then:

```text
movies = []
loading = false
```

UI:

```text
😕 No movies found.
```

If the network itself fails:

```text
catch
 ↓
setError()
 ↓
❌ Unable to fetch movies
```

---

# 🎨 27. BASIC MOVIE GRID

You can use CSS Grid:

```css
.movie-grid {
    display: grid;

    grid-template-columns:
        repeat(
            auto-fit,
            minmax(180px, 1fr)
        );

    gap: 20px;
}
```

Then:

```jsx
<div className="movie-grid">

    {movies.map(movie => (

        <MovieCard
            key={movie.imdbID}
            movie={movie}
        />

    ))}

</div>
```

This gives us a responsive movie grid.

---

# 📱 28. RESPONSIVE IDEA

Desktop:

```text
┌──────┐ ┌──────┐ ┌──────┐ ┌──────┐
│ Movie│ │ Movie│ │ Movie│ │ Movie│
└──────┘ └──────┘ └──────┘ └──────┘
```

Tablet:

```text
┌──────┐ ┌──────┐
│ Movie│ │ Movie│
└──────┘ └──────┘
```

Mobile:

```text
┌──────────┐
│   Movie  │
└──────────┘

┌──────────┐
│   Movie  │
└──────────┘
```

Responsive CSS allows the number of columns to adapt.

---

# 🚨 29. COMMON MISTAKES

## Mistake 1 — Not validating search

Bad:

```javascript
searchMovies("");
```

Better:

```javascript
if (searchTerm.trim() === "") {
    return;
}
```

---

## Mistake 2 — Not using `await`

Bad:

```javascript
const data =
    searchMovies(searchTerm);
```

Correct:

```javascript
const data =
    await searchMovies(searchTerm);
```

---

## Mistake 3 — Forgetting JSON parsing

Bad:

```javascript
return response;
```

Usually:

```javascript
return response.json();
```

---

## Mistake 4 — Forgetting keys

Bad:

```jsx
movies.map(movie => (
    <MovieCard movie={movie} />
))
```

Correct:

```jsx
movies.map(movie => (
    <MovieCard
        key={movie.imdbID}
        movie={movie}
    />
))
```

---

## Mistake 5 — Putting everything in `App.jsx`

Avoid making:

```text
App.jsx
1000+ lines
```

Instead:

```text
components/
services/
pages/
utils/
```

as the application grows.

---

# 🧠 30. WHAT WE HAVE LEARNED

The Weather App taught:

```text
API basics
```

The Movie App adds:

```text
API
+
Search
+
Lists
+
Reusable cards
+
Form submission
+
No-results state
+
Responsive grid
```

So now our architecture is becoming more realistic.

---

# 💼 INTERVIEW QUESTIONS

### Q1. Why should API logic be separated from components?

**Answer:**  
To separate data-fetching logic from UI logic and make the application easier to maintain and test.

---

### Q2. Why do we use `event.preventDefault()` in the search form?

**Answer:**  
To prevent the browser's default form submission and page reload.

---

### Q3. Why does a movie list need a `key`?

**Answer:**  
React uses the key to identify list items efficiently during rendering and updates.

---

### Q4. Why do we maintain a `loading` state?

**Answer:**  
To tell the user that an asynchronous operation is currently running.

---

### Q5. What is the difference between an empty state and a no-results state?

**Answer:**  

**Empty state:**

```text
The user hasn't searched yet.
```

**No-results state:**

```text
The user searched, but the API returned no matching movies.
```

---

### Q6. What happens after `setMovies(data)`?

**Answer:**  
React schedules a state update and re-renders the component so the new movie data can appear in the UI.

---

# 🧪 MINI CHALLENGE

Build your own Movie Search App.

### Requirements

```text
☐ Search input
☐ Search button
☐ Enter key support
☐ API request
☐ Loading state
☐ Error state
☐ No-results state
☐ Movie cards
☐ Poster
☐ Movie title
☐ Release year
☐ Responsive grid
```

### Bonus

Add:

```text
⭐ Rating
🎭 Genre
📅 Release date
🔎 Movie details
❤️ Favorite button
```

---

# 📝 10-QUESTION QUIZ

### Q1. What state stores the user's movie search?

**Answer:** `searchTerm`

### Q2. What state stores the movie results?

**Answer:** `movies`

### Q3. Which function can make an HTTP request from the browser?

**Answer:** `fetch()`

### Q4. Why use `async/await`?

**Answer:** To work with asynchronous operations in a readable way.

### Q5. Why use `event.preventDefault()`?

**Answer:** To prevent the default browser form submission.

### Q6. Why does each movie need a key?

**Answer:** To give React a stable identity for each list item.

### Q7. Which method is used to render the movie array?

**Answer:** `map()`

### Q8. What should be displayed while the API request is running?

**Answer:** A loading state.

### Q9. What should be displayed when the API returns zero movies?

**Answer:** A no-results state.

### Q10. Where can API-fetching logic be placed?

**Answer:** In a separate service/module such as:

```text
src/services/movieApi.js
```

---

# 🧠 FINAL MEMORY TRICK

Remember the Movie App as:

```text
🎬 MOVIE APP

SEARCH
   ↓
VALIDATE
   ↓
LOADING
   ↓
API
   ↓
JSON
   ↓
STATE
   ↓
MAP
   ↓
MOVIE CARDS
```

And remember the component structure:

```text
App
│
├── SearchBar
│
└── MovieList
      │
      ├── MovieCard
      ├── MovieCard
      └── MovieCard
```

---

# ✅ CHAPTER 53 — PART 2 COMPLETE

You have now completed:

```text
✅ Part 1 — Weather App
✅ Part 2 — Movie Search App
```

Next we continue with:

# 💰 CHAPTER 53 — PART 3
## EXPENSE TRACKER

We will learn:

```text
💰 Add expenses
📝 Forms
📊 Total expenses
🔢 Calculations
🗑️ Delete expenses
🏷️ Categories
🔎 Filter
↕️ Sort
💾 Local Storage
```

Type **NEXT** to continue with **Expense Tracker**.