# 67. Add Binary

## 🧠 Problem
Given two binary strings `a` and `b`, return their sum as a binary string.

---

## 📌 Examples

### Example 1
**Input:**  
`a = "11"`, `b = "1"`  
**Output:**  
`"100"`  
**Explanation:** 3 + 1 = 4 in binary: "11" + "1" = "100"

### Example 2
**Input:**  
`a = "1010"`, `b = "1011"`  
**Output:**  
`"10101"`  
**Explanation:** 10 + 11 = 21 in binary: "1010" + "1011" = "10101"

---

## ⚙️ Constraints
- `1 <= a.length, b.length <= 10^4`
- `a` and `b` consist only of '0' or '1' characters.
- Each string does not contain leading zeros except for the zero itself.

---

## 💡 Approach
Process both strings from **right to left** (least significant to most significant bit), adding digits and tracking the carry just like manual addition.

---

## ✅ Code

```python
class Solution(object):
    def addBinary(self, a, b):
        i = len(a) - 1
        j = len(b) - 1
        carry = 0
        result = []
        while i >= 0 or j >= 0 or carry:
            total = carry
            if i >= 0:
                total += int(a[i])
                i -= 1
            if j >= 0:
                total += int(b[j])
                j -= 1
            result.append(str(total % 2))
            carry = total // 2
        return ''.join(reversed(result))
```

## 🪄 Line-by-Line Explanation

- **`i = len(a) - 1`** — Start at the last index of string `a`.
- **`j = len(b) - 1`** — Start at the last index of string `b`.
- **`carry = 0`** — Initialize carry to 0 (no carry initially).
- **`result = []`** — List to store binary digits of the result (in reverse order).
- **`while i >= 0 or j >= 0 or carry:`** — Continue while there are digits in either string or a pending carry.
  - `total = carry` — Start with the carry from the previous iteration.
  - **`if i >= 0:`** — If `a` has a remaining digit at index `i`:
    - `total += int(a[i])` — Add it to the total.
    - `i -= 1` — Move to the next digit in `a`.
  - **`if j >= 0:`** — If `b` has a remaining digit at index `j`:
    - `total += int(b[j])` — Add it to the total.
    - `j -= 1` — Move to the next digit in `b`.
  - **`result.append(str(total % 2))`** — Append the current bit (0 or 1) to result.
  - **`carry = total // 2`** — Calculate the carry for the next iteration (0 or 1).
- **`return ''.join(reversed(result))`** — Reverse the result list (since we built it backwards) and join into a string.

## ⏱️ Complexity

- **Time Complexity:** O(max(m, n)) — where m and n are lengths of `a` and `b`; we process each digit once.
- **Space Complexity:** O(max(m, n)) — for the result string.
