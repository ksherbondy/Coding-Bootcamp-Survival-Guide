# JavaScript Types and Data Structures

> A concept-first series for understanding what JavaScript values really are, how they behave, what operations they support, and when to use them.

This series is meant to be read as a companion to JavaScript practice, not as a replacement for writing code.

The recurring questions are:

```text
What is this thing?
How do I create it?
How does JavaScript treat it?
What can I do with it?
Is it mutable?
How is it compared?
What methods does it have?
What is it often confused with?
When should I use it?
```

## Lessons

1. `01-numbers.md`
2. `02-strings.md`
3. `03-booleans.md`
4. `04-null-and-undefined.md`
5. `05-bigint.md`
6. `06-symbol.md`
7. `07-objects.md`
8. `08-arrays.md`
9. `09-functions.md`
10. `10-map.md`
11. `11-set.md`

## Primitive Types

JavaScript has seven primitive types:

```text
string
number
bigint
boolean
undefined
symbol
null
```

Everything else is an object.

That includes:

```text
arrays
functions
maps
sets
dates
regular expressions
promises
class instances
```

## Core Mental Model

A useful first split is:

```text
primitive
→ usually represents one value directly
→ immutable

object
→ collection / structure / behavior
→ usually mutable
→ compared by identity/reference
```

There are important details and exceptions, but this distinction gives beginners a strong starting point.
