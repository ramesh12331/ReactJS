# 🛍️ PRODUCT LISTING APP

So far we built:

```text
🌤️ Weather App
🎬 Movie Search App
💰 Expense Tracker
🧠 Quiz App
🍳 Recipe App
```

Now we build:

```text
🛍️ PRODUCT LISTING APP
```

This project is very important because it looks much closer to a **real e-commerce application**.

---

# 🎯 1. WHAT ARE WE BUILDING?

Our application will display products:

```text
┌─────────────────────────────────────────────┐
│             🛍️ Product Store                │
│                                             │
│ 🔍 [ Search products... ]                   │
│                                             │
│ Category: [ All ▼ ]                         │
│ Sort:     [ Price: Low → High ▼ ]          │
│                                             │
│ ┌─────────┐ ┌─────────┐ ┌─────────┐       │
│ │  📱     │ │  💻     │ │  🎧     │       │
│ │ Phone   │ │ Laptop  │ │ Headset │       │
│ │ ₹20,000 │ │ ₹60,000 │ │ ₹3,000  │       │
│ │ ⭐ 4.5  │ │ ⭐ 4.8  │ │ ⭐ 4.2  │       │
│ └─────────┘ └─────────┘ └─────────┘       │
│                                             │
│        ← 1  2  3  4  5 →                  │
└─────────────────────────────────────────────┘
```

---

# 🧠 2. WHY IS THIS PROJECT IMPORTANT?

Because it combines many concepts:

```text
API
 ↓
React State
 ↓
Search
 ↓
Filter
 ↓
Sort
 ↓
Pagination
 ↓
Loading
 ↓
Error Handling
 ↓
Reusable Components
```

This is very similar to real applications such as:

```text
Amazon
Flipkart
Myntra
E-commerce dashboards
Admin product management
```

---

# 🧠 3. WHAT WILL WE LEARN?

By completing this project, you practice:

```text
useState
useEffect
fetch()
async/await
API integration
map()
filter()
sort()
slice()
Search
Filtering
Sorting
Pagination
Props
Callbacks
Conditional rendering
Loading state
Error state
Empty state
```

---

# 🏗️ 4. APPLICATION FLOW

The complete flow:

```text
                 Product API
                     ↓
                   fetch()
                     ↓
                Product Data
                     ↓
                 React State
                     ↓
        ┌────────────┼────────────┐
        ↓            ↓            ↓
      Search       Filter       Sort
        │            │            │
        └────────────┼────────────┘
                     ↓
                 Pagination
                     ↓
                Product List
                     ↓
                Product Card
```

---

# 📁 5. PROJECT STRUCTURE

Create:

```text
product-app/
│
├── src/
│   │
│   ├── components/
│   │   ├── SearchBar.jsx
│   │   ├── CategoryFilter.jsx
│   │   ├── SortSelect.jsx
│   │   ├── ProductList.jsx
│   │   ├── ProductCard.jsx
│   │   ├── Pagination.jsx
│   │   └── Loading.jsx
│   │
│   ├── services/
│   │   └── productApi.js
│   │
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
│
└── package.json
```

---

# 🌐 6. API DATA

For learning, imagine our API returns:

```json
{
    "products": [
        {
            "id": 1,
            "title": "Smartphone",
            "price": 20000,
            "category": "electronics",
            "rating": 4.5,
            "thumbnail": "phone.jpg"
        }
    ]
}
```

Each product contains:

```text
Product
│
├── id
├── title
├── price
├── category
├── rating
└── thumbnail
```

---

# 🧠 7. API SERVICE

Create:

```text
src/services/productApi.js
```

```javascript
const API_URL =
    import.meta.env.VITE_PRODUCT_API_URL;


export async function getProducts() {

    const response =
        await fetch(
            `${API_URL}/products`
        );


    if (!response.ok) {

        throw new Error(
            "Unable to fetch products"
        );

    }


    const data =
        await response.json();


    return data.products || [];
}
```

---

# ⚛️ 8. APP STATE

Our application needs several pieces of state.

```jsx
import { useEffect, useState } from "react";


const [products, setProducts] =
    useState([]);


const [searchTerm, setSearchTerm] =
    useState("");


const [category, setCategory] =
    useState("All");


const [sortBy, setSortBy] =
    useState("default");


const [currentPage, setCurrentPage] =
    useState(1);


const [loading, setLoading] =
    useState(true);


const [error, setError] =
    useState("");
```

Let's understand them one by one.

---

# 📦 9. `products`

```javascript
const [products, setProducts] =
    useState([]);
```

Stores all products received from the API.

Example:

```text
products
   ↓
[
   Phone,
   Laptop,
   Headphones,
   Watch,
   Camera
]
```

---

# 🔍 10. `searchTerm`

```javascript
const [searchTerm, setSearchTerm] =
    useState("");
```

Stores what the user types.

Example:

```text
"phone"
```

---

# 🏷️ 11. `category`

```javascript
const [category, setCategory] =
    useState("All");
```

Initially:

```text
All
```

The user can choose:

```text
All
Electronics
Clothing
Books
Beauty
```

---

# ↕️ 12. `sortBy`

```javascript
const [sortBy, setSortBy] =
    useState("default");
```

Possible values:

```text
default
price-low
price-high
rating-high
name
```

---

# 📄 13. `currentPage`

```javascript
const [currentPage, setCurrentPage] =
    useState(1);
```

This tells us which page the user is viewing.

Example:

```text
Page 1
Page 2
Page 3
```

---

# ⏳ 14. `loading`

```javascript
const [loading, setLoading] =
    useState(true);
```

While API data is loading:

```text
⏳ Loading products...
```

---

# ❌ 15. `error`

```javascript
const [error, setError] =
    useState("");
```

If the API fails:

```text
❌ Unable to fetch products
```

---

# 🌐 16. FETCH PRODUCTS

We need to load products when the component starts.

Use `useEffect()`:

```jsx
useEffect(() => {

    async function loadProducts() {

        try {

            setLoading(true);

            setError("");


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
```

---

# 🧠 17. WHY `[]`?

The empty dependency array:

```javascript
[]
```

means:

```text
Run when component mounts.
```

So:

```text
App starts
   ↓
useEffect()
   ↓
loadProducts()
   ↓
API
   ↓
setProducts()
```

---

# 🔍 18. SEARCH PRODUCTS

We don't necessarily need another API request for every search.

We can filter the products already loaded.

```javascript
const searchedProducts =
    products.filter(product =>
        product.title
            .toLowerCase()
            .includes(
                searchTerm.toLowerCase()
            )
    );
```

---

# 🧠 19. UNDERSTAND `includes()`

Suppose:

```text
product.title =
"Apple Smartphone"
```

User enters:

```text
smartphone
```

We convert both to lowercase:

```text
apple smartphone
smartphone
```

Then:

```javascript
.includes("smartphone")
```

returns:

```text
true
```

So the product remains in the list.

---

# 🏷️ 20. CATEGORY FILTER

Now filter by category.

```javascript
const categoryProducts =
    category === "All"
        ? searchedProducts
        : searchedProducts.filter(
            product =>
                product.category ===
                category
        );
```

---

# 🧠 21. UNDERSTAND THE CONDITION

If:

```text
category = "All"
```

then:

```text
show everything
```

Otherwise:

```text
show only matching category
```

Flow:

```text
All
 ↓
All searched products
```

or:

```text
Electronics
 ↓
Only electronics
```

---

# ↕️ 22. SORTING

Now we need sorting.

Important:

> `sort()` modifies the array.

React state should not be mutated directly.

Therefore use:

```javascript
const sortedProducts =
    [...categoryProducts].sort(
        (a, b) => {

            if (
                sortBy ===
                "price-low"
            ) {
                return a.price - b.price;
            }


            if (
                sortBy ===
                "price-high"
            ) {
                return b.price - a.price;
            }


            if (
                sortBy ===
                "rating-high"
            ) {
                return b.rating - a.rating;
            }


            if (
                sortBy ===
                "name"
            ) {
                return a.title.localeCompare(
                    b.title
                );
            }


            return 0;
        }
    );
```

---

# 🧠 23. WHY `[...categoryProducts]`?

This:

```javascript
[...categoryProducts]
```

creates a new array.

Then:

```javascript
.sort()
```

works on the copy.

Without it:

```javascript
categoryProducts.sort()
```

could mutate the existing array.

Remember:

```text
❌ Don't mutate React state
✅ Create a new array
```

---

# 💰 24. PRICE LOW → HIGH

Suppose:

```text
Phone     ₹20,000
Laptop    ₹60,000
Watch     ₹10,000
```

Use:

```javascript
a.price - b.price
```

Result:

```text
₹10,000
₹20,000
₹60,000
```

---

# 💰 25. PRICE HIGH → LOW

Use:

```javascript
b.price - a.price
```

Result:

```text
₹60,000
₹20,000
₹10,000
```

---

# ⭐ 26. RATING HIGH → LOW

Use:

```javascript
b.rating - a.rating
```

Example:

```text
⭐ 4.9
⭐ 4.8
⭐ 4.5
⭐ 4.1
```

---

# 🔤 27. SORT BY NAME

Use:

```javascript
a.title.localeCompare(
    b.title
)
```

Example:

```text
Camera
Headphones
Laptop
Phone
Watch
```

---

# 📄 28. PAGINATION

Now comes an important concept.

Suppose we have:

```text
100 products
```

We don't want to display all 100 at once.

Maybe:

```text
10 products per page
```

Then:

```text
Page 1 → 1–10
Page 2 → 11–20
Page 3 → 21–30
...
```

---

# 🔢 29. PRODUCTS PER PAGE

Create:

```javascript
const productsPerPage = 8;
```

So:

```text
8 products
```

are displayed on each page.

---

# 🧮 30. CALCULATE INDEX

We need:

```javascript
const startIndex =
    (currentPage - 1) *
    productsPerPage;
```

Suppose:

```text
currentPage = 1
```

Then:

```text
(1 - 1) × 8
= 0
```

Start:

```text
0
```

---

# 🧮 31. PAGE 2

Suppose:

```text
currentPage = 2
```

Then:

```text
(2 - 1) × 8
= 8
```

Start:

```text
8
```

Therefore:

```text
Page 2
↓
items starting from index 8
```

---

# ✂️ 32. `slice()`

Now:

```javascript
const visibleProducts =
    sortedProducts.slice(
        startIndex,
        startIndex +
            productsPerPage
    );
```

Example:

```text
sortedProducts =
[0,1,2,3,4,5,6,7,8,9,10,11]
```

Page 1:

```text
slice(0, 8)
```

Result:

```text
0–7
```

Page 2:

```text
slice(8, 16)
```

Result:

```text
8–11
```

---

# 🧮 33. TOTAL PAGES

Calculate:

```javascript
const totalPages =
    Math.ceil(
        sortedProducts.length /
        productsPerPage
    );
```

Suppose:

```text
products = 25
productsPerPage = 8
```

Then:

```text
25 / 8
= 3.125
```

We need:

```text
4 pages
```

So:

```javascript
Math.ceil(3.125)
```

gives:

```text
4
```

---

# 🧠 34. COMPLETE DATA PROCESSING

This is extremely important.

Our data passes through several stages:

```text
All Products
      ↓
Search
      ↓
Category Filter
      ↓
Sort
      ↓
Pagination
      ↓
Visible Products
```

Think:

```text
API
 ↓
products
 ↓
searchedProducts
 ↓
categoryProducts
 ↓
sortedProducts
 ↓
visibleProducts
 ↓
UI
```

---

# 🛍️ 35. PRODUCT CARD

Create:

```text
src/components/ProductCard.jsx
```

```jsx
function ProductCard({
    product
}) {

    return (

        <article className="product-card">

            <img
                src={product.thumbnail}
                alt={product.title}
            />


            <h2>
                {product.title}
            </h2>


            <p>
                🏷️ {product.category}
            </p>


            <p>
                💰 ₹
                {product.price}
            </p>


            <p>
                ⭐ {product.rating}
            </p>


            <button>
                🛒 Add to Cart
            </button>

        </article>
    );
}


export default ProductCard;
```

---

# 📋 36. PRODUCT LIST

```jsx
import ProductCard
    from "./ProductCard";


function ProductList({
    products
}) {

    if (products.length === 0) {

        return (
            <p>
                📭 No products found.
            </p>
        );
    }


    return (

        <div className="product-grid">

            {products.map(product => (

                <ProductCard
                    key={product.id}
                    product={product}
                />

            ))}

        </div>
    );
}


export default ProductList;
```

---

# 🔍 37. SEARCH BAR

```jsx
function SearchBar({
    searchTerm,
    onSearchChange
}) {

    return (

        <input
            type="text"
            value={searchTerm}
            placeholder="🔍 Search products..."
            onChange={event =>
                onSearchChange(
                    event.target.value
                )
            }
        />
    );
}


export default SearchBar;
```

This is a controlled input.

---

# 🏷️ 38. CATEGORY FILTER COMPONENT

```jsx
function CategoryFilter({
    category,
    categories,
    onCategoryChange
}) {

    return (

        <select
            value={category}
            onChange={event =>
                onCategoryChange(
                    event.target.value
                )
            }
        >

            {categories.map(item => (

                <option
                    key={item}
                    value={item}
                >
                    {item}
                </option>

            ))}

        </select>
    );
}


export default CategoryFilter;
```

---

# ↕️ 39. SORT COMPONENT

```jsx
function SortSelect({
    sortBy,
    onSortChange
}) {

    return (

        <select
            value={sortBy}
            onChange={event =>
                onSortChange(
                    event.target.value
                )
            }
        >

            <option value="default">
                Default
            </option>

            <option value="price-low">
                Price: Low → High
            </option>

            <option value="price-high">
                Price: High → Low
            </option>

            <option value="rating-high">
                Rating: High → Low
            </option>

            <option value="name">
                Name: A → Z
            </option>

        </select>
    );
}


export default SortSelect;
```

---

# 📄 40. PAGINATION COMPONENT

Create:

```text
src/components/Pagination.jsx
```

```jsx
function Pagination({
    currentPage,
    totalPages,
    onPageChange
}) {

    if (totalPages <= 1) {
        return null;
    }


    return (

        <div>

            <button
                disabled={
                    currentPage === 1
                }
                onClick={() =>
                    onPageChange(
                        currentPage - 1
                    )
                }
            >
                ← Previous
            </button>


            <span>
                {" "}
                Page {currentPage}
                {" "}
                of {totalPages}
                {" "}
            </span>


            <button
                disabled={
                    currentPage ===
                    totalPages
                }
                onClick={() =>
                    onPageChange(
                        currentPage + 1
                    )
                }
            >
                Next →
            </button>

        </div>
    );
}


export default Pagination;
```

---

# 🧠 41. PAGINATION FLOW

Suppose:

```text
currentPage = 1
totalPages = 5
```

Previous:

```text
disabled = true
```

Next:

```text
disabled = false
```

User clicks Next:

```text
1 → 2
```

Now:

```text
Previous → enabled
Next → enabled
```

On page 5:

```text
Previous → enabled
Next → disabled
```

---

# 🏠 42. COMPLETE APP LOGIC

Now let's bring everything together.

```jsx
import {
    useEffect,
    useState
} from "react";


import {
    getProducts
} from "./services/productApi";


import SearchBar
    from "./components/SearchBar";


import CategoryFilter
    from "./components/CategoryFilter";


import SortSelect
    from "./components/SortSelect";


import ProductList
    from "./components/ProductList";


import Pagination
    from "./components/Pagination";


function App() {

    const [products, setProducts] =
        useState([]);


    const [searchTerm, setSearchTerm] =
        useState("");


    const [category, setCategory] =
        useState("All");


    const [sortBy, setSortBy] =
        useState("default");


    const [currentPage, setCurrentPage] =
        useState(1);


    const [loading, setLoading] =
        useState(true);


    const [error, setError] =
        useState("");


    useEffect(() => {

        async function loadProducts() {

            try {

                setLoading(true);

                setError("");


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


    const searchedProducts =
        products.filter(product =>
            product.title
                .toLowerCase()
                .includes(
                    searchTerm.toLowerCase()
                )
        );


    const categoryProducts =
        category === "All"
            ? searchedProducts
            : searchedProducts.filter(
                product =>
                    product.category ===
                    category
            );


    const sortedProducts =
        [...categoryProducts].sort(
            (a, b) => {

                if (
                    sortBy ===
                    "price-low"
                ) {
                    return (
                        a.price -
                        b.price
                    );
                }


                if (
                    sortBy ===
                    "price-high"
                ) {
                    return (
                        b.price -
                        a.price
                    );
                }


                if (
                    sortBy ===
                    "rating-high"
                ) {
                    return (
                        b.rating -
                        a.rating
                    );
                }


                if (
                    sortBy ===
                    "name"
                ) {
                    return (
                        a.title.localeCompare(
                            b.title
                        )
                    );
                }


                return 0;
            }
        );


    const productsPerPage = 8;


    const totalPages =
        Math.ceil(
            sortedProducts.length /
            productsPerPage
        );


    const startIndex =
        (currentPage - 1) *
        productsPerPage;


    const visibleProducts =
        sortedProducts.slice(
            startIndex,
            startIndex +
                productsPerPage
        );


    const categories = [
        "All",
        ...new Set(
            products.map(
                product =>
                    product.category
            )
        )
    ];


    if (loading) {

        return (
            <h2>
                ⏳ Loading products...
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

        <main>

            <h1>
                🛍️ Product Store
            </h1>


            <SearchBar
                searchTerm={searchTerm}
                onSearchChange={
                    value => {

                        setSearchTerm(value);

                        setCurrentPage(1);

                    }
                }
            />


            <CategoryFilter
                category={category}
                categories={categories}
                onCategoryChange={
                    value => {

                        setCategory(value);

                        setCurrentPage(1);

                    }
                }
            />


            <SortSelect
                sortBy={sortBy}
                onSortChange={
                    value => {

                        setSortBy(value);

                        setCurrentPage(1);

                    }
                }
            />


            <ProductList
                products={
                    visibleProducts
                }
            />


            <Pagination
                currentPage={
                    currentPage
                }
                totalPages={
                    totalPages
                }
                onPageChange={
                    setCurrentPage
                }
            />

        </main>
    );
}


export default App;
```

---

# 🧠 43. WHY RESET PAGE TO 1?

This is a very important real-world detail.

Suppose:

```text
Page 5
```

Then the user searches:

```text
phone
```

Maybe only 3 products match.

There is no Page 5 anymore.

So when search/filter/sort changes, do:

```javascript
setCurrentPage(1);
```

Flow:

```text
Page 5
   ↓
User searches
   ↓
New result set
   ↓
Reset
   ↓
Page 1
```

This prevents pagination bugs.

---

# 🔄 44. SEARCH + PAGINATION DRY RUN

Suppose:

```text
100 products
8 per page
```

Total:

```text
13 pages
```

User is on:

```text
Page 5
```

Now searches:

```text
Laptop
```

Only:

```text
12 products
```

remain.

New pages:

```text
12 / 8
= 1.5
```

Therefore:

```text
2 pages
```

If we stayed on Page 5:

```text
❌ No products
```

So:

```javascript
setCurrentPage(1);
```

gives:

```text
✅ Page 1
```

---

# 🧠 45. WHY USE DERIVED DATA?

Notice we don't store:

```text
searchedProducts
categoryProducts
sortedProducts
visibleProducts
```

inside `useState`.

Instead, we calculate them from:

```text
products
searchTerm
category
sortBy
currentPage
```

This is called **derived data**.

---

# 🚨 46. COMMON MISTAKE — DUPLICATE STATE

Avoid:

```javascript
const [products, setProducts] =
    useState([]);

const [filteredProducts, setFilteredProducts] =
    useState([]);
```

if `filteredProducts` can always be calculated from `products` and filter values.

Why?

Because duplicate state can become inconsistent.

Example:

```text
products = 100
filteredProducts = 90
```

Then one updates but the other doesn't.

Better:

```text
products
   +
searchTerm
   +
category
   +
sortBy
   ↓
filtered result
```

---

# 🚨 47. COMMON MISTAKE — MUTATING SORT

Don't:

```javascript
products.sort(...)
```

because `sort()` mutates the array.

Instead:

```javascript
[...products].sort(...)
```

---

# 🚨 48. COMMON MISTAKE — PAGINATION BEFORE FILTERING

Wrong conceptual order:

```text
Products
 ↓
Pagination
 ↓
Search
```

Imagine Page 1 contains products 1–8.

Then search:

```text
Laptop
```

You might get unexpected results.

Better:

```text
All Products
 ↓
Search
 ↓
Filter
 ↓
Sort
 ↓
Pagination
```

---

# 🧠 49. THE GOLDEN DATA PIPELINE

Remember:

```text
              API
               ↓
          ALL PRODUCTS
               ↓
             SEARCH
               ↓
            FILTER
               ↓
              SORT
               ↓
          PAGINATION
               ↓
        VISIBLE PRODUCTS
               ↓
         PRODUCT CARDS
```

This pipeline is one of the most important concepts in this project.

---

# 📊 50. PRODUCT GRID CSS

You can create a responsive grid:

```css
.product-grid {
    display: grid;

    grid-template-columns:
        repeat(
            auto-fit,
            minmax(220px, 1fr)
        );

    gap: 20px;
}


.product-card {
    padding: 16px;

    border: 1px solid #ddd;

    border-radius: 12px;
}


.product-card img {
    width: 100%;

    height: 200px;

    object-fit: contain;
}
```

The grid automatically adapts to screen size.

---

# 📱 51. RESPONSIVE DESIGN

Desktop:

```text
┌────┐ ┌────┐ ┌────┐ ┌────┐
│    │ │    │ │    │ │    │
└────┘ └────┘ └────┘ └────┘
```

Tablet:

```text
┌────┐ ┌────┐
│    │ │    │
└────┘ └────┘
```

Mobile:

```text
┌──────────┐
│          │
└──────────┘
```

Using:

```css
repeat(
    auto-fit,
    minmax(220px, 1fr)
)
```

helps create a responsive layout.

---

# 🛒 52. ADD TO CART — NEXT LEVEL

We can later add:

```text
🛒 Add to Cart
```

State:

```javascript
const [cart, setCart] =
    useState([]);
```

When the user clicks:

```text
Add to Cart
```

we can:

```javascript
setCart(
    previousCart => [
        ...previousCart,
        product
    ]
);
```

Then:

```text
Cart: 3 items
```

This becomes the foundation for a complete e-commerce project.

---

# 🧠 53. PERFORMANCE NOTE

For a small project, the calculations above are perfectly reasonable.

For very large datasets, we can later consider:

```text
useMemo()
Server-side filtering
Server-side sorting
Server-side pagination
Virtualized lists
Debounced search
```

Don't optimize too early.

First make the application correct.

---

# 💼 54. INTERVIEW QUESTIONS

### Q1. Why use `filter()`?

**Answer:**  
To create a new array containing only products that satisfy a condition.

---

### Q2. Why use `[...products].sort()`?

**Answer:**  
Because `sort()` mutates the array. The spread operator creates a copy first.

---

### Q3. How does pagination work?

**Answer:**  
We calculate the starting index and use `slice()` to select only the products for the current page.

---

### Q4. Why use `Math.ceil()` for total pages?

**Answer:**  
Because a partial final page still needs to count as a complete page.

---

### Q5. Why reset the page when search changes?

**Answer:**  
Because the new filtered result may contain fewer pages than the previous result.

---

### Q6. What is derived data?

**Answer:**  
Data calculated from existing state/props rather than stored as separate state.

---

### Q7. What is the correct processing order?

**Answer:**

```text
Search
 ↓
Filter
 ↓
Sort
 ↓
Pagination
```

---

### Q8. Why use `key={product.id}`?

**Answer:**  
React uses keys to identify list items efficiently and correctly.

---

### Q9. What does `slice()` do?

**Answer:**  
It creates a new array containing a selected portion of an array.

---

### Q10. Why use `useEffect()` for the initial API request?

**Answer:**  
Because the API request is a side effect that should happen after the component renders/mounts.

---

# 🧪 55. PRACTICE TASKS

### Task 1

Add a price range filter:

```text
Under ₹1,000
₹1,000–₹10,000
₹10,000+
```

---

### Task 2

Add rating filter:

```text
⭐ 4+
⭐ 3+
⭐ 2+
```

---

### Task 3

Add:

```text
Clear Filters
```

button.

It should reset:

```text
search
category
sort
page
```

---

### Task 4

Show:

```text
Showing 8 of 100 products
```

---

### Task 5

Add a product details screen.

---

### Task 6

Add:

```text
❤️ Favorite
```

---

### Task 7

Add Local Storage for favorites.

---

### Task 8

Add cart functionality.

---

# 🧩 56. MINI CHALLENGE

Build a complete:

# 🛍️ MINI E-COMMERCE PRODUCT PAGE

Requirements:

```text
☐ Fetch products from API
☐ Search
☐ Category filter
☐ Price filter
☐ Sort
☐ Pagination
☐ Loading
☐ Error
☐ Empty state
☐ Product cards
☐ Responsive grid
☐ Product details
☐ Add to cart
```

Bonus:

```text
☐ Favorites
☐ Local Storage
☐ Cart count
☐ Cart page
☐ Remove from cart
☐ Quantity
☐ Total price
```

---

# 📝 57. 10-QUESTION QUIZ

### Q1. Which method searches through an array based on a condition?

**Answer:** `filter()`.

---

### Q2. Which method sorts an array?

**Answer:** `sort()`.

---

### Q3. Why shouldn't we directly call `products.sort()` on React state?

**Answer:** Because `sort()` mutates the original array.

---

### Q4. Which method is useful for pagination?

**Answer:** `slice()`.

---

### Q5. How do we calculate total pages?

**Answer:**

```javascript
Math.ceil(
    products.length /
    productsPerPage
)
```

---

### Q6. What is the purpose of `currentPage`?

**Answer:** It stores the currently displayed page number.

---

### Q7. Why reset `currentPage` when search changes?

**Answer:** The new search results may have fewer pages.

---

### Q8. What is derived data?

**Answer:** Data calculated from existing state instead of stored separately.

---

### Q9. What is the correct product pipeline?

**Answer:**

```text
Search
 ↓
Filter
 ↓
Sort
 ↓
Pagination
```

---

### Q10. What major React concepts did this project combine?

**Answer:**

```text
useState
useEffect
API integration
fetch
async/await
map
filter
sort
slice
Props
Callbacks
Conditional rendering
Pagination
```

---

# 🧠 58. FINAL MEMORY TRICK

Remember the Product Listing App as:

```text
🛍️ PRODUCTS
      ↓
🔍 SEARCH
      ↓
🏷️ FILTER
      ↓
↕️ SORT
      ↓
📄 PAGINATE
      ↓
🃏 PRODUCT CARDS
```

Remember the array methods:

```text
map()
 ↓
DISPLAY

filter()
 ↓
SELECT MATCHING ITEMS

sort()
 ↓
ORDER ITEMS

slice()
 ↓
TAKE ONE PAGE
```

And remember:

```text
❌ Don't mutate state
✅ Create new arrays
```

---

# 🎯 CHAPTER 53 — COMPLETE!

You have now completed all six Intermediate Projects:

```text
┌────────────────────────────────────────┐
│      🚀 CHAPTER 53 COMPLETE            │
├────────────────────────────────────────┤
│                                        │
│ 🌤️  1. Weather App                    │
│ 🎬  2. Movie Search App               │
│ 💰  3. Expense Tracker                │
│ 🧠  4. Quiz App                       │
│ 🍳  5. Recipe App                     │
│ 🛍️  6. Product Listing App            │
│                                        │
└────────────────────────────────────────┘
```

You have practiced:

```text
React State
Props
Events
API Integration
fetch()
async/await
Loading
Error Handling
Search
Filtering
Sorting
Pagination
Conditional Rendering
Reusable Components
Local Storage
```

# 🧠 CHAPTER 53 BIG PICTURE

The six projects gradually increased your React skills:

```text
Weather
   ↓
Basic API

Movie Search
   ↓
API + Search

Expense Tracker
   ↓
Forms + State + Local Storage

Quiz
   ↓
State + Logic

Recipe App
   ↓
API + Search + Details

Product Listing
   ↓
API + Search + Filter + Sort + Pagination
```

This is exactly the type of progression you want for becoming comfortable building real React applications.

---

# 🚀 NEXT CHAPTER

## CHAPTER 54 — ADVANCED PROJECTS

Now we move into larger, portfolio-level applications:

```text
🏢 Enterprise Dashboard
🛒 E-Commerce Application
📊 Analytics Dashboard
💬 Real-Time Chat Application
📚 Learning Management System
🏥 Management System
```

Type **NEXT** to start **Chapter 54 — Advanced Projects**.