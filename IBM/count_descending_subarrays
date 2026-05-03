# Count Descending Subarrays
- **Company:** IBM
- **Difficulty:** Medium
- **Pattern:** Two Pointers + Combinatorics
- **Link:** https://www.fastprep.io/problems/ibm-count-descending-subarrays

## Problem
Given an integer array nums, count how many contiguous subarrays
of length at least 2 are strictly descending by exactly 1 at every step.

## Python
```python
def countDescendingSubarrays(nums):
    left = 0
    count = 0
    for right in range(1, len(nums)):
        if nums[right] != nums[right - 1] - 1:
            sub_len = right - left
            left = right
            count += sub_len * (sub_len - 1) // 2
    sub_len = len(nums) - left
    count += sub_len * (sub_len - 1) // 2
    return count
```
