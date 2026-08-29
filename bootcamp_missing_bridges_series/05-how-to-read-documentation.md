# 05. How to Read Documentation

## Turning reference material into working knowledge

Documentation can feel harder than tutorials because documentation is usually written to be precise, not necessarily to teach from zero.

A tutorial often answers:

```text
What should I type?
```

Documentation answers:

```text
What does this thing guarantee?
How do I use it correctly?
What inputs and outputs are valid?
```

---

# 1. Identify the Kind of Documentation

Common types:

```text
overview
tutorial
concept guide
API reference
method reference
configuration reference
migration guide
examples
```

Do not read an API reference as though it were a beginner tutorial.

Know what job the page is doing.

---

# 2. Start With the Signature

Example:

```js
array.map(callbackFn, thisArg?)
```

This tells us:

```text
name:
map

required:
callbackFn

optional:
thisArg
```

Then the callback signature:

```js
callbackFn(element, index, array)
```

tells us what values `map()` supplies.

---

# 3. Parameters vs Arguments

Documentation describes parameters:

```js
function add(a, b) {}
```

You supply arguments:

```js
add(2, 3);
```

Learning the vocabulary makes technical references easier to parse.

---

# 4. Find the Return Value

A function may:

```text
return a new value
mutate existing state
return undefined
return a promise
return this
throw
```

Those behaviors determine how you should use it.

Example:

```text
push()
→ mutates array
→ returns new length

map()
→ returns new array
→ does not replace original automatically
```

---

# 5. Look for Mutation

Ask:

```text
Does this change the original?
```

Examples:

```text
map     → new array
filter  → new array
sort    → mutates array
push    → mutates array
```

Mutation information is often more important than the example code.

---

# 6. Read Exceptions and Edge Cases

Look for behavior when:

```text
input missing
wrong type
empty collection
NaN
network failure
file missing
invalid option
```

These sections explain bugs you will eventually encounter.

---

# 7. Check Version Information

Libraries change.

Look for:

```text
deprecated
removed
introduced
changed
migration
version
```

An old tutorial may use an API that current documentation warns against.

---

# 8. Read Examples After the Contract

Examples are more useful after you understand:

```text
inputs
outputs
side effects
constraints
```

Otherwise it is easy to copy parts without knowing which pieces are essential.

---

# 9. Translate Docs Into Plain English

Formal:

```text
Returns a new array populated with the results of calling a
provided function on every element in the calling array.
```

Plain-English model:

```text
map walks through every element,
calls my function,
collects what my function returns,
and gives me a new array.
```

Mechanical expansion:

```js
const output = [];

for (const item of input) {
  output.push(callback(item));
}
```

This is how you connect abstraction to execution.

---

# 10. Read Documentation to Answer Questions

Do not always read an entire page.

Ask:

```text
Does this mutate?
What does it return?
What arguments does the callback receive?
Can it throw?
Does it return a promise?
What status codes can happen?
```

Targeted documentation reading is a professional skill.

---

# 11. Learn Common Notation

You may see:

```text
?
[]
...
|
<>
```

Examples:

```text
value?          optional
...args         rest values
string | null   either type
Array<string>   array of strings
```

Syntax differs across languages and documentation systems, but recognizing type notation removes a lot of intimidation.

---

# 12. Type Definitions Are Documentation

```ts
function getUser(id: string): Promise<User | null>
```

This says:

```text
input:
string

immediate return:
Promise

eventual result:
User or null
```

A signature can communicate an enormous amount.

---

# Documentation Reading Worksheet

```text
NAME:

PURPOSE:

DOCUMENT TYPE:

SIGNATURE:

REQUIRED INPUTS:

OPTIONAL INPUTS:

RETURN VALUE:

MUTATES?:

SIDE EFFECTS:

ASYNC?:

ERRORS / EXCEPTIONS:

EDGE CASES:

VERSION NOTES:

ONE-SENTENCE MENTAL MODEL:
```

---

# Practice

Choose one:

```text
Array.prototype.reduce
fetch
JSON.parse
Promise.all
fs.readFile
```

Do not copy an example first.

Complete the worksheet, then write your own tiny example from the contract you understood.

That is how documentation becomes a working tool.
