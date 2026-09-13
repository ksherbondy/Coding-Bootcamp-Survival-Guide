# JavaScript Strings

> Strings represent text, but they can also be traversed and indexed in ways that feel similar to arrays.

## Learning Objectives

By the end of this lesson, you should be able to:

- create strings
- use indexes to access characters
- explain why strings are array-like but are not arrays
- explain string immutability
- use common string methods
- slice, search, replace, split, and compare strings
- understand basic Unicode concerns
- convert between strings and arrays when useful

---

# 1. Creating Strings

```js
const first = "James";
const second = 'James';
const third = `James`;
```

All three are strings.

```js
typeof first; // "string"
```

Template literals use backticks and support interpolation:

```js
const name = "James";

const message = `Hello, ${name}`;
```

---

# 2. Strings Are Indexed

A string is not an array, but JavaScript lets you access characters using numeric indexes.

```js
const myName = "James";

console.log(myName[0]); // "J"
console.log(myName[1]); // "a"
console.log(myName[2]); // "m"
```

Indexes begin at:

```text
0
```

So if you want the **second letter**:

```js
myName[1];
```

returns:

```text
"a"
```

This is an important connection between strings and arrays.

---

# 3. Strings Have a `length`

```js
const myName = "James";

console.log(myName.length);
```

returns:

```text
5
```

That means you can traverse a string with a loop:

```js
for (let i = 0; i < myName.length; i++) {
  console.log(myName[i]);
}
```

This looks very similar to traversing an array.

---

# 4. Strings Are Array-Like, but Not Arrays

Both support:

```text
numeric indexing
length
iteration
```

Example:

```js
for (const char of "James") {
  console.log(char);
}
```

But:

```js
Array.isArray("James");
```

returns:

```js
false
```

And strings do not automatically have array methods such as:

```js
map()
filter()
push()
pop()
```

---

# 5. Strings Are Immutable

You can read:

```js
const name = "James";

console.log(name[1]);
```

but you cannot change that character in place:

```js
name[1] = "o";
```

The string does not become:

```text
Jomes
```

Strings are immutable.

Operations that appear to modify a string actually produce a new string.

---

# 6. Build a New String Instead

```js
let name = "James";

name = "J" + "o" + name.slice(2);
```

Now:

```text
Jomes
```

The original string value was not mutated. A new string was created and the variable was reassigned.

---

# 7. Character Access

Two common approaches:

```js
name[1];
name.charAt(1);
```

Bracket notation is usually simpler:

```js
name[1];
```

---

# 8. Last Character

Because indexes begin at `0`:

```js
const word = "hello";

word[word.length - 1];
```

returns:

```text
"o"
```

Modern JavaScript also provides:

```js
word.at(-1);
```

which returns:

```text
"o"
```

`at()` supports negative indexes.

---

# 9. `slice()`

Extract part of a string:

```js
const word = "JavaScript";

word.slice(0, 4);
```

returns:

```text
"Java"
```

The start is included.

The end is excluded.

```text
slice(start, end)
```

Negative values count from the end:

```js
word.slice(-6);
```

returns:

```text
"Script"
```

---

# 10. `substring()`

```js
word.substring(0, 4);
```

also returns:

```text
"Java"
```

`slice()` is often preferred because its negative-index behavior is useful and consistent.

---

# 11. Changing Case

```js
"hello".toUpperCase();
```

returns:

```text
"HELLO"
```

```js
"HELLO".toLowerCase();
```

returns:

```text
"hello"
```

Remember: a new string is returned.

---

# 12. Searching Strings

```js
const sentence = "JavaScript is fun";

sentence.includes("Script");   // true
sentence.startsWith("Java");   // true
sentence.endsWith("fun");      // true
```

Find an index:

```js
sentence.indexOf("Script");
```

If not found:

```js
sentence.indexOf("Python");
```

returns:

```text
-1
```

---

# 13. `replace()` and `replaceAll()`

```js
"cat dog cat".replace("cat", "fox");
```

returns:

```text
"fox dog cat"
```

Only the first matching string is replaced.

```js
"cat dog cat".replaceAll("cat", "fox");
```

returns:

```text
"fox dog fox"
```

---

# 14. `split()` Turns a String Into an Array

```js
const sentence = "one two three";

const words = sentence.split(" ");
```

Result:

```js
["one", "two", "three"]
```

This is one of the most useful bridges between strings and arrays.

---

# 15. Split Into Characters

```js
"James".split("");
```

produces:

```js
["J", "a", "m", "e", "s"]
```

Once it is an array, array methods become available:

```js
"James"
  .split("")
  .map(char => char.toUpperCase());
```

---

# 16. Joining Back Into a String

Arrays have:

```js
join()
```

So:

```js
["J", "a", "m", "e", "s"].join("");
```

returns:

```text
"James"
```

A common pattern is:

```text
string
↓ split
array
↓ array operations
array
↓ join
string
```

---

# 17. Strings Are Iterable

You can use:

```js
for...of
```

directly:

```js
for (const char of "James") {
  console.log(char);
}
```

You can also use spread:

```js
[..."James"];
```

Result:

```js
["J", "a", "m", "e", "s"]
```

For Unicode text, spread is often better than `split("")` for iterating code points.

---

# 18. Trimming Whitespace

```js
"   hello   ".trim();
```

returns:

```text
"hello"
```

Also:

```js
trimStart();
trimEnd();
```

---

# 19. Padding

```js
"7".padStart(3, "0");
```

returns:

```text
"007"
```

```js
"7".padEnd(3, "0");
```

returns:

```text
"700"
```

Useful for:

```text
binary formatting
IDs
display formatting
fixed-width output
```

---

# 20. Repeating

```js
"ha".repeat(3);
```

returns:

```text
"hahaha"
```

---

# 21. Comparing Strings

```js
"cat" === "cat";
```

returns:

```js
true
```

Strings are primitives, so equal string values compare by value.

Case matters:

```js
"Cat" === "cat";
```

returns:

```js
false
```

---

# 22. Converting to a String

```js
String(123);       // "123"
String(true);      // "true"
String(null);      // "null"
```

You can also use:

```js
(123).toString();
```

but `String(value)` handles more cases safely.

---

# 23. Strings and Numbers

```js
"5" + 2;
```

returns:

```text
"52"
```

because `+` also performs string concatenation.

But:

```js
"5" - 2;
```

returns:

```text
3
```

because subtraction forces numeric conversion.

This is why explicit conversion is often clearer:

```js
Number("5") + 2;
```

---

# 24. Character Codes

JavaScript strings use UTF-16 code units internally.

You can inspect one:

```js
"A".charCodeAt(0);
```

returns:

```text
65
```

Create a character:

```js
String.fromCharCode(65);
```

returns:

```text
"A"
```

For full Unicode code points, use:

```js
codePointAt()
String.fromCodePoint()
```

---

# 25. Unicode Is More Complicated Than "One Character = One Index"

For many common English characters:

```js
"James"[1]
```

works exactly as expected.

But some Unicode characters can occupy more than one UTF-16 code unit.

Therefore:

```js
str.length
```

does not always mean:

```text
number of human-perceived characters
```

This matters with:

```text
emoji
some symbols
combined characters
international text
```

For beginner exercises using ordinary ASCII text, indexing works intuitively. For production Unicode handling, remember the model is more complicated.

---

# 26. Common String Methods

```text
at()
charAt()
charCodeAt()
codePointAt()

includes()
startsWith()
endsWith()

indexOf()
lastIndexOf()

slice()
substring()

toUpperCase()
toLowerCase()

trim()
trimStart()
trimEnd()

replace()
replaceAll()

split()

padStart()
padEnd()

repeat()

concat()

match()
matchAll()
search()

localeCompare()

normalize()

toString()
valueOf()
```

---

# 27. Useful Mental Model

Strings behave like:

```text
immutable sequences of text
```

They are **array-like** because you can:

```text
index them
read length
iterate through them
```

but they are not arrays.

So:

```js
myName[1];
```

works.

But:

```js
myName.push("!");
```

does not.

---

# 28. When to Convert a String Into an Array

You may not need to.

If you only need traversal:

```js
for (const char of word) {
}
```

is enough.

Convert when array operations make the problem easier:

```js
const result = word
  .split("")
  .filter(...)
  .map(...)
  .join("");
```

Do not convert automatically just because you know array methods.

Use the simplest representation for the task.

---

# 29. Common Mistakes

Trying to mutate a character:

```js
word[0] = "X";
```

Forgetting zero-based indexing:

```text
first character  → index 0
second character → index 1
```

Assuming strings have array methods:

```js
"hello".map(...)
```

They do not.

Forgetting that `+` may concatenate:

```js
"10" + 5;
```

returns:

```text
"105"
```

---

# 30. Quick Reference

```js
str.length;

str[0];
str.at(-1);

str.slice(start, end);

str.includes(value);
str.startsWith(value);
str.endsWith(value);

str.indexOf(value);

str.toUpperCase();
str.toLowerCase();

str.trim();

str.replace(oldValue, newValue);
str.replaceAll(oldValue, newValue);

str.split(separator);

str.padStart(length, fill);
str.padEnd(length, fill);

str.repeat(count);

String(value);
```

---

# Practice

1. If `const myName = "James";`, what does `myName[1]` return?
2. Why can you index a string but not change `myName[1]`?
3. How is a string similar to an array?
4. How is it different?
5. What does `split("")` do?
6. What does `slice(1, 4)` include?
7. Why can `str.length` be misleading with some Unicode characters?
