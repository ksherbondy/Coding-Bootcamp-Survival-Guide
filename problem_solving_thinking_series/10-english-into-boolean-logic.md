# 10. English Into Boolean Logic

## Objective

Translate natural-language conditions into boolean expressions.

---

# Common Translations

## "A and B"

```js
A && B
```

## "A or B"

```js
A || B
```

## "not A"

```js
!A
```

## "between 5 and 10 inclusive"

```js
value >= 5 && value <= 10
```

## "at least 18"

```js
age >= 18
```

## "less than 100"

```js
value < 100
```

## "not empty"

```js
array.length > 0
```

---

# Example

Problem:

> A user may enter if they are an admin OR they are at least 18 AND have a valid ticket.

Translate structure first:

```text
admin
OR
(age >= 18 AND valid ticket)
```

JavaScript:

```js
const canEnter =
  isAdmin ||
  (age >= 18 && hasValidTicket);
```

Parentheses make the intended logic obvious.

---

# Instructor Exercise

Translate before coding:

> Return true when a number is positive, even, and less than 100.

Reasoning:

```text
positive → n > 0
even → n % 2 === 0
less than 100 → n < 100
all conditions → &&
```

Solution:

```js
return n > 0 && n % 2 === 0 && n < 100;
```
