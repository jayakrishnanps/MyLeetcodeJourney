# 69. Sqrt(x)

## 🧠 Problem
Given a non-negative integer `x`, return the square root of `x` rounded down to the nearest integer. The returned integer should be non-negative as well.

**Constraint:** You must not use any built-in exponent function or operator (e.g., `pow(x, 0.5)` or `x ** 0.5`).

---

## 📌 Examples

### Example 1
**Input:**  
`x = 4`  
**Output:**  
`2`  
**Explanation:** The square root of 4 is 2.

### Example 2
**Input:**  
`x = 8`  
**Output:**  
`2`  
**Explanation:** The square root of 8 is 2.828..., rounded down to 2.

---

## ⚙️ Constraints
- `0 <= x <= 2^31 - 1`

---

## 💡 Approach
Find the **largest whole number whose square is <= x**. Start from 0 and increment until we overshoot, then return the previous value.

**Example:** For x = 8:
- 1 × 1 = 1 <= 8 ✓
- 2 × 2 = 4 <= 8 ✓
- 3 × 3 = 9 > 8 ✗
- Return 2

---

## ✅ Code

```python
class Solution(object):
    def mySqrt(self, x):
        i = 0
        while i * i <= x:
            i += 1
        return i - 1
```

## 🪄 Line-by-Line Explanation

- **`class Solution(object):`** — Blueprint to hold the solution method.
- **`def mySqrt(self, x):`** — Method that takes a non-negative integer and returns its square root (rounded down).
- **`i = 0`** — Initialize counter starting at 0.
- **`while i * i <= x:`** — Keep incrementing while the square of `i` doesn't exceed `x`.
  - `i += 1` — Increment the counter.
- **`return i - 1`** — When the loop ends, `i` has overshot; return the previous value.

## ⏱️ Complexity

- **Time Complexity:** O(√x) — we iterate from 0 to the square root of x.
- **Space Complexity:** O(1) — only using a single variable.
