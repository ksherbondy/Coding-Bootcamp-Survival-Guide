# JavaScript Set

> `Set` stores unique values.

## Learning Objectives

You should be able to:

- create Sets
- add and remove values
- test membership
- remove duplicates from arrays
- iterate Sets
- explain when Set is a better fit than Array

---

# 1. Creating a Set

```js
const values = new Set();
```

Initialize from an iterable:

```js
const values = new Set([1, 2, 3]);
```

---

# 2. Values Are Unique

```js
const values = new Set([1, 1, 2, 2, 3]);
```

contains:

```text
1
2
3
```

Duplicate insertions do not create additional entries.

---

# 3. Add

```js
values.add(4);
```

---

# 4. Check Membership

```js
values.has(4);
```

returns a boolean.

This is one of the biggest reasons to use a Set.

When the question is:

```text
Have I seen this value before?
```

a Set is often a natural choice.

---

# 5. Delete

```js
values.delete(4);
```

Remove everything:

```js
values.clear();
```

---

# 6. Size

```js
values.size;
```

Sets use:

```text
size
```

not:

```text
length
```

---

# 7. Remove Duplicates From an Array

```js
const numbers = [1, 1, 2, 2, 3];

const unique = [...new Set(numbers)];
```

Result:

```js
[1, 2, 3]
```

This is a common and expressive Set use.

---

# 8. Iteration

```js
for (const value of values) {
  console.log(value);
}
```

Sets preserve insertion order for iteration.

---

# 9. Set Methods

Common methods:

```text
add()
has()
delete()
clear()
entries()
keys()
values()
forEach()
```

Property:

```text
size
```

Modern JavaScript also includes set-operation methods in newer environments, such as:

```text
union()
intersection()
difference()
symmetricDifference()
isSubsetOf()
isSupersetOf()
isDisjointFrom()
```

Runtime support depends on the JavaScript version.

---

# 10. Set vs Array

Use an Array when:

```text
order/index position matters
duplicates are meaningful
you need array transformations
```

Use a Set when:

```text
uniqueness is central
membership checks are central
duplicates should collapse
```

---

# 11. Objects in Sets Use Identity

```js
const a = {};
const b = {};

const set = new Set();

set.add(a);

set.has(a); // true
set.has(b); // false
```

Even though both objects look empty, they are different objects.

---

# 12. Seen-Value Pattern

```js
const seen = new Set();

for (const value of values) {
  if (seen.has(value)) {
    console.log("duplicate:", value);
  }

  seen.add(value);
}
```

This pattern appears constantly in coding challenges.

---

# 13. Set Intersection Manually

```js
const a = new Set([1, 2, 3]);
const b = new Set([2, 3, 4]);

const common = [];

for (const value of a) {
  if (b.has(value)) {
    common.push(value);
  }
}
```

Result:

```js
[2, 3]
```

This shows how Set membership can reduce repeated searching.

---

# Practice

1. What defining rule does Set enforce?
2. What method checks whether a value exists?
3. Does Set use `.length` or `.size`?
4. How can Set remove duplicate array values?
5. When would an Array be a better fit?
