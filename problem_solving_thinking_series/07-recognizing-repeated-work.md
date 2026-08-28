# 7. Recognizing Repeated Work

## Objective

Learn to recognize when the program is doing the same expensive work again and again.

---

# Warning Signs

Listen to yourself describe your code.

If you say:

```text
"I keep searching..."
"I keep counting..."
"I keep recalculating..."
"I keep rebuilding..."
"I keep checking the same values..."
```

there may be repeated work.

---

# Example: Repeated Lookup

```js
for (const order of orders) {
  const user = users.find(user => user.id === order.userId);
}
```

If there are many orders, the users array is searched repeatedly.

Build an index once:

```js
const usersById = new Map(
  users.map(user => [user.id, user])
);

for (const order of orders) {
  const user = usersById.get(order.userId);
}
```

---

# Example: Repeated Window Sum

Naive:

```js
for (let start = 0; start <= numbers.length - k; start++) {
  let sum = 0;

  for (let i = start; i < start + k; i++) {
    sum += numbers[i];
  }
}
```

Notice adjacent windows share most of their values.

Sliding window:

```js
let sum = 0;

for (let i = 0; i < k; i++) {
  sum += numbers[i];
}

for (let i = k; i < numbers.length; i++) {
  sum += numbers[i];
  sum -= numbers[i - k];
}
```

Instead of recalculating everything:

```text
remove what left
add what entered
```

---

# Common Fixes for Repeated Work

| Repeated work | Possible response |
|---|---|
| membership search | Set |
| key lookup | Map |
| repeated calculation | cache / memoization |
| repeated range calculation | sliding window / prefix sum |
| repeated sorted search | binary search |
| repeated sorting | maintain structure / preprocess |
