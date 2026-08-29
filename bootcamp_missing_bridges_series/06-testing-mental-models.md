# 06. Testing Mental Models

## What exactly is this test proving?

Students often learn test syntax:

```js
expect(result).toBe(5);
```

before learning what makes a test useful.

A testing framework is only a tool.

The deeper question is:

```text
What behavior should this code guarantee?
```

---

# 1. A Test Is an Executable Claim

```js
expect(add(2, 3)).toBe(5);
```

claims:

```text
Given 2 and 3,
add should return 5.
```

A useful test communicates an expected behavior.

---

# 2. Arrange, Act, Assert

```text
ARRANGE
prepare inputs/state

ACT
run behavior

ASSERT
check result
```

Example:

```js
const cart = [{ price: 10 }, { price: 5 }];

const total = calculateTotal(cart);

expect(total).toBe(15);
```

---

# 3. Test Behavior, Not Implementation Detail

Suppose:

```js
function total(nums) {
  return nums.reduce((a, b) => a + b, 0);
}
```

The guarantee is:

```text
returns the sum
```

The test should not require `reduce()` specifically.

If you replace it with a loop, behavior tests should still pass.

---

# 4. Happy Paths

Normal expected use:

```text
valid user
valid request
non-empty array
successful database response
```

Happy paths are necessary.

They are not enough.

---

# 5. Edge Cases

Ask:

```text
empty?
one item?
minimum?
maximum?
duplicate?
negative?
zero?
missing?
null?
undefined?
exact boundary?
```

This is where testing and general problem-solving thinking overlap.

---

# 6. Failure Cases

Failure behavior is still behavior.

Example:

```js
expect(() => parseAge("abc")).toThrow();
```

API example:

```text
invalid credentials
→ 401
```

A system should fail in expected ways.

---

# 7. Regression Tests

A regression test captures a bug after you find it.

```text
Bug:
empty cart crashed checkout.

Regression test:
empty cart returns total 0.
```

Now future changes must preserve the fix.

---

# 8. Unit Tests

A unit test focuses on a small piece:

```js
calculateTax(100)
```

Strengths:

```text
fast
focused
easy to isolate
```

But it does not prove the whole application works together.

---

# 9. Integration Tests

Integration tests check components together.

Example:

```text
HTTP request
→ route
→ service
→ database test instance
→ response
```

They catch boundary errors that isolated tests may miss.

---

# 10. End-to-End Tests

An end-to-end test may simulate:

```text
open browser
click login
enter credentials
submit
see dashboard
```

It tests from a user's perspective.

It is powerful, but slower and more complex.

---

# 11. Mocking

Mocks replace dependencies with controlled behavior.

Example:

```text
Instead of calling real payment API,
return a fake success response.
```

Mocks help isolate.

But excessive mocking creates a danger:

```text
Am I testing my code,
or only testing the fake environment I invented?
```

---

# 12. Determinism

A good test should usually produce the same result under the same conditions.

Sources of nondeterminism:

```text
random values
current time
network
external APIs
database leftovers
race conditions
```

Control them when practical.

---

# 13. Test Names Are Documentation

Weak:

```text
test user stuff
```

Better:

```text
returns user when id exists
returns 404 when id is missing
rejects invalid id format
```

A test suite should explain expected behavior to a reader.

---

# 14. Passing Tests Do Not Prove Everything

Passing tests prove:

```text
the claims we wrote are currently satisfied
```

They do not prove:

```text
all possible behavior is correct
```

Untested behavior remains unknown.

---

# Testing Checklist

```text
What behavior am I guaranteeing?
What input triggers it?
What output or side effect should happen?
What boundaries exist?
What failure cases exist?
Which dependencies should be real?
Which should be controlled?
Could this test fail nondeterministically?
Would it remain valid after refactoring?
```

---

# Practice

For:

```js
function divide(a, b) {
  return a / b;
}
```

Before writing test code, identify policy for:

```text
normal values
zero numerator
zero denominator
negative values
floating point result
non-number input
```

Testing begins with thinking, not syntax.
