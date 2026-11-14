# Expense Analyser

A Node.js expense analysis script that reads and processes tabular expense records (`expenses.txt`) demonstrating synchronous and asynchronous file I/O, higher-order array functions, closures, switch statements, and ternary operators.

---

## Deliverables & Analysis Questions

### 1. What did you predict for Warmup.js, and what did it print? Explain the first line and the error from Part B.
The predicted output for `Warmup.js` was `undefined`, `99 1`, `3`, and `true false`, which matches the actual console output. The first line prints `undefined` because variables declared with `var` are hoisted to the top of their scope and initialized to `undefined` prior to execution. In contrast, Part B produces `ReferenceError: Cannot access 'b' before initialization` because `let` variables reside in a Temporal Dead Zone (TDZ) and cannot be accessed before their declaration statement is evaluated.

### 2. Why did Loop finished print before Async read finished? Who did the reading while your loop was running?
`Loop finished` printed before `Async read finished` because `fs.readFile` is non-blocking and delegates the file reading to the libuv thread pool, freeing the main thread to immediately execute the synchronous 1,000,000-iteration loop. While the main JavaScript thread ran the loop, a background worker thread from libuv and the underlying OS filesystem handled reading `expenses.txt`. The completion callback was queued in the event loop and could only run after the synchronous call stack cleared.

### 3. How does tracker remember spent after createBudgetTracker has returned?
The `spend` function forms a closure over the lexical environment of `createBudgetTracker`, retaining a reference to the `spent` variable in heap memory even after the outer function has returned. Because this reference persists, every invocation of `tracker` reads and updates the same enclosed state. This allows the budget tracker to accumulate expenses across successive function calls without relying on global variables.

### 4. What does the ^ in front of the lodash version in package.json mean, and why is package-lock.json created?
The caret (`^`) symbol is a SemVer range operator indicating that npm may install minor and patch updates (e.g., any version `>= 4.18.1` and `< 5.0.0`) that do not introduce breaking changes. `package-lock.json` is automatically generated to record the exact resolved versions, dependency trees, and cryptographic hashes of all installed packages. This ensures consistent, reproducible installations across different machines and environments.

---

## Project Structure

```text
expense_analyser/
├── analyser.js        # Core expense analysis script
├── Warmup.js          # JavaScript scope, closure, and coercion warm-up
├── expenses.txt       # Raw expense records (category,amount)
├── output.txt         # Console output from analyser.js
├── package.json       # Project dependencies and metadata
├── package-lock.json  # Locked dependency versions
└── README.md          # Project documentation and conceptual answers
```

## How to Run

1. **Install dependencies:**
   ```bash
   npm install
   ```

2. **Run the warm-up script:**
   ```bash
   node Warmup.js
   ```

3. **Run the expense analyser:**
   ```bash
   node analyser.js
   ```
