
# 🍳 CHAPTER 53 — INTERMEDIATE PROJECTS

# PART 5 — RECIPE APP

So far we completed:

```text
🌤️ Part 1 → Weather App
🎬 Part 2 → Movie Search App
💰 Part 3 → Expense Tracker
🧠 Part 4 → Quiz App
```

Now we will build:

# 🍳 RECIPE APP

This project will combine many React skills we already learned.

---

# 🎯 1. WHAT ARE WE BUILDING?

Our Recipe App will allow users to:

```text
🔍 Search recipes
🍳 Display recipe cards
🏷️ Filter recipes
🥕 Display ingredients
📋 Display instructions
📄 View recipe details
⏳ Show loading state
❌ Handle errors
🌐 Get data from an API
```

Example:

```text
┌───────────────────────────────────────┐
│          🍳 Recipe Finder             │
│                                       │
│ [ Search recipes...       ] [Search] │
│                                       │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐ │
│ │ 🍕      │ │ 🍔      │ │ 🍝      │ │
│ │ Pizza   │ │ Burger  │ │ Pasta   │ │
│ │ View →  │ │ View →  │ │ View →  │ │
│ └─────────┘ └─────────┘ └─────────┘ │
└───────────────────────────────────────┘
```

When the user clicks a recipe:

```text
┌───────────────────────────────────────┐
│ 🍕 Margherita Pizza                   │
│                                       │
│ Ingredients                           │
│ • Flour                               │
│ • Tomato                              │
│ • Cheese                              │
│ • Basil                               │
│                                       │
│ Instructions                          │
│ 1. Prepare the dough                  │
│ 2. Add tomato                         │
│ 3. Add cheese                         │
│ 4. Bake                               │
└───────────────────────────────────────┘
```

---

# 🧠 2. WHY IS THIS AN INTERMEDIATE PROJECT?

Because we combine several concepts:

```text
API
 ↓
fetch()
 ↓
async/await
 ↓
React State
 ↓
Search
 ↓
Filter
 ↓
List Rendering
 ↓
Props
 ↓
Conditional Rendering
 ↓
Loading/Error
 ↓
Recipe Details
```

This is much closer to a real-world application.

---

# 🧠 3. REAL-WORLD ANALOGY

Think about a food website.

You type:

```text
Pizza
```

The application sends:

```text
Pizza
 ↓
Recipe API
 ↓
Recipe results
 ↓
React
 ↓
Recipe Cards
```

Then you click one recipe:

```text
Recipe Card
     ↓
Recipe ID
     ↓
Recipe Details
     ↓
Ingredients + Instructions
```

---

# 🧩 4. WHAT WILL WE PRACTICE?

```text
useState
useEffect
fetch()
async/await
Props
Callbacks
map()
filter()
find()
Conditional Rendering
Loading state
Error state
Search
API integration
Reusable components
```

---

# 📁 5. PROJECT STRUCTURE

Create:

```text
recipe-app/
│
├── src/
│   │
│   ├── components/
│   │   ├── SearchBar.jsx
│   │   ├── RecipeList.jsx
│   │   ├── RecipeCard.jsx
│   │   ├── RecipeDetails.jsx
│   │   └── Loading.jsx
│   │
│   ├── services/
│   │   └── recipeApi.js
│   │
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
│
└── package.json
```

---

# 🧠 6. APPLICATION ARCHITECTURE

```text
                    App
                     │
        ┌────────────┼────────────┐
        ↓            ↓            ↓
   SearchBar     RecipeList    RecipeDetails
                     │
                     ↓
                RecipeCard
```

API:

```text
App
 ↓
recipeApi.js
 ↓
Recipe API
 ↓
JSON
 ↓
State
 ↓
UI
```

---

# 🌐 7. API CONCEPT

We need recipe data.

For learning, imagine the API endpoint:

```text
https://example.com/api/recipes
```

Search:

```text
https://example.com/api/recipes?search=pizza
```

The server may return:

```json
{
    "recipes": [
        {
            "id": 1,
            "name": "Margherita Pizza",
            "image": "pizza.jpg",
            "category": "Italian",
            "ingredients": [
                "Flour",
                "Tomato",
                "Cheese",
                "Basil"
            ],
            "instructions": [
                "Prepare the dough",
                "Add tomato",
                "Add cheese",
                "Bake"
            ]
        }
    ]
}
```

---

# 🧠 8. IMPORTANT

The API above is only an **example structure** for learning.

The important React concept is:

```text
React
 ↓
fetch()
 ↓
API
 ↓
JSON
 ↓
setState()
 ↓
UI
```

---

# 🔑 9. ENVIRONMENT VARIABLE

If the real recipe API requires an API key, don't hard-code it directly in your component.

Example:

```env
VITE_RECIPE_API_KEY=your_api_key
VITE_RECIPE_API_URL=https://example.com/api
```

Then:

```javascript
const API_URL =
    import.meta.env.VITE_RECIPE_API_URL;

const API_KEY =
    import.meta.env.VITE_RECIPE_API_KEY;
```

Remember:

> `VITE_` variables are available in browser code, so they are **not secret storage**.

Never put:

```text
Database password
Private backend secret
JWT secret
```

inside a frontend environment variable.

---

# 🧩 10. API SERVICE

Create:

```text
src/services/recipeApi.js
```

```javascript
const API_URL =
    import.meta.env.VITE_RECIPE_API_URL;

const API_KEY =
    import.meta.env.VITE_RECIPE_API_KEY;


export async function searchRecipes(
    searchTerm
) {

    const response =
        await fetch(
            `${API_URL}/recipes?search=${encodeURIComponent(searchTerm)}&apiKey=${API_KEY}`
        );


    if (!response.ok) {

        throw new Error(
            "Unable to fetch recipes"
        );

    }


    const data =
        await response.json();


    return data.recipes || [];
}
```

---

# 🔍 11. WHY `encodeURIComponent()`?

Suppose the user searches:

```text
chicken biryani
```

The space needs to be safely included in a URL.

Therefore:

```javascript
encodeURIComponent(
    "chicken biryani"
)
```

produces an encoded value suitable for the URL.

So:

```javascript
`...?search=${encodeURIComponent(searchTerm)}`
```

is safer than directly inserting the raw text.

---

# 🧠 12. APP STATE

In `App.jsx`:

```jsx
import { useState } from "react";


const [
    searchTerm,
    setSearchTerm
] = useState("");


const [
    recipes,
    setRecipes
] = useState([]);


const [
    loading,
    setLoading
] = useState(false);


const [
    error,
    setError
] = useState("");


const [
    selectedRecipe,
    setSelectedRecipe
] = useState(null);
```

Let's understand each one.

---

# 🔎 13. `searchTerm`

```javascript
const [searchTerm, setSearchTerm] =
    useState("");
```

Stores:

```text
What is the user searching for?
```

Example:

```text
pizza
```

---

# 🍳 14. `recipes`

```javascript
const [recipes, setRecipes] =
    useState([]);
```

Stores API results.

Example:

```text
recipes
   ↓
[
   Pizza,
   Pasta,
   Burger
]
```

---

# ⏳ 15. `loading`

```javascript
const [loading, setLoading] =
    useState(false);
```

Used while waiting for the API.

Flow:

```text
Search
 ↓
loading = true
 ↓
API request
 ↓
Response
 ↓
loading = false
```

---

# ❌ 16. `error`

```javascript
const [error, setError] =
    useState("");
```

If API fails:

```text
error =
"Unable to fetch recipes"
```

---

# 📄 17. `selectedRecipe`

```javascript
const [selectedRecipe, setSelectedRecipe] =
    useState(null);
```

Initially:

```text
null
```

means:

```text
No recipe selected
```

After clicking a card:

```text
selectedRecipe =
Pizza
```

---

# 🔍 18. SEARCH FUNCTION

Now create:

```javascript
async function handleSearch() {

    if (
        searchTerm.trim() === ""
    ) {

        setError(
            "Please enter a recipe name."
        );

        return;
    }


    try {

        setLoading(true);

        setError("");

        setRecipes([]);

        setSelectedRecipe(null);


        const data =
            await searchRecipes(
                searchTerm
            );


        setRecipes(data);

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

# 🧠 19. UNDERSTAND THE FLOW

When the user clicks Search:

```text
handleSearch()
      ↓
Validate input
      ↓
setLoading(true)
      ↓
API request
      ↓
Receive data
      ↓
setRecipes(data)
      ↓
setLoading(false)
```

---

# 🔄 20. WHY `try/catch/finally`?

We use:

```javascript
try {
    // API request
}
catch {
    // error
}
finally {
    // always stop loading
}
```

This is important because:

```text
Success
   ↓
loading = false

OR

Error
   ↓
loading = false
```

The `finally` block runs in both cases.

---

# 🔎 21. SEARCH BAR COMPONENT

Create:

```text
src/components/SearchBar.jsx
```

```jsx
function SearchBar({
    searchTerm,
    onSearchChange,
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
                placeholder="Search recipes..."
                onChange={event =>
                    onSearchChange(
                        event.target.value
                    )
                }
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

# 🧠 22. WHY `event.preventDefault()`?

A form normally tries to submit and reload/navigate the page.

We don't want that.

So:

```javascript
event.preventDefault();
```

keeps the user inside our React application.

---

# 🍳 23. RECIPE CARD

Create:

```text
src/components/RecipeCard.jsx
```

```jsx
function RecipeCard({
    recipe,
    onSelect
}) {

    return (

        <article>

            <img
                src={recipe.image}
                alt={recipe.name}
            />


            <h2>
                {recipe.name}
            </h2>


            <p>
                🏷️ {recipe.category}
            </p>


            <button
                onClick={() =>
                    onSelect(recipe)
                }
            >
                View Recipe →
            </button>

        </article>
    );
}


export default RecipeCard;
```

---

# 📋 24. RECIPE LIST

Create:

```text
src/components/RecipeList.jsx
```

```jsx
import RecipeCard
    from "./RecipeCard";


function RecipeList({
    recipes,
    onSelect
}) {

    if (recipes.length === 0) {

        return (
            <p>
                🍽️ No recipes found.
            </p>
        );

    }


    return (

        <div>

            {recipes.map(recipe => (

                <RecipeCard
                    key={recipe.id}
                    recipe={recipe}
                    onSelect={onSelect}
                />

            ))}

        </div>
    );
}


export default RecipeList;
```

---

# 🧠 25. WHY `key={recipe.id}`?

React needs a unique key when rendering lists.

Use:

```jsx
key={recipe.id}
```

instead of:

```jsx
key={recipe.name}
```

when the API provides a reliable unique ID.

---

# 📄 26. RECIPE DETAILS

Create:

```text
src/components/RecipeDetails.jsx
```

```jsx
function RecipeDetails({
    recipe,
    onClose
}) {

    if (!recipe) {
        return null;
    }


    return (

        <section>

            <button
                onClick={onClose}
            >
                ← Back
            </button>


            <h1>
                {recipe.name}
            </h1>


            <img
                src={recipe.image}
                alt={recipe.name}
            />


            <h2>
                🥕 Ingredients
            </h2>


            <ul>

                {recipe.ingredients.map(
                    (ingredient, index) => (

                        <li key={index}>
                            {ingredient}
                        </li>

                    )
                )}

            </ul>


            <h2>
                👨‍🍳 Instructions
            </h2>


            <ol>

                {recipe.instructions.map(
                    (step, index) => (

                        <li key={index}>
                            {step}
                        </li>

                    )
                )}

            </ol>

        </section>
    );
}


export default RecipeDetails;
```

---

# 🧠 27. WHY `if (!recipe) return null`?

Initially:

```javascript
selectedRecipe = null;
```

There is no recipe to display.

So:

```javascript
if (!recipe) {
    return null;
}
```

prevents React from trying to access:

```javascript
recipe.name
```

when `recipe` doesn't exist.

---

# 🔄 28. RECIPE SELECTION

When the user clicks:

```text
View Recipe →
```

we call:

```javascript
onSelect(recipe)
```

The parent receives the recipe:

```javascript
setSelectedRecipe(recipe);
```

Now:

```text
selectedRecipe
      ↓
RecipeDetails
```

---

# 🧩 29. COMPLETE APP

Now combine everything:

```jsx
import { useState } from "react";

import SearchBar
    from "./components/SearchBar";

import RecipeList
    from "./components/RecipeList";

import RecipeDetails
    from "./components/RecipeDetails";

import { searchRecipes }
    from "./services/recipeApi";


function App() {

    const [
        searchTerm,
        setSearchTerm
    ] = useState("");


    const [
        recipes,
        setRecipes
    ] = useState([]);


    const [
        loading,
        setLoading
    ] = useState(false);


    const [
        error,
        setError
    ] = useState("");


    const [
        selectedRecipe,
        setSelectedRecipe
    ] = useState(null);


    async function handleSearch() {

        if (
            searchTerm.trim() === ""
        ) {

            setError(
                "Please enter a recipe name."
            );

            return;
        }


        try {

            setLoading(true);

            setError("");

            setRecipes([]);

            setSelectedRecipe(null);


            const data =
                await searchRecipes(
                    searchTerm
                );


            setRecipes(data);

        } catch (error) {

            setError(
                error.message
            );

        } finally {

            setLoading(false);

        }

    }


    return (

        <main>

            <h1>
                🍳 Recipe Finder
            </h1>


            <SearchBar
                searchTerm={searchTerm}
                onSearchChange={
                    setSearchTerm
                }
                onSearch={
                    handleSearch
                }
            />


            {loading && (
                <p>
                    ⏳ Loading recipes...
                </p>
            )}


            {error && (
                <p>
                    ❌ {error}
                </p>
            )}


            {selectedRecipe ? (

                <RecipeDetails
                    recipe={
                        selectedRecipe
                    }
                    onClose={() =>
                        setSelectedRecipe(
                            null
                        )
                    }
                />

            ) : (

                <RecipeList
                    recipes={recipes}
                    onSelect={
                        setSelectedRecipe
                    }
                />

            )}

        </main>
    );
}


export default App;
```

---

# 🔍 30. COMPLETE DATA FLOW

This is the most important part.

```text
User enters:
"pizza"
       ↓
SearchBar
       ↓
handleSearch()
       ↓
searchRecipes()
       ↓
fetch()
       ↓
Recipe API
       ↓
JSON response
       ↓
setRecipes()
       ↓
RecipeList
       ↓
RecipeCard
```

When user clicks a card:

```text
RecipeCard
     ↓
onSelect(recipe)
     ↓
setSelectedRecipe(recipe)
     ↓
RecipeDetails
```

---

# ⏳ 31. LOADING STATE

When API request starts:

```javascript
setLoading(true);
```

UI:

```text
⏳ Loading recipes...
```

After response:

```javascript
setLoading(false);
```

The loading message disappears.

---

# ❌ 32. ERROR STATE

Suppose API fails.

```javascript
catch (error) {
    setError(error.message);
}
```

UI:

```text
❌ Unable to fetch recipes
```

This is much better than showing a blank page.

---

# 📭 33. EMPTY STATE

Suppose the API returns:

```javascript
[]
```

Then:

```jsx
if (recipes.length === 0)
```

can show:

```text
🍽️ No recipes found.
```

Good applications should handle:

```text
Loading
Success
Error
Empty
```

---

# 🏷️ 34. ADD CATEGORY FILTER

Suppose each recipe contains:

```javascript
category: "Italian"
```

We can add:

```javascript
const [category, setCategory] =
    useState("All");
```

Then:

```javascript
const filteredRecipes =
    category === "All"
        ? recipes
        : recipes.filter(
            recipe =>
                recipe.category ===
                category
        );
```

---

# 🧠 35. FILTER FLOW

```text
API recipes
     ↓
category filter
     ↓
filteredRecipes
     ↓
RecipeList
```

Example:

```text
All
 ↓
Pizza
Pasta
Burger
Curry
```

Select:

```text
Italian
```

Result:

```text
Pizza
Pasta
```

---

# 🔍 36. SEARCH + FILTER

We can combine:

```text
Search
+
Category
```

For example:

```text
Search = chicken
Category = Indian
```

Then only matching recipes are displayed.

---

# 🧠 37. `filter()` DRY RUN

Suppose:

```javascript
recipes = [
    {
        name: "Pizza",
        category: "Italian"
    },
    {
        name: "Biryani",
        category: "Indian"
    },
    {
        name: "Pasta",
        category: "Italian"
    }
];
```

Filter:

```javascript
recipes.filter(
    recipe =>
        recipe.category === "Italian"
);
```

Result:

```text
Pizza
Pasta
```

---

# 🧪 38. COMPLETE DRY RUN

User opens application.

Initial state:

```text
searchTerm = ""
recipes = []
loading = false
error = ""
selectedRecipe = null
```

---

## STEP 1 — SEARCH

User types:

```text
pizza
```

State:

```text
searchTerm = "pizza"
```

---

## STEP 2 — CLICK SEARCH

```text
handleSearch()
```

runs.

First:

```text
loading = true
```

UI:

```text
⏳ Loading recipes...
```

---

## STEP 3 — API

Request:

```text
GET /recipes?search=pizza
```

API returns:

```text
Pizza
Margherita Pizza
Pepperoni Pizza
```

---

## STEP 4 — SAVE DATA

```javascript
setRecipes(data);
```

Now:

```text
recipes
   ↓
Pizza
Margherita Pizza
Pepperoni Pizza
```

---

## STEP 5 — DISPLAY

```text
RecipeList
     ↓
RecipeCard
     ↓
Pizza cards
```

---

## STEP 6 — SELECT

User clicks:

```text
View Recipe →
```

Then:

```text
setSelectedRecipe(recipe)
```

---

## STEP 7 — DETAILS

Now:

```text
selectedRecipe !== null
```

So we show:

```text
RecipeDetails
```

instead of:

```text
RecipeList
```

---

# 🔄 39. CONDITIONAL UI

Our application has two major screens:

```text
selectedRecipe === null
        ↓
Recipe List
```

and:

```text
selectedRecipe !== null
        ↓
Recipe Details
```

This is a simple way to create a detail view without introducing routing yet.

---

# 🚨 40. COMMON MISTAKES

## Mistake 1 — Calling API directly during rendering

Avoid:

```jsx
const data = fetch(...);
```

directly inside the component render body.

Use an event handler for search or `useEffect` when data should load based on state/lifecycle.

---

## Mistake 2 — Forgetting `await`

Wrong:

```javascript
const response = fetch(url);
```

when you expect the response body immediately.

Better:

```javascript
const response =
    await fetch(url);
```

---

## Mistake 3 — Forgetting `response.ok`

Don't assume every HTTP response is successful.

Use:

```javascript
if (!response.ok) {
    throw new Error(
        "Unable to fetch recipes"
    );
}
```

---

## Mistake 4 — Forgetting `response.json()`

The response object isn't automatically your JavaScript data.

Use:

```javascript
const data =
    await response.json();
```

---

## Mistake 5 — Accessing `recipe` when it is null

Bad:

```javascript
recipe.name
```

when:

```text
recipe = null
```

Use conditional rendering or:

```javascript
if (!recipe) {
    return null;
}
```

---

## Mistake 6 — Using array index as the main recipe key

Prefer:

```jsx
key={recipe.id}
```

when the API provides a stable unique ID.

---

# 💼 41. INTERVIEW QUESTIONS

### Q1. Why separate API logic into `recipeApi.js`?

**Answer:**  
It keeps API communication separate from UI components and makes the code easier to maintain and reuse.

---

### Q2. What does `loading` represent?

**Answer:**  
It represents whether an API request is currently in progress.

---

### Q3. Why use `try/catch/finally`?

**Answer:**  
`try` performs the request, `catch` handles errors, and `finally` performs cleanup such as stopping the loading indicator.

---

### Q4. Why use `response.json()`?

**Answer:**  
It parses the response body as JSON so JavaScript can work with the returned data.

---

### Q5. Why use `encodeURIComponent()`?

**Answer:**  
To safely encode user-entered text before putting it into a URL.

---

### Q6. How does RecipeCard communicate the selected recipe to App?

**Answer:**  
`App` passes a callback through props, and `RecipeCard` calls that callback with the selected recipe.

---

### Q7. Why is `selectedRecipe` initially `null`?

**Answer:**  
Because no recipe has been selected when the application first loads.

---

### Q8. What states should an API-driven React UI normally handle?

**Answer:**

```text
Loading
Success
Error
Empty
```

---

# 🧪 42. PRACTICE TASKS

### Task 1

Add:

```text
⭐ Rating
```

to each recipe card.

---

### Task 2

Add:

```text
⏱️ Cooking Time
```

---

### Task 3

Add a category dropdown:

```text
All
Indian
Italian
Chinese
Mexican
```

---

### Task 4

Add a search button loading state:

```text
Searching...
```

---

### Task 5

Add a Back button to Recipe Details.

---

### Task 6

Add:

```text
❤️ Favorite
```

to each recipe.

---

### Task 7

Save favorite recipes in Local Storage.

---

# 🧩 43. MINI CHALLENGE

Build your own **Recipe Finder**.

Required:

```text
☐ Search recipes
☐ API integration
☐ Recipe cards
☐ Recipe image
☐ Recipe name
☐ Category
☐ Recipe details
☐ Ingredients
☐ Instructions
☐ Loading state
☐ Error state
☐ Empty state
☐ Back button
```

Bonus:

```text
☐ Category filter
☐ Rating
☐ Cooking time
☐ Favorites
☐ Local Storage
☐ Pagination
```

---

# 📝 44. 10-QUESTION QUIZ

### Q1. What function performs the API request?

**Answer:** `fetch()`.

---

### Q2. What does `async` allow us to use?

**Answer:** `await` inside the function.

---

### Q3. What does `await fetch()` give us?

**Answer:** A `Response` object.

---

### Q4. What converts the response body into JSON?

**Answer:**

```javascript
response.json()
```

---

### Q5. What state stores the recipe results?

**Answer:**

```javascript
recipes
```

---

### Q6. What state stores the currently selected recipe?

**Answer:**

```javascript
selectedRecipe
```

---

### Q7. Which array method is used to display recipes?

**Answer:** `map()`.

---

### Q8. Which array method is useful for category filtering?

**Answer:** `filter()`.

---

### Q9. Why use `response.ok`?

**Answer:** To check whether the HTTP response was successful before processing it as a successful API result.

---

### Q10. What are the four important API UI states?

**Answer:**

```text
⏳ Loading
✅ Success
❌ Error
📭 Empty
```

---

# 🧠 FINAL MEMORY TRICK

Remember the Recipe App as:

```text
🔍 SEARCH
    ↓
🌐 API
    ↓
📦 JSON
    ↓
⚛️ STATE
    ↓
🍳 RECIPE LIST
    ↓
🃏 RECIPE CARD
    ↓
👆 SELECT
    ↓
📄 DETAILS
```

For API applications:

```text
Request
   ↓
Loading
   ↓
 ┌───────────────┐
 ↓               ↓
Success         Error
 ↓               ↓
Data            Message
 ↓
UI
```

And remember:

```text
map()
   ↓
Display recipes

filter()
   ↓
Filter recipes

fetch()
   ↓
Get API data

JSON()
   ↓
Read response

useState()
   ↓
Store UI data
```

---

# ✅ CHAPTER 53 — PART 5 COMPLETE

We have completed:

```text
🌤️ Weather App
🎬 Movie Search App
💰 Expense Tracker
🧠 Quiz App
🍳 Recipe App
```

Only one project remains in our Chapter 53 Intermediate Projects section:

# 🛍️ PART 6 — PRODUCT LISTING APP

We will build a more **e-commerce-style React application** with:

```text
🛍️ Products
🔍 Search
🏷️ Category Filter
↕️ Sorting
📄 Pagination
⏳ Loading
❌ Error Handling
💰 Price
⭐ Rating
🛒 Add to Cart
```

Type **NEXT** to continue.