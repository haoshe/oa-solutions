# Get Minimum Increment

**Source:** https://www.fastprep.io/problems/ibm-get-minimum-increment

---

## Problem Summary

Given an integer array `arr` of length `n`, you may perform **at most one** operation:

1. Choose a subset of elements where **no two selected elements are adjacent**.
2. Choose a non-negative integer `x`, and **add `x` to every selected element**.

Find the **minimum `x`** that makes the array non-decreasing. If it is impossible, return `-1`.

---

## Intuition

### Step 1 — Identify the violations

A violation is any pair `(i, i+1)` where `arr[i] > arr[i+1]` — i.e. a "drop" in the array.

Example: `[1, 1, 3, 2]` has one drop at pair `(2, 3)` since `3 > 2`.

### Step 2 — Which elements must be selected?

For each violation at pair `(i, i+1)`, we have two choices:

| Choice | Effect | Viable? |
|--------|--------|---------|
| Add x to `arr[i]` | Makes the drop worse | ✗ |
| Add x to `arr[i+1]` | Closes the gap | ✓ |

Adding x to `arr[i]` would require `arr[i] + x <= arr[i+1]`, which means `x <= arr[i+1] - arr[i] < 0`. Impossible since x must be non-negative.

**Conclusion:** for every drop at `(i, i+1)`, index `i+1` is **forced** into the selected set S.

```
S = { i+1 : arr[i] > arr[i+1] }
```

### Step 3 — When is it impossible?

Since no two adjacent indices can both be in S, if two forced indices are adjacent (e.g. both `arr[i] > arr[i+1]` and `arr[i+1] > arr[i+2]`), there is no valid selection → return `-1`.

### Step 4 — How large must x be?

**Lower bound** — x must be large enough to close every drop:

```
x >= arr[i] - arr[i+1]   for every bad pair (i, i+1)
lower = max of these values
```

**Upper bound** — x must not be so large that it creates a *new* violation on the right side of a selected element. For each `j` in S, after adding x:

```
arr[j] + x <= arr[j+1]
x <= arr[j+1] - arr[j]
upper = min of these values
```

If no element of S has a right neighbour (e.g. only the last index is in S), there is no upper constraint, so `upper = ∞`.

### Step 5 — Final answer

- If `lower <= upper`: the minimum valid x is `lower`.
- Otherwise: no valid x exists → return `-1`.

---

## Worked Example

```
arr = [1, 1, 3, 2]
```

**Find violations:** `arr[2] = 3 > arr[3] = 2` → drop at pair `(2, 3)`

**Build S:** `S = {3}`

**Adjacency check:** only one element in S, no issue.

**Lower bound:** `arr[2] - arr[3] = 3 - 2 = 1` → `lower = 1`

**Upper bound:** index 3 has no right neighbour → `upper = ∞`

**Answer:** `lower <= upper` → return **1** ✓

Verification: add 1 to index 3 → `[1, 1, 3, 3]` — non-decreasing. ✓

---

## Another Example

```
arr = [1, 5, 2, 3]
```

**Find violations:** `arr[1] = 5 > arr[2] = 2` → drop at pair `(1, 2)`

**Build S:** `S = {2}`

**Lower bound:** `5 - 2 = 3` → `lower = 3`

**Upper bound:** j=2, j+1=3: `arr[3] - arr[2] = 3 - 2 = 1` → `upper = 1`

**Answer:** `lower (3) > upper (1)` → return **-1** ✓

Verification: adding x=3 to index 2 → `[1, 5, 5, 3]` — still drops at `(2,3)`. No valid x exists.

---

## Impossible Case

```
arr = [3, 2, 1]
```

**Find violations:** drop at `(0,1)` and `(1,2)`

**Build S:** `S = {1, 2}`

**Adjacency check:** 1 and 2 are adjacent → return **-1** ✓

A strictly decreasing sequence of 3 elements cannot be fixed with a single non-adjacent selection.

---

## Solution

```python
class Solution:
    def getMinimumIncrement(self, arr: List[int]) -> int:
        n = len(arr)

        # Build the forced selection set
        S = {i+1 for i in range(n-1) if arr[i] > arr[i+1]}

        # Already non-decreasing
        if not S:
            return 0

        # Two adjacent forced indices — impossible
        for j in S:
            if j+1 in S:
                return -1

        # Minimum x required to close all drops
        lower = max(arr[i] - arr[i+1] for i in range(n-1) if arr[i] > arr[i+1])

        # Maximum x allowed before creating new violations on the right
        upper = min((arr[j+1] - arr[j] for j in S if j+1 < n), default=float('inf'))

        return lower if lower <= upper else -1
```

---

## Complexity

| | |
|---|---|
| Time | O(n) |
| Space | O(n) for S (at most n/2 elements due to non-adjacency) |
