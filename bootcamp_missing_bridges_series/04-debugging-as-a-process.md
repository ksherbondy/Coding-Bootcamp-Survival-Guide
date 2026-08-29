# 04. Debugging as a Process

## Stop guessing. Start narrowing.

Debugging is often taught as:

```text
add console.log()
```

But logging is only one tool.

Debugging is a reasoning process.

A useful model:

```text
OBSERVE
  ↓
STATE EXPECTATION
  ↓
FORM HYPOTHESIS
  ↓
ISOLATE
  ↓
INSTRUMENT
  ↓
TEST
  ↓
UPDATE BELIEF
  ↓
FIX
  ↓
VERIFY
```

---

# 1. Start With Expected vs Actual

Do not begin with:

```text
It doesn't work.
```

Write:

```text
Expected:
clicking Save sends PUT /api/profile and shows success.

Actual:
clicking Save produces no network request.
```

Now the problem is already smaller.

---

# 2. Reproduce the Bug

Record:

```text
input
environment
steps
expected behavior
actual behavior
```

Example:

```text
1. open profile page
2. change display name
3. click Save
4. nothing happens
```

A reproducible bug can be tested.

---

# 3. Find the First Wrong Observation

Suppose the expected chain is:

```text
click
→ handler
→ fetch
→ server
→ database
→ response
→ UI
```

Test each boundary.

If the handler never runs, the database is irrelevant.

Find the first point where reality differs from expectation.

---

# 4. Use Binary Search Debugging

Imagine:

```text
input
→ transform
→ validate
→ save
→ format
→ render
```

Inspect near the middle.

If `save` receives correct data, the bug is likely later.

If it receives wrong data, the bug is earlier.

You just removed half the search space.

---

# 5. Log Questions, Not Random Values

Weak:

```js
console.log(data);
console.log("here");
console.log("test");
```

Better:

```js
console.log("saveProfile input:", profile);
console.log("validation result:", validation);
console.log("PUT /api/profile status:", response.status);
```

Every log should answer a question.

---

# 6. Use Assertions for Assumptions

```js
console.assert(
  userId !== undefined,
  "userId should exist before saveProfile"
);
```

An assertion turns a hidden assumption into an explicit check.

---

# 7. Read Stack Traces as Call Paths

Look for:

```text
your file
your function
your line
```

Then ask:

```text
What value existed here?
Who called this function?
What assumption failed?
```

A stack trace is not just an error wall. It is a history of active calls.

---

# 8. Reduce the Problem

Instead of debugging a chain:

```js
loadUsers()
  .filter(...)
  .map(...)
  .reduce(...);
```

split it:

```js
const users = loadUsers();
const active = users.filter(...);
const names = active.map(...);
const result = names.reduce(...);
```

Now every stage can be inspected.

Abstraction is useful, but temporary de-abstraction is a debugging tool.

---

# 9. Build a Minimal Reproduction

If the failing system is huge, reproduce the behavior in the smallest possible environment.

This helps distinguish:

```text
framework problem?
library problem?
our code?
our data?
our environment?
```

---

# 10. Debug Data Flow

Ask:

```text
Where did this value come from?
What type is it?
When did it change?
Who changed it?
```

Many bugs are really:

```text
wrong value
wrong type
wrong time
wrong owner
```

---

# 11. Debug Control Flow

If code "never gets there," inspect decisions.

```js
if (user) {
  if (user.active) {
    save(user);
  }
}
```

Check:

```text
Was user truthy?
Was user.active true?
Was save called?
```

---

# 12. Debug Async Code

Ask:

```text
Did the operation start?
Did it resolve or reject?
Did I await it?
Did I handle rejection?
Did execution continue before completion?
```

Example:

```js
try {
  const response = await fetch(url);
  console.log("status:", response.status);
} catch (error) {
  console.error("request failed:", error);
}
```

---

# 13. Debug HTTP With the Network Inspector

Browser developer tools can reveal:

```text
URL
method
status
request headers
request body
response body
timing
```

If an API call is failing, this is direct evidence.

---

# 14. Inspect State Before Changing State

This principle applies everywhere.

Git:

```bash
git status
```

Application:

```js
console.log(currentState);
```

Database:

```sql
SELECT ...
```

Do not issue more changes until you know what is currently true.

---

# 15. Keep a Hypothesis Log

```text
Hypothesis 1:
event listener is missing
→ disproved: handler logs

Hypothesis 2:
request body lacks id
→ confirmed in Network tab
```

This prevents circular guessing.

---

# 16. Fix the Cause, Not Only the Symptom

This:

```js
if (value === undefined) value = 0;
```

may stop a crash.

But why was `value` undefined?

Sometimes the correct fix is upstream.

---

# 17. Verify the Fix

After the bug disappears:

```text
Does the original failing case pass?
Do normal cases still pass?
Did I create another edge case?
Should I add a regression test?
```

---

# Debugging Template

```text
EXPECTED:

ACTUAL:

REPRODUCTION STEPS:

FIRST KNOWN CORRECT POINT:

FIRST KNOWN INCORRECT POINT:

CURRENT HYPOTHESIS:

EVIDENCE FOR:

EVIDENCE AGAINST:

NEXT TEST:

ROOT CAUSE:

FIX:

VERIFICATION:
```

---

# Final Mental Model

Debugging is not:

```text
try random changes until something works
```

It is:

```text
reduce uncertainty one observation at a time
```
