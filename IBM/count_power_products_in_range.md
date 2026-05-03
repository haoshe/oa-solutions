# Count Power Products in Range
- **Company:** IBM
- **Difficulty:** Medium
- **Pattern:** Bounded Exponential Enumeration
- **Link:** https://www.fastprep.io/problems/ibm-count-power-products-in-range

## Problem
Given two integers low and high, count how many integers in the inclusive range
[low, high] can be written in the form 3^x * 5^y, where x and y are non-negative integers.

## Python
```python
class Solution:
    def countPowerProductsInRange(self, low: int, high: int) -> int:
        res = set()
        x = 0
        while 3 ** x <= high:
            y = 0
            while 5 ** y <= high:
                cur = 3**x * 5**y
                if cur >= low and cur <= high:
                    res.add(cur)
                y += 1
            x += 1
        return len(res)
```

