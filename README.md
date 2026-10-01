# Quiz CLI

## Project Overview

Quiz CLI is an interactive command-line quiz game for testing and reinforcing programming knowledge. It is implemented with modern Node.js ES modules and uses only built-in Node.js APIs, so no third-party runtime dependencies are required.

The application loads categorized questions from `data/questions.json`, lets the player choose a category and quiz length, presents questions through an interactive terminal menu, gives immediate feedback and explanations, and displays a final score with performance guidance and incorrect-answer review.

### Core capabilities

- Categories for JavaScript Basics, Node.js Fundamentals, and General Programming.
- Configurable quiz length: all available questions, or 3/5 questions when supported by the selected category.
- Fisher–Yates shuffling so each quiz uses a randomized question order.
- Interactive numeric selection and yes/no prompts using Node's built-in `readline` module.
- Progress bar, score percentage, performance message, and answer review.
- ANSI terminal colors without external packages.
- Error handling around application startup and question-file loading.
- Replay support after each completed quiz.

## Requirements

- Node.js 18 or newer.
- A terminal that supports standard ANSI color escape codes.

## Setup Instructions

Clone or download the repository, then change into the project directory:

```bash
git clone <repository-url>
cd test-app
```

There are no external dependencies to install. If you want npm to create a local lockfile or verify the package manifest, run:

```bash
npm install
```

The application uses ES modules because `package.json` sets `"type": "module"`.

## Running the Application

Start the quiz with:

```bash
npm start
```

Alternatively, invoke Node directly:

```bash
node index.js
```

Use the prompts to select a category, choose the number of questions, answer each question by entering its displayed number, and decide whether to play again.

## Testing

The package defines the following test command:

```bash
npm test
```

It invokes Node's built-in test runner with `node --test`. No test files are currently included in the repository, so this command is primarily the project’s configured test entry point.

## Usage Example

A typical session follows this flow:

```text
Choose a category:
  1. JavaScript Basics
  2. Node.js Fundamentals
  3. General Programming

Your choice (enter number): 1

How many questions?
  1. All questions
  2. 3 questions
  3. 5 questions

Your choice (enter number): 2
```

The player then answers each question using its numeric option. The application reports whether the answer is correct, shows an explanation when one is available, and finishes with a score such as `4/5 (80%)`. Incorrect answers are listed for review before the replay prompt.

## Data Format

Questions are stored in `data/questions.json`. The top-level `categories` object maps category IDs to category records:

```json
{
  "categories": {
    "category-id": {
      "name": "Category name",
      "questions": [
        {
          "question": "Question text",
          "options": ["Option A", "Option B"],
          "answer": 0,
          "explanation": "Why the answer is correct."
        }
      ]
    }
  }
}
```

The `answer` field is a zero-based index into `options`. To add or modify quiz content, update the JSON while preserving this structure. The menu automatically discovers category IDs and adjusts the available question-count choices based on each category’s size.

## File Structure

```text
.
├── index.js              # Application entry point and main game loop
├── package.json           # Project metadata, scripts, and Node.js requirement
├── data/
│   └── questions.json     # Categories, questions, answers, and explanations
└── src/
    ├── colors.js          # ANSI color and text-style utilities
    ├── input.js           # Readline interface and terminal prompts
    └── quiz.js            # Quiz class, shuffling, scoring, and result display
```

## Technical Details

### Application flow

1. `index.js` creates a `readline` interface and loads the JSON question bank asynchronously with `node:fs/promises`.
2. Category IDs are read dynamically from `data.categories` and displayed through `select()`.
3. The selected category determines the available question-count options.
4. A `Quiz` instance copies and shuffles the selected questions using Fisher–Yates.
5. `Quiz.askQuestion()` renders progress, collects an answer, records the result, and displays feedback and explanations.
6. `Quiz.showResults()` calculates the percentage, prints a performance message, and lists incorrect answers for review.
7. `confirm()` determines whether the player starts another round.

### Module design

- **`index.js`** coordinates file loading, menus, application lifecycle, and error handling.
- **`src/input.js`** wraps callback-based `readline.question()` in Promises and exposes reusable selection, confirmation, and pause helpers.
- **`src/quiz.js`** contains the game state: shuffled questions, current index, score, answer history, progress, and result presentation.
- **`src/colors.js`** provides ANSI escape-code helpers such as `success()`, `error()`, `info()`, and `highlight()` without adding a dependency.
- **`data/questions.json`** separates educational content from application logic.

The code demonstrates ES module imports/exports, async/await, Promises, classes, getters, destructuring, template literals, array methods, filesystem access, and structured error handling.

## Configuration and Customization

- Add categories under `categories` in `data/questions.json`.
- Add questions with a question string, an options array, a zero-based correct-answer index, and an optional explanation.
- Adjust terminal styling in `src/colors.js` by adding ANSI codes or convenience functions.
- Change quiz behavior in `src/quiz.js`, including the progress-bar width, scoring messages, or review output.
- Change menu and replay behavior in `index.js` and prompt validation in `src/input.js`.

## License

This project is licensed under the MIT License, as declared in `package.json`.
