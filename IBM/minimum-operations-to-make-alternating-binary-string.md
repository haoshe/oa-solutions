# Minimum Operations to Make Alternating Binary String
- **Company:** IBM
- **Difficulty:** Medium
- **Pattern:** Greedy
- **Link:** https://www.fastprep.io/problems/ibm-minimum-operations-to-make-alternating-binary-string
## Problem
Given a binary string, find the minimum number of operations required to make it alternating.
Each operation selects any bit and flips it (0 to 1 or 1 to 0).
## Approach
There are only two valid alternating binary strings:
- Pattern A: `010101...`
- Pattern B: `101010...`

Count mismatches against Pattern A. Mismatches against Pattern B = `len(s) - count`.
Return the minimum of the two.
## Python
```python
class Solution:
    def minOperations(self, s: str) -> int:
        count = 0
        for i in range(len(s)):
            if s[i] != str(i % 2):  # compare against pattern A (010101...)
                count += 1
        return min(count, len(s) - count)  # flips for A vs flips for B
```
