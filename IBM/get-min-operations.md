# Get Min Operations

**Source:** IBM Online Assessment — [FastPrep](https://www.fastprep.io/problems/ibm-get-min-operations)  
**Difficulty:** Medium  
**Pattern:** Greedy / Sliding Window

---

## Problem

Given a binary string `s` of length `n`, return the **minimum number of operations** to ensure no segment of `m` or more consecutive `'0'`s exists.

**One operation:** choose any contiguous segment of length `k` and set every bit to `'1'`.

**Function signature:**
```python
def getMinOperations(s: str, m: int, k: int) -> int:
```

---

## Example

```
s = "000000", m = 3, k = 2

Apply one operation at [3, 4] (1-based) → "001100"
Runs of zeros: "00", "00" — both length 2 < 3 ✓

Output: 1
```

---

## Key Insight

This is a **greedy interval cover** problem on zero-runs.

Scan left to right tracking consecutive zeros. The moment we hit the `m`-th consecutive zero at position `i`, we **must** act. To maximise rightward coverage, place the `k`-block starting at position `i`, covering `[i, i+k-1]`. Then resume scanning from `i+k`.

**Why place the block as far right as possible?**  
Any valid block must cover position `i` (the trigger). Placing it at `[i, i+k-1]` rather than `[i-m+1, ...]` pushes coverage further right, potentially absorbing future violations — the same logic as Jump Game II.

---

## Solution

```python
def getMinOperations(s: str, m: int, k: int) -> int:
    count = 0
    ops = 0
    i = 0
    while i < len(s):
        if s[i] == '0':
            count += 1
            if count == m:       # must place a block now
                ops += 1
                i += k - 1      # after i+=1 below, next = i+k
                count = 0
        else:
            count = 0
        i += 1
    return ops
```

**Time:** O(n)  
**Space:** O(1)

---

## Trace: `s = "0000000"`, `m = 3`, `k = 2`

| i | s[i] | count | action            | next i |
|---|------|-------|-------------------|--------|
| 0 | '0'  | 1     | —                 | 1      |
| 1 | '0'  | 2     | —                 | 2      |
| 2 | '0'  | 3     | ops=1, place block| 4      |
| 4 | '0'  | 1     | —                 | 5      |
| 5 | '0'  | 2     | —                 | 6      |
| 6 | '0'  | 3     | ops=2, place block| 8      |

**Output: 2** ✓

---

## Common Pitfall

After placing the block and skipping `k` positions, the skip must land on `i+k` — the **first character after the block**. A naive `i += k` followed by `i += 1` (without removing the unconditional increment) overshoots to `i+k+1`, silently dropping a zero from the count.

```python
# BUG — skips one position too many
if count == m:
    i = i + k   # then i += 1 fires → lands at i+k+1, not i+k
    ops += 1
    count = 0
i += 1
```

This passes `"000000"` (m=3, k=2) by luck but fails `"0000000"` (returns 1 instead of 2).

---

## Constraints

- `1 ≤ n ≤ 2 × 10⁵`
- `1 ≤ m, k ≤ n`
