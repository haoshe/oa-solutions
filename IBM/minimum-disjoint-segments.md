# Minimum Number of Non-Empty Disjoint Segments
- **Company:** IBM
- **Difficulty:** Medium
- **Pattern:** Greedy / Frequency Count
- **Link:** https://www.fastprep.io/problems/ibm-minimum-disjoint-segments

## Problem
Given a string, find the minimum number of non-empty disjoint segments a string can be
partitioned into, such that each segment has no repeated characters.

Choose the partitioning that yields the fewest such segments among all possible ways to
partition the string. Each character of the string should be in exactly one segment.

## Key Insight
The answer is the maximum frequency of any single character in the string.
Every time a character repeats, it must go into a different segment. The character
that appears the most times forces that many segments minimum.

## Python
```python
from collections import Counter

class Solution:
    def minimumDisjointSegments(self, s: str) -> int:
        return max(Counter(s).values())
```
