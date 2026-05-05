# Process Execution Time
- **Company:** IBM
- **Difficulty:** Medium
- **Pattern:** Interval Merging
- **Link:** https://www.fastprep.io/problems/ibm-process-execution-time

## Problem
Given the inclusive start and end times of multiple processes, compute the total amount of
time during which at least one process is running. Overlapping intervals should be merged
before summing their lengths.

## Key Observations
- Intervals are inclusive: a process from 1 to 5 contributes 5 - 1 + 1 = 5 units of time
- Overlapping intervals must be merged first, then summed
- Input is given as two separate lists (start and end), not as pairs

## Python
```python
class Solution:
    def getExecutionTime(self, start: List[int], end: List[int]) -> int:
        # zip(start, end) pairs elements by index into tuples: [(1,5), (2,6), (8,10)]
        # sorted() sorts by the first element (start time) by default
        intervals = sorted(zip(start, end))

        merged = []
        for interval in intervals:
            if not merged or merged[-1][1] < interval[0]:
                # no overlap — append as a list (not tuple) so we can mutate it later
                merged.append(list(interval))
            else:
                # overlap — extend the current interval's end time if needed
                merged[-1][1] = max(merged[-1][1], interval[1])

        time = 0
        for interval in merged:
            # +1 because intervals are inclusive
            time += interval[1] - interval[0] + 1
        return time
```

## Why `list(interval)`?
`zip` produces tuples which are **immutable** — you cannot do `merged[-1][1] = ...` on a tuple.
Converting to a list with `list(interval)` allows the end time to be updated in place during merging.

## Why `sorted(zip(start, end))`?
The merge algorithm only works correctly if intervals are sorted by start time.
`zip` combines the two lists into pairs, and `sorted` orders them by start time (first element of each tuple) by default.
