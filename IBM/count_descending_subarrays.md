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
  def countDescendingSubarrays(self, nums: List[int]) -> int:
    left, right = 0, 1
    res = 0
    while right < len(nums):
      while right < len(nums) and nums[right] == nums[right-1] -1:
        right += 1
      cur_len = right - left
      left = right
      if cur_len >= 2:
        res += cur_len * (cur_len - 1) // 2
      right += 1
    return res
```
