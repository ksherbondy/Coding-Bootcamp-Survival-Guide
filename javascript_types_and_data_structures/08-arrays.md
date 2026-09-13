# JavaScript Arrays

> Arrays are ordered, zero-indexed objects designed for collections of values.

## Learning Objectives

You should be able to:

- create arrays
- access and update elements
- understand zero-based indexing and `length`
- explain mutating vs non-mutating methods
- iterate arrays
- use higher-order array methods
- work with nested arrays
- understand shallow copying
- choose arrays when order matters

---

# 1. Creating Arrays

```js
const numbers = [10, 20, 30];
const names = ["Ada", "Grace", "Linus"];
const mixed = [1, "hello", true, null];
```

Arrays can hold any JavaScript values.

---

# 2. Arrays Are Zero-Indexed

```js
const names = ["Ada", "Grace", "Linus"];

names[0]; // "Ada"
names[1]; // "Grace"
names[2]; // "Linus"
```

The first element is index `0`.

---

# 3. `length`

```js
names.length;
```

returns:

```text
3
```

Last element:

```js
names[names.length - 1];
```

Modern syntax:

```js
names.at(-1);
```

---

# 4. Updating an Element

```js
names[1] = "Margaret";
```

Arrays are mutable.

---

# 5. Arrays Are Objects

```js
typeof [];
```

returns:

```text
"object"
```

So use:

```js
Array.isArray(value);
```

to test specifically for arrays.

---

# 6. Add and Remove at the End

```js
array.push(value);
array.pop();
```

Stack-style behavior:

```text
push → add to end
pop  → remove from end
```

---

# 7. Add and Remove at the Beginning

```js
array.unshift(value);
array.shift();
```

These can require shifting existing indexes, so they are often more expensive than end operations on large arrays.

---

# 8. `slice()` vs `splice()`

`slice()`:

```js
array.slice(start, end);
```

returns a new array and does not mutate the original.

`splice()`:

```js
array.splice(start, deleteCount, ...items);
```

mutates the original array.

This distinction matters.

---

# 9. Iteration

Classic loop:

```js
for (let i = 0; i < array.length; i++) {
  console.log(array[i]);
}
```

`for...of`:

```js
for (const value of array) {
  console.log(value);
}
```

`forEach()`:

```js
array.forEach(value => {
  console.log(value);
});
```

---

# 10. `map()`

Transforms every element.

```js
const doubled = [1, 2, 3].map(value => value * 2);
```

Result:

```js
[2, 4, 6]
```

Mental model:

```text
one input element
→ one output element
```

---

# 11. `filter()`

Keeps elements that pass a test.

```js
const evens = [1, 2, 3, 4].filter(value => value % 2 === 0);
```

Result:

```js
[2, 4]
```

---

# 12. `reduce()`

Combines many values into one accumulated result.

```js
const total = [1, 2, 3, 4].reduce((acc, value) => {
  return acc + value;
}, 0);
```

Result:

```text
10
```

---

# 13. Search Methods

```js
array.includes(value);
array.indexOf(value);

array.find(callback);
array.findIndex(callback);

array.some(callback);
array.every(callback);
```

Think:

```text
includes → is this exact value present?
find     → give me first matching element
some     → does any element pass?
every    → do all elements pass?
```

---

# 14. Sorting

```js
array.sort();
```

mutates the array.

Default sorting is string-based.

Therefore:

```js
[10, 2, 30].sort();
```

does not perform ordinary numeric sorting.

Use:

```js
numbers.sort((a, b) => a - b);
```

ascending.

Descending:

```js
numbers.sort((a, b) => b - a);
```

---

# 15. Nested Arrays

```js
const matrix = [
  [1, 2],
  [3, 4]
];
```

Access:

```js
matrix[1][0];
```

returns:

```text
3
```

For arbitrary 2D traversal:

```js
for (const row of matrix) {
  for (const value of row) {
    console.log(value);
  }
}
```

---

# 16. Destructuring Fixed Pairs

If each inner array is a known pair:

```js
const edges = [
  ["A", "B"],
  ["B", "C"]
];
```

you can write:

```js
for (const [from, to] of edges) {
  console.log(from, to);
}
```

That is different from generic nested traversal.

---

# 17. Spread

Copy an array:

```js
const copy = [...array];
```

Combine:

```js
const combined = [...a, ...b];
```

Remember: spread creates a shallow copy.

---

# 18. Destructuring

```js
const [first, second] = array;
```

Rest:

```js
const [first, ...remaining] = array;
```

---

# 19. Flattening

```js
array.flat();
```

Example:

```js
[[1, 2], [3, 4]].flat();
```

returns:

```js
[1, 2, 3, 4]
```

---

# 20. Array Method Reference

Common instance methods include:

```text
at()
concat()
copyWithin()
entries()
every()
fill()
filter()
find()
findIndex()
findLast()
findLastIndex()
flat()
flatMap()
forEach()
includes()
indexOf()
join()
keys()
lastIndexOf()
map()
pop()
push()
reduce()
reduceRight()
reverse()
shift()
slice()
some()
sort()
splice()
toLocaleString()
toReversed()
toSorted()
toSpliced()
toString()
unshift()
values()
with()
```

Common static methods:

```text
Array.from()
Array.fromAsync()
Array.isArray()
Array.of()
```

Runtime support for the newest methods depends on JavaScript version.

---

# 21. Mutating vs Non-Mutating

Common mutating methods:

```text
push
pop
shift
unshift
splice
sort
reverse
fill
copyWithin
```

Common methods returning new arrays:

```text
map
filter
slice
concat
flat
flatMap
toSorted
toReversed
toSpliced
with
```

Knowing whether a method mutates is crucial.

---

# 22. Arrays vs Objects

Use an array when:

```text
order matters
indexes matter
you have a sequence/list
```

Use an object when:

```text
named properties matter
you are modeling a record/entity
```

---

# Practice

1. What index contains the first element?
2. What is the difference between `slice()` and `splice()`?
3. Which method transforms each element?
4. Which method removes items that fail a test?
5. Why does numeric `.sort()` usually need a comparator?
6. When is `[a, b]` destructuring useful in a 2D array?
