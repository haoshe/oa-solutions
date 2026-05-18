# Circular Route Query Distance

**Source:** Amazon OA (FastPrep)
**Pattern:** Prefix Sum
**Difficulty:** Medium

---

## Problem

A circular route has `n` stops numbered from `0` to `n - 1`. The array `distances` has length `n`, where `distances[i]` is the distance from stop `i` to stop `(i + 1) % n`.

Given several route queries, each query `[start, end]` asks for the **shorter** of the two possible distances between those stops on the circle. Return the **sum of the shortest distances** over all queries.

---

## Examples

**Example 1**
```
distances = [1,2,3,4], queries = [[0,1],[1,3],[3,0]]
Output: 10
```
- [0,1] → clockwise = 1, counter-clockwise = 9 → min = **1**
- [1,3] → clockwise = 5, counter-clockwise = 5 → min = **5**
- [3,0] → clockwise = 4, counter-clockwise = 6 → min = **4**
- Total = **10**

**Example 2**
```
distances = [7,10,1,12], queries = [[0,2],[2,1]]
Output: 23
```
- [0,2] → counter-clockwise = 13, clockwise = 17 → min = **13**
- [2,1] → opposite direction = 10 → min = **10**
- Total = **23**

---

## Intuition

For each query `[start, end]`, we need the arc length in both directions:
- **Clockwise** (start → end going forward): `prefix[end] - prefix[start]`
- **Counter-clockwise** (the other way round): `total - clockwise`

Take `min` of the two. Precomputing prefix sums makes each query O(1).

---

## Solution

```python
from typing import List

class Solution:
    def minCircularQueryDistance(self, distances: List[int], queries: List[List[int]]) -> int:
        n = len(distances)

        # Build prefix sums
        prefix = [0] * (n + 1)
        for i in range(n):
            prefix[i + 1] = prefix[i] + distances[i]
        total = prefix[n]

        result = 0
        for start, end in queries:
            # Ensure start <= end for prefix sum calculation
            if start > end:
                start, end = end, start

            clockwise = prefix[end] - prefix[start]
            counter_clockwise = total - clockwise
            result += min(clockwise, counter_clockwise)

        return result
```

---

## Complexity

| | |
|---|---|
| Time | O(n + q) — O(n) to build prefix, O(1) per query |
| Space | O(n) — prefix array |

---

## Key Points

- Swap `start` and `end` if `start > end` so the prefix subtraction always goes left-to-right
- Counter-clockwise is simply `total - clockwise` — no need to recompute from scratch
- Classic two-direction circular distance pattern: precompute total, split into two arcs
