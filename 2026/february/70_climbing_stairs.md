# 70. Climbing Stairs

## Difficulty
Easy

---

## Problem Statement

You are climbing a staircase. It takes **n** steps to reach the top.

Each time, you can either climb **1 step** or **2 steps**.

Return the number of **distinct ways** you can climb to the top.

---

## Examples

### Example 1
**Input**
n = 2

**Output**
2

**Explanation**

There are two ways to climb to the top:
1. 1 step + 1 step  
2. 2 steps  

---

### Example 2
**Input**
n = 3

**Output**
3

**Explanation**

There are three ways to climb to the top:
1. 1 step + 1 step + 1 step  
2. 1 step + 2 steps  
3. 2 steps + 1 step  

---

## Constraints
- 1 <= n <= 45

---

## Approach

To reach step **n**, the final move must be:
- from step **n - 1** using 1 step
- from step **n - 2** using 2 steps

Therefore, the number of ways to reach step **n** is the sum of the ways to reach
steps **n - 1** and **n - 2**.

---

## Algorithm

1. If `n` is 1 or 2, return `n`.
2. Store the number of ways to reach the previous two steps.
3. Iteratively compute the number of ways for each step up to `n`.
4. Return the final value.

---

## Code (Accepted Solution)

```python
class Solution(object):
    def climbStairs(self, n):
        if n <= 2:
            return n

        prev2 = 1  # ways to reach step 1
        prev1 = 2  # ways to reach step 2

        for _ in range(3, n + 1):
            curr = prev1 + prev2
            prev2 = prev1
            prev1 = curr

        return prev1