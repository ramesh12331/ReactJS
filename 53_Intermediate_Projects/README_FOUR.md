
# 🧠 CHAPTER 53 — INTERMEDIATE PROJECTS

# PART 4 — QUIZ APP

So far:

```text
🌤️ Part 1 → Weather App
🎬 Part 2 → Movie Search App
💰 Part 3 → Expense Tracker
```

Now:

```text
🧠 Part 4 → Quiz App
```

This project will teach us how to manage **multiple states, arrays, user answers, scores, question navigation, and result screens**.

---

# 🎯 1. WHAT ARE WE BUILDING?

We want to create a quiz application like this:

```text
┌─────────────────────────────────────┐
│           🧠 React Quiz             │
│                                     │
│ Question 1 of 5                     │
│                                     │
│ What is React?                      │
│                                     │
│ ○ JavaScript Library                │
│ ○ Database                          │
│ ○ Programming Language              │
│ ○ Operating System                  │
│                                     │
│              [ Next → ]             │
└─────────────────────────────────────┘
```

After the final question:

```text
┌─────────────────────────────────────┐
│          🎉 Quiz Complete!          │
│                                     │
│             Score                  │
│                                     │
│              4 / 5                  │
│                                     │
│          ⭐ Great Job!              │
│                                     │
│          [ 🔄 Restart ]             │
└─────────────────────────────────────┘
```

---

# 🧠 2. WHAT WILL WE LEARN?

This project combines:

```text
useState
Arrays
Objects
map()
Conditional rendering
Events
Props
Callbacks
Array indexing
Score calculation
Question navigation
Forms/buttons
```

We can later extend it with:

```text
🌐 API
⏱️ Timer
📊 Categories
🎯 Difficulty
🏆 High score
💾 Local Storage
```

---

# 🧠 3. REAL-WORLD ANALOGY

Think about a school examination.

```text
Question
   ↓
Student selects answer
   ↓
Next question
   ↓
Student selects answer
   ↓
...
   ↓
Final result
```

Our React application does the same thing:

```text
Question State
      ↓
Answer Selection
      ↓
Next Question
      ↓
Score Update
      ↓
Final Result
```

---

# 🧠 4. FIRST THINK — WHAT DATA DO WE NEED?

Before coding, ask:

> What does a quiz contain?

A quiz contains multiple questions.

Example:

```javascript
const questions = [
    {
        id: 1,
        question: "What is React?",
        options: [
            "JavaScript Library",
            "Database",
            "Operating System",
            "Programming Language"
        ],
        answer: "JavaScript Library"
    },

    {
        id: 2,
        question: "Which Hook manages state?",
        options: [
            "useState",
            "useEffect",
            "useRef",
            "useMemo"
        ],
        answer: "useState"
    }
];
```

---

# 📦 5. UNDERSTAND THE DATA

Each question is an object:

```text
Question
│
├── id
├── question
├── options
└── answer
```

Multiple questions form an array:

```text
questions[]
│
├── Question 1
├── Question 2
├── Question 3
├── Question 4
└── Question 5
```

---

# 🧠 6. WHAT SHOULD BE STATE?

This is the most important part.

We need to know:

### Current question

```javascript
const [currentQuestion, setCurrentQuestion] =
    useState(0);
```

### Selected answer

```javascript
const [selectedAnswer, setSelectedAnswer] =
    useState("");
```

### Score

```javascript
const [score, setScore] =
    useState(0);
```

### Quiz finished?

```javascript
const [quizFinished, setQuizFinished] =
    useState(false);
```

Therefore:

```text
currentQuestion
       ↓
Which question are we showing?

selectedAnswer
       ↓
What answer did the user select?

score
       ↓
How many correct answers?

quizFinished
       ↓
Should we show the result?
```

---

# 📁 7. PROJECT STRUCTURE

A clean structure:

```text
quiz-app/
│
├── src/
│   │
│   ├── components/
│   │   ├── Quiz.jsx
│   │   ├── Question.jsx
│   │   ├── Result.jsx
│   │   └── Progress.jsx
│   │
│   ├── data/
│   │   └── questions.js
│   │
│   ├── App.jsx
│   ├── App.css
│   └── main.jsx
│
└── package.json
```

---

# 📦 8. QUESTIONS FILE

Create:

```text
src/data/questions.js
```

```javascript
const questions = [
    {
        id: 1,

        question:
            "What is React?",

        options: [
            "JavaScript Library",
            "Database",
            "Operating System",
            "Programming Language"
        ],

        answer:
            "JavaScript Library"
    },

    {
        id: 2,

        question:
            "Which Hook is used for state?",

        options: [
            "useState",
            "useEffect",
            "useRef",
            "useMemo"
        ],

        answer:
            "useState"
    },

    {
        id: 3,

        question:
            "Which syntax is used for JavaScript inside JSX?",

        options: [
            "{}",
            "[]",
            "()",
            "<>"
        ],

        answer:
            "{}"
    }
];

export default questions;
```

---

# 🧠 9. WHY KEEP QUESTIONS IN A SEPARATE FILE?

Instead of:

```text
App.jsx
↓
100 questions
↓
UI
↓
Logic
```

we separate:

```text
data/
   ↓
questions.js

components/
   ↓
UI

App.jsx
   ↓
application logic
```

This makes the project easier to maintain.

---

# 🏠 10. BASIC APP

Start with:

```jsx
import { useState } from "react";

import questions
    from "./data/questions";


function App() {

    const [
        currentQuestion,
        setCurrentQuestion
    ] = useState(0);


    const [
        selectedAnswer,
        setSelectedAnswer
    ] = useState("");


    const [
        score,
        setScore
    ] = useState(0);


    const [
        quizFinished,
        setQuizFinished
    ] = useState(false);


    return (
        <div>

            <h1>
                🧠 React Quiz
            </h1>

        </div>
    );
}


export default App;
```

---

# 🔢 11. WHAT DOES `currentQuestion = 0` MEAN?

JavaScript arrays start at index `0`.

Our questions:

```text
Index 0 → Question 1
Index 1 → Question 2
Index 2 → Question 3
```

Therefore:

```javascript
questions[currentQuestion]
```

when:

```text
currentQuestion = 0
```

means:

```text
questions[0]
```

which gives Question 1.

---

# 🧠 12. GET CURRENT QUESTION

We can write:

```javascript
const question =
    questions[currentQuestion];
```

Suppose:

```text
currentQuestion = 1
```

Then:

```text
question =
questions[1]
```

We get Question 2.

---

# 🎯 13. DISPLAY THE QUESTION

```jsx
<h2>
    {question.question}
</h2>
```

If:

```javascript
question.question =
"What is React?"
```

The UI displays:

```text
What is React?
```

---

# 🔘 14. DISPLAY OPTIONS

Remember:

```javascript
question.options
```

is an array.

Example:

```javascript
[
    "JavaScript Library",
    "Database",
    "Operating System",
    "Programming Language"
]
```

Use `map()`:

```jsx
{question.options.map(option => (

    <button
        key={option}
        onClick={() =>
            setSelectedAnswer(option)
        }
    >
        {option}
    </button>

))}
```

---

# 🔄 15. UNDERSTAND THE FLOW

User clicks:

```text
JavaScript Library
```

Then:

```text
onClick
   ↓
setSelectedAnswer(
    "JavaScript Library"
)
   ↓
selectedAnswer changes
   ↓
React re-renders
```

Now React knows which answer the user selected.

---

# 🧠 16. COMPLETE BASIC QUESTION UI

```jsx
function Question({
    question,
    selectedAnswer,
    onAnswer
}) {

    return (

        <div>

            <h2>
                {question.question}
            </h2>


            {question.options.map(
                option => (

                    <button
                        key={option}
                        onClick={() =>
                            onAnswer(option)
                        }
                    >
                        {option}
                    </button>

                )
            )}

        </div>
    );
}


export default Question;
```

---

# 📤 17. PROPS FLOW

Parent:

```text
App
```

passes:

```text
question
selectedAnswer
onAnswer
```

to:

```text
Question
```

Flow:

```text
App
 │
 ├── question
 ├── selectedAnswer
 └── onAnswer()
        ↓
    Question
```

---

# 🧠 18. WHY USE A CALLBACK?

The child component needs to tell the parent:

> "The user selected this answer."

So:

```text
Question
   ↓
onAnswer(option)
   ↓
App
   ↓
setSelectedAnswer()
```

This is **Child → Parent communication**.

---

# ✅ 19. CHECK THE ANSWER

Suppose:

```text
selectedAnswer =
"JavaScript Library"
```

And:

```text
question.answer =
"JavaScript Library"
```

Compare:

```javascript
if (
    selectedAnswer ===
    question.answer
) {
    // correct
}
```

If equal:

```text
Correct ✅
```

Otherwise:

```text
Wrong ❌
```

---

# 🧠 20. NEXT BUTTON

We need a function:

```javascript
function handleNext() {

    if (
        selectedAnswer ===
        question.answer
    ) {

        setScore(
            previousScore =>
                previousScore + 1
        );

    }


    if (
        currentQuestion <
        questions.length - 1
    ) {

        setCurrentQuestion(
            previous =>
                previous + 1
        );

        setSelectedAnswer("");

    } else {

        setQuizFinished(true);

    }

}
```

---

# 🔍 21. UNDERSTAND THE FIRST PART

```javascript
if (
    selectedAnswer ===
    question.answer
)
```

We compare:

```text
User answer
     ↓
Correct answer
```

Example:

```text
"JavaScript Library"
        ===
"JavaScript Library"
```

Result:

```text
true
```

Therefore:

```javascript
setScore(
    previousScore =>
        previousScore + 1
);
```

---

# 🔢 22. WHY `previousScore + 1`?

Suppose:

```text
score = 2
```

Correct answer:

```text
2 + 1
```

New score:

```text
3
```

We use the functional form:

```javascript
setScore(
    previousScore =>
        previousScore + 1
);
```

because the new value depends on the previous value.

---

# ➡️ 23. MOVE TO NEXT QUESTION

Suppose:

```text
currentQuestion = 0
```

Then:

```javascript
setCurrentQuestion(
    previous =>
        previous + 1
);
```

becomes:

```text
0 + 1
```

Result:

```text
currentQuestion = 1
```

Now React displays Question 2.

---

# 🛑 24. LAST QUESTION

We don't want:

```text
questions[3]
```

when there are only 3 questions.

Remember:

```text
questions.length = 3

Last index = 2
```

Therefore:

```javascript
currentQuestion <
questions.length - 1
```

checks whether another question exists.

---

# 🧠 25. VISUALIZE IT

For 3 questions:

```text
Question 1
index = 0
     ↓
Question 2
index = 1
     ↓
Question 3
index = 2
     ↓
FINISHED
```

The condition:

```javascript
currentQuestion <
questions.length - 1
```

protects us from going beyond the final question.

---

# 🎉 26. QUIZ FINISHED

When the last question is answered:

```javascript
setQuizFinished(true);
```

Now:

```text
quizFinished = true
```

We can show:

```text
🎉 Quiz Complete!

Score: 2 / 3

[ Restart ]
```

---

# 🔄 27. CONDITIONAL RENDERING

We can write:

```jsx
if (quizFinished) {

    return (
        <Result
            score={score}
            total={questions.length}
            onRestart={handleRestart}
        />
    );

}
```

Otherwise:

```text
Show question
```

This is conditional rendering.

---

# 🏆 28. RESULT COMPONENT

Create:

```text
src/components/Result.jsx
```

```jsx
function Result({
    score,
    total,
    onRestart
}) {

    return (

        <div>

            <h1>
                🎉 Quiz Complete!
            </h1>


            <h2>
                Score:
                {" "}
                {score} / {total}
            </h2>


            <button
                onClick={onRestart}
            >
                🔄 Restart
            </button>

        </div>
    );
}


export default Result;
```

---

# 🔄 29. RESTART FUNCTION

In `App.jsx`:

```javascript
function handleRestart() {

    setCurrentQuestion(0);

    setSelectedAnswer("");

    setScore(0);

    setQuizFinished(false);

}
```

Now:

```text
Restart
   ↓
Question = 0
Score = 0
Answer = ""
Finished = false
   ↓
Question 1
```

---

# 🧩 30. COMPLETE APP

Now combine everything:

```jsx
import { useState } from "react";

import questions
    from "./data/questions";

import Question
    from "./components/Question";

import Result
    from "./components/Result";


function App() {

    const [
        currentQuestion,
        setCurrentQuestion
    ] = useState(0);


    const [
        selectedAnswer,
        setSelectedAnswer
    ] = useState("");


    const [
        score,
        setScore
    ] = useState(0);


    const [
        quizFinished,
        setQuizFinished
    ] = useState(false);


    const question =
        questions[currentQuestion];


    function handleAnswer(answer) {

        setSelectedAnswer(answer);

    }


    function handleNext() {

        if (
            selectedAnswer ===
            question.answer
        ) {

            setScore(
                previousScore =>
                    previousScore + 1
            );

        }


        if (
            currentQuestion <
            questions.length - 1
        ) {

            setCurrentQuestion(
                previous =>
                    previous + 1
            );

            setSelectedAnswer("");

        } else {

            setQuizFinished(true);

        }

    }


    function handleRestart() {

        setCurrentQuestion(0);

        setSelectedAnswer("");

        setScore(0);

        setQuizFinished(false);

    }


    if (quizFinished) {

        return (

            <Result
                score={score}
                total={questions.length}
                onRestart={handleRestart}
            />

        );
    }


    return (

        <div>

            <h1>
                🧠 React Quiz
            </h1>


            <p>
                Question{" "}
                {currentQuestion + 1}
                {" "}
                of{" "}
                {questions.length}
            </p>


            <Question
                question={question}
                selectedAnswer={
                    selectedAnswer
                }
                onAnswer={
                    handleAnswer
                }
            />


            <button
                onClick={handleNext}
                disabled={
                    selectedAnswer === ""
                }
            >
                Next →
            </button>

        </div>
    );
}


export default App;
```

---

# 🧠 31. IMPORTANT: `disabled`

We have:

```jsx
disabled={
    selectedAnswer === ""
}
```

If the user hasn't selected anything:

```text
selectedAnswer = ""
```

Then:

```text
Next → DISABLED
```

After selecting:

```text
selectedAnswer = "useState"
```

Then:

```text
Next → ENABLED
```

This prevents the user from moving forward without answering.

---

# 🧪 32. COMPLETE DRY RUN

Suppose we have:

```text
3 questions
```

Initial state:

```text
currentQuestion = 0
selectedAnswer = ""
score = 0
quizFinished = false
```

UI:

```text
Question 1 of 3

What is React?

[JavaScript Library]
[Database]
[Operating System]
[Programming Language]
```

---

## STEP 1

User selects:

```text
JavaScript Library
```

State:

```text
selectedAnswer =
"JavaScript Library"
```

---

## STEP 2

User clicks:

```text
Next
```

Compare:

```text
selectedAnswer
        ↓
JavaScript Library

question.answer
        ↓
JavaScript Library
```

Match:

```text
true
```

Therefore:

```text
score = 1
```

---

## STEP 3

Move to next:

```text
currentQuestion = 1
```

UI:

```text
Question 2 of 3
```

---

## STEP 4

Suppose user gives wrong answer.

Then:

```text
score remains 1
```

Move to Question 3.

---

## STEP 5

Suppose Question 3 is correct.

```text
score = 2
```

Because it is the final question:

```text
quizFinished = true
```

---

# 🎉 33. RESULT

Now:

```text
🎉 Quiz Complete!

Score: 2 / 3

[ 🔄 Restart ]
```

---

# 📊 34. PROGRESS BAR

We can also display progress.

Example:

```text
Question 2 of 5
```

Calculate percentage:

```javascript
const progress =
    ((currentQuestion + 1) /
        questions.length) * 100;
```

For Question 2 of 5:

```text
(2 / 5) × 100
= 40%
```

Then:

```jsx
<div>
    <div
        style={{
            width: `${progress}%`
        }}
    >
    </div>
</div>
```

---

# 🧠 35. WHY IS PROGRESS USEFUL?

The user knows:

```text
How many questions are completed?
How many are remaining?
```

Instead of:

```text
Question 2
```

we can show:

```text
Question 2 of 10
```

Much better UX.

---

# 🏆 36. SCORE PERCENTAGE

We can calculate:

```javascript
const percentage =
    (score / questions.length) * 100;
```

Suppose:

```text
score = 8
questions = 10
```

Then:

```text
8 / 10 × 100
= 80%
```

Display:

```text
🎯 80%
```

---

# ⭐ 37. RESULT MESSAGE

We can make the result dynamic:

```javascript
function getResultMessage(
    percentage
) {

    if (percentage >= 80) {
        return "🏆 Excellent!";
    }

    if (percentage >= 60) {
        return "👍 Good Job!";
    }

    if (percentage >= 40) {
        return "🙂 Keep Practicing!";
    }

    return "📚 Study More!";
}
```

Then:

```jsx
<p>
    {getResultMessage(percentage)}
</p>
```

---

# 🧠 38. COMPONENT ARCHITECTURE

Our final application:

```text
App
│
├── Question
│     │
│     └── Answer Buttons
│
└── Result
      │
      └── Restart
```

We could later add:

```text
App
│
├── QuizHeader
├── Progress
├── Question
├── AnswerOptions
├── NextButton
└── Result
```

---

# 🌐 39. FUTURE API VERSION

The current version uses local questions:

```text
questions.js
```

Later we can replace that with:

```text
React
 ↓
Quiz API
 ↓
Questions
 ↓
State
 ↓
Quiz UI
```

Then we can add:

```text
⏳ Loading
❌ Error
🔄 Retry
```

This connects directly to the API patterns you learned in the Weather and Movie projects.

---

# 🚨 40. COMMON MISTAKES

## Mistake 1 — Forgetting array indexing

Wrong:

```javascript
questions.question
```

Correct:

```javascript
questions[currentQuestion].question
```

because `questions` is an array.

---

## Mistake 2 — Going beyond the last question

Bad:

```javascript
setCurrentQuestion(
    currentQuestion + 1
);
```

without checking the last index.

Better:

```javascript
if (
    currentQuestion <
    questions.length - 1
) {
    setCurrentQuestion(
        previous => previous + 1
    );
}
```

---

## Mistake 3 — Updating score incorrectly

Avoid relying on a stale value when calculating from previous state.

Prefer:

```javascript
setScore(
    previousScore =>
        previousScore + 1
);
```

---

## Mistake 4 — Allowing Next without an answer

Bad:

```text
User doesn't select
        ↓
Next
        ↓
Question changes
```

Better:

```jsx
disabled={
    selectedAnswer === ""
}
```

---

## Mistake 5 — Not resetting all state

Bad restart:

```javascript
setCurrentQuestion(0);
```

The old score and answer may remain.

Better:

```javascript
setCurrentQuestion(0);
setSelectedAnswer("");
setScore(0);
setQuizFinished(false);
```

---

# 🧠 41. BEST PRACTICES

### 1. Keep question data separate

```text
data/
   questions.js
```

### 2. Keep UI components small

```text
Question
Result
Progress
```

### 3. Keep the source of truth in the parent

```text
App
 ↓
quiz state
```

### 4. Don't duplicate state

Don't store:

```text
currentQuestion
+
currentQuestionNumber
```

if one can be calculated from the other.

### 5. Use functional state updates when appropriate

```javascript
setScore(
    previous => previous + 1
);
```

### 6. Handle the final question explicitly

Always know when:

```text
currentQuestion ===
questions.length - 1
```

---

# 💼 42. INTERVIEW QUESTIONS

### Q1. Why is `currentQuestion` initialized to `0`?

**Answer:**  
Because JavaScript arrays use zero-based indexing, so index `0` represents the first question.

---

### Q2. Why use `map()` for answer options?

**Answer:**  
Because the options are stored in an array and we need to render each option.

---

### Q3. How does the child component communicate the selected answer to the parent?

**Answer:**  
The parent passes a callback function as a prop, and the child calls that function when an answer is selected.

---

### Q4. Why use functional state updates for the score?

**Answer:**  
Because the new score depends on the previous score.

---

### Q5. How do you determine whether the quiz is finished?

**Answer:**  
When the current question is the last question, we set the quiz-finished state to `true`.

---

### Q6. Why use conditional rendering for the Result component?

**Answer:**  
Because the question UI and result UI represent different application states.

---

### Q7. What happens when `selectedAnswer === ""`?

**Answer:**  
The user hasn't selected an answer, so the Next button can remain disabled.

---

# 🧪 43. PRACTICE QUESTIONS

### Practice 1

Add a fourth question.

### Practice 2

Show:

```text
Correct ✅
```

after selecting the right answer.

### Practice 3

Show:

```text
Wrong ❌
```

for an incorrect answer.

### Practice 4

Add a progress bar.

### Practice 5

Calculate the percentage.

### Practice 6

Display:

```text
🏆 Excellent
👍 Good
🙂 Average
📚 Keep Practicing
```

depending on the score.

### Practice 7

Add a Restart button.

### Practice 8

Add a question counter.

---

# 🧩 44. MINI CHALLENGE

Build your own **5-question React Quiz App**.

Requirements:

```text
☐ 5 questions
☐ 4 options per question
☐ Select answer
☐ Next button
☐ Question counter
☐ Score calculation
☐ Final result
☐ Restart button
☐ Disable Next until answer selected
```

Bonus:

```text
☐ Progress bar
☐ Correct/wrong indication
☐ Percentage
☐ Result message
☐ Timer
☐ High score
```

---

# 📝 45. 10-QUESTION QUIZ

### Q1. What does `currentQuestion = 0` represent?

**Answer:** The first question.

---

### Q2. Which method renders all answer options?

**Answer:** `map()`.

---

### Q3. What stores the user's selected answer?

**Answer:** `selectedAnswer`.

---

### Q4. What stores the number of correct answers?

**Answer:** `score`.

---

### Q5. What tells us whether to display the result screen?

**Answer:** `quizFinished`.

---

### Q6. How do we move to the next question?

**Answer:**

```javascript
setCurrentQuestion(
    previous => previous + 1
);
```

---

### Q7. Why should Next be disabled initially?

**Answer:** To prevent moving forward without selecting an answer.

---

### Q8. How do we calculate the score percentage?

**Answer:**

```javascript
(score / questions.length) * 100
```

---

### Q9. How do we restart the quiz?

**Answer:** Reset the question index, selected answer, score, and finished state.

---

### Q10. What React concepts are combined in this project?

**Answer:**

```text
useState
Props
Callbacks
Events
Arrays
map()
Conditional Rendering
State Updates
Component Communication
```

---

# 🧠 FINAL MEMORY TRICK

Remember the Quiz App as:

```text
🧠 QUESTION
     ↓
🔘 SELECT ANSWER
     ↓
➡️ NEXT
     ↓
✅ CHECK ANSWER
     ↓
🏆 UPDATE SCORE
     ↓
➡️ NEXT QUESTION
     ↓
...
     ↓
🎉 RESULT
     ↓
🔄 RESTART
```

And remember the four main states:

```text
currentQuestion
       ↓
WHERE AM I?

selectedAnswer
       ↓
WHAT DID USER SELECT?

score
       ↓
HOW MANY ARE CORRECT?

quizFinished
       ↓
IS THE QUIZ OVER?
```

---

# 📌 CHAPTER 53 — PART 4 COMPLETE

We have now completed:

```text
🌤️ Weather App
🎬 Movie Search App
💰 Expense Tracker
🧠 Quiz App
```

Your source syllabus has **two more Chapter 53 projects**:

```text
🍳 Recipe App
🛍️ Product Listing App
```

and Chapter 53 is specifically intended to practice API, search, filtering, sorting, pagination, loading, and error handling.

# 🚀 NEXT

## CHAPTER 53 — PART 5
### 🍳 RECIPE APP

We will build:

```text
🍳 Recipe Search
🔍 Search
🏷️ Categories
🥗 Ingredients
📋 Recipe Cards
📄 Recipe Details
⏳ Loading
❌ Error Handling
🌐 API Integration
```

Type **NEXT** to continue.