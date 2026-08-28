# 5. Algorithm Selection Checklist

## Objective

Use problem properties to generate candidate algorithms.

---

# Checklist

## Is the input sorted?

Consider:

```text
binary search
two pointers
early stopping
neighbor comparison
```

## Does the problem say contiguous, consecutive, substring, or subarray?

Consider:

```text
sliding window
```

## Does it ask for two values?

Consider:

```text
Set
Map
two pointers
```

## Does it ask for all possible combinations or arrangements?

Consider:

```text
recursion
backtracking
```

## Does it ask for shortest path in an unweighted graph?

Consider:

```text
BFS
queue
```

## Does it ask for top K / highest priority?

Consider:

```text
heap / priority queue
```

## Does it involve overlapping ranges?

Consider:

```text
sort + merge
```

## Does it repeatedly halve the search space?

Consider:

```text
binary search
logarithmic behavior
```

## Does it repeatedly double/halve values?

Consider:

```text
powers of two
binary
bit shifts
log2
```

---

# Example

## Problem

> Given a sorted array, determine whether two values sum to the target.

Signals:

```text
sorted
two values
sum
target
```

Candidates:

```text
two pointers
Set
```

Because the input is sorted, two pointers become especially attractive:

```js
function hasPair(numbers, target) {
  let left = 0;
  let right = numbers.length - 1;

  while (left < right) {
    const sum = numbers[left] + numbers[right];

    if (sum === target) return true;

    if (sum < target) {
      left++;
    } else {
      right--;
    }
  }

  return false;
}
```

The algorithm came from the problem's properties, not from memorizing code.
