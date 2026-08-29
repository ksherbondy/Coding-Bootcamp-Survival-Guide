# 02. JavaScript Execution and Async Mental Models

## What JavaScript is actually doing when your code runs

Students often learn syntax before they develop a model of execution. That creates confusion around:

```text
callbacks
promises
async/await
timers
fetch
scope
closures
the call stack
the event loop
```

The goal is not to reproduce an engine textbook. The goal is to predict behavior.

---

# 1. JavaScript Executes Instructions in an Order

```js
console.log("A");
console.log("B");
console.log("C");
```

prints:

```text
A
B
C
```

Functions temporarily redirect execution:

```js
function greet() {
  console.log("hello");
}

console.log("A");
greet();
console.log("B");
```

Conceptually:

```text
global code
  ↓
greet()
  ↓
return
  ↓
global code continues
```

---

# 2. The Call Stack Tracks Active Function Calls

Example:

```js
function one() {
  two();
}

function two() {
  three();
}

function three() {
  console.log("done");
}

one();
```

At the deepest point:

```text
three
two
one
global
```

When `three()` returns, that call is removed. Then `two()`, then `one()`.

Recursion uses the same machinery.

---

# 3. Scope Answers "Where Can This Name Be Used?"

```js
function test() {
  const value = 10;
  console.log(value);
}
```

`value` is available inside that lexical scope.

Scope is a language visibility rule. It is not simply a statement about physical RAM.

---

# 4. Closures Preserve Access to Outer Bindings

```js
function makeCounter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}
```

The returned function still has access to `count`.

Mental model:

> A function remembers the lexical environment in which it was created.

Closures appear in callbacks, events, factories, hooks, and private-state patterns.

---

# 5. Why Asynchronous Programming Exists

Some operations take time:

```text
network requests
file reads
database queries
timers
user input
```

If the main JavaScript execution path had to block for all of them, applications would become unresponsive.

---

# 6. `setTimeout()` Does Not Make JavaScript Sleep

```js
console.log("A");

setTimeout(() => {
  console.log("B");
}, 1000);

console.log("C");
```

Output:

```text
A
C
B
```

Conceptually:

```text
JavaScript:
run A

environment:
start timer

JavaScript:
run C

later:
timer finishes
callback becomes ready
event loop schedules it
```

---

# 7. The Event Loop Coordinates Ready Work

Simplified:

```text
CALL STACK
    ↑
    |
EVENT LOOP
    |
    ↓
READY TASKS
```

The exact implementation is more detailed, but this model is enough for most bootcamp reasoning.

---

# 8. Callbacks Are Functions Given to Something Else

```js
button.addEventListener("click", handleClick);
```

You are giving the browser a function to call later.

Likewise:

```js
array.map(transform);
```

gives `map()` a function to call for each element.

Callbacks are not automatically asynchronous. They are simply functions passed to other code.

---

# 9. Promises Represent Future Completion

A promise represents work that may complete later.

States:

```text
pending
fulfilled
rejected
```

`fetch()` returns a promise immediately while the network work continues.

---

# 10. `async/await` Makes Async Code Read Sequentially

```js
const response = await fetch("/api/users");
```

Useful interpretation:

```text
start asynchronous work
pause this async function's continuation
allow other work to happen
resume when promise settles
```

`await` does not freeze the entire runtime.

---

# 11. "One Function Call" Can Hide Iteration

```js
const doubled = nums.map(x => x * 2);
```

still performs iteration.

Conceptually:

```js
const doubled = [];

for (let i = 0; i < nums.length; i++) {
  doubled.push(nums[i] * 2);
}
```

Abstraction hides mechanics; it does not erase them.

---

# 12. Function Calls Are Control Flow

Read:

```js
const result = calculateTotal(cart);
```

as:

```text
pause here
enter calculateTotal
run instructions
produce result
return here
continue
```

This mental expansion helps when reading unfamiliar code.

---

# 13. Recursive Calls Use the Same Stack

```js
function countdown(n) {
  if (n === 0) return;
  countdown(n - 1);
}
```

At depth:

```text
countdown(1)
countdown(2)
countdown(3)
global
```

Then calls unwind.

---

# 14. Common Async Mistakes

## Forgetting `await`

```js
const response = fetch(url);
```

`response` is a promise.

## Assuming `forEach()` Coordinates Async Work

```js
items.forEach(async item => {
  await save(item);
});
```

`forEach()` does not await the callback promises as a group.

Sequential:

```js
for (const item of items) {
  await save(item);
}
```

Concurrent:

```js
await Promise.all(items.map(save));
```

Which is correct depends on the problem.

---

# Execution-Tracing Checklist

```text
What runs synchronously now?
What function am I inside?
What is on the call stack?
Did I call this function or pass it?
Does this operation finish immediately?
If not, who performs the waiting/work?
What promise represents completion?
When does this function resume?
```

---

# Practice Trace

Predict before running:

```js
console.log("start");

async function work() {
  console.log("inside 1");
  await Promise.resolve();
  console.log("inside 2");
}

work();

console.log("end");
```

Then explain every line using:

```text
call stack
async function
promise
continuation
```
