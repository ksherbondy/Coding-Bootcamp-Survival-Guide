# JavaScript Functions

> Functions are reusable blocks of behavior, first-class values, and callable objects.

## Learning Objectives

You should be able to:

- declare functions in several forms
- distinguish parameters and arguments
- return values
- use callbacks and higher-order functions
- explain closures
- understand arrow functions and `this`
- use default and rest parameters
- understand `call`, `apply`, and `bind`
- recognize functions as objects

---

# 1. Function Declaration

```js
function add(a, b) {
  return a + b;
}
```

Call:

```js
add(2, 3);
```

---

# 2. Parameters vs Arguments

```js
function add(a, b) {
```

`a` and `b` are parameters.

```js
add(2, 3);
```

`2` and `3` are arguments.

---

# 3. Return Values

```js
function square(n) {
  return n * n;
}
```

`return`:

```text
produces the function's result
and exits the function
```

If no value is returned, the function result is:

```js
undefined
```

---

# 4. Return and Automatic Semicolon Insertion

Keep the returned expression on the same line:

```js
return {
  name: "Max"
};
```

Do not write:

```js
return
{
  name: "Max"
};
```

JavaScript may treat the line break as:

```js
return;
```

and return `undefined`.

---

# 5. Function Expression

```js
const add = function(a, b) {
  return a + b;
};
```

Here the function is a value assigned to a variable.

---

# 6. Arrow Function

```js
const add = (a, b) => {
  return a + b;
};
```

Concise form:

```js
const add = (a, b) => a + b;
```

---

# 7. Arrow Functions Are Not Just Shorter Syntax

Arrow functions do not create their own:

```text
this
arguments
super
new.target
```

They also cannot be used as constructors with `new`.

So choose arrows for their semantics, not only because they are shorter.

---

# 8. Default Parameters

```js
function greet(name = "Guest") {
  return `Hello ${name}`;
}
```

---

# 9. Rest Parameters

```js
function sum(...numbers) {
  return numbers.reduce((total, n) => total + n, 0);
}
```

`numbers` becomes an array.

---

# 10. Functions Are Values

You can store a function:

```js
const operation = add;
```

Pass it:

```js
array.map(transform);
```

Return it:

```js
function makeMultiplier(x) {
  return function(y) {
    return x * y;
  };
}
```

---

# 11. Callback Functions

A callback is a function passed to another function.

```js
const numbers = [1, 2, 3];

numbers.map(function(value) {
  return value * 2;
});
```

The callback is:

```js
function(value) {
  return value * 2;
}
```

---

# 12. Higher-Order Functions

A higher-order function:

```text
accepts a function
or
returns a function
```

Examples:

```text
map
filter
reduce
setTimeout
event listeners
```

---

# 13. Closures

A closure occurs when a function retains access to variables from its surrounding lexical scope.

```js
function makeCounter() {
  let count = 0;

  return function() {
    count++;
    return count;
  };
}
```

Usage:

```js
const counter = makeCounter();

counter(); // 1
counter(); // 2
counter(); // 3
```

The inner function continues to access `count` after `makeCounter()` has returned.

---

# 14. `this`

For an ordinary method:

```js
const dog = {
  name: "Snoopy",

  speak() {
    return this.name;
  }
};
```

called as:

```js
dog.speak();
```

`this` refers to `dog`.

Arrow functions do not get their own method-style `this`.

---

# 15. `call()`

Call a function with an explicit `this`:

```js
function greet() {
  return `Hello ${this.name}`;
}

greet.call({ name: "Ada" });
```

---

# 16. `apply()`

Similar to `call()`, but arguments are supplied as an array-like collection:

```js
function add(a, b) {
  return a + b;
}

add.apply(null, [2, 3]);
```

---

# 17. `bind()`

Creates a new function with `this` bound:

```js
const greetAda = greet.bind({
  name: "Ada"
});

greetAda();
```

---

# 18. Functions Are Objects

Functions can have properties:

```js
function greet() {
}

greet.language = "English";
```

Therefore:

```js
typeof greet;
```

returns:

```text
"function"
```

but functions still have object-like capabilities.

---

# 19. Constructor Functions

```js
function Dog(name) {
  this.name = name;
}
```

When called with:

```js
new Dog("Snoopy");
```

it participates in JavaScript's prototype-based object construction.

Arrow functions cannot be constructor functions.

---

# 20. Recursion

A function can call itself:

```js
function countdown(n) {
  if (n === 0) return;

  console.log(n);
  countdown(n - 1);
}
```

A recursive function needs:

```text
base case
progress toward the base case
recursive call
```

---

# 21. IIFE

Immediately Invoked Function Expression:

```js
(function() {
  console.log("runs immediately");
})();
```

Historically useful for creating private scopes before modern modules and block-scoped declarations became common.

---

# 22. Function Method Reference

Functions inherit useful methods including:

```text
call()
apply()
bind()
toString()
```

Function objects also expose properties such as:

```text
name
length
prototype   // for constructable ordinary functions
```

---

# Practice

1. What is the difference between a parameter and an argument?
2. What happens when no value is returned?
3. What makes a function higher-order?
4. What is a closure?
5. Why is an arrow function different from a normal function?
6. What do `call`, `apply`, and `bind` affect?
