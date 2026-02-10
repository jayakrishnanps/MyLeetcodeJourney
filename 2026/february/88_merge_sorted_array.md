# 88. Merge Sorted Array

**Difficulty:** Easy  
**Topics:** Array, Two Pointers, Sorting

## Problem Description

You are given two integer arrays `nums1` and `nums2`, sorted in non-decreasing order, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

Merge `nums1` and `nums2` into a single array sorted in non-decreasing order.

The final sorted array should not be returned by the function, but instead be stored inside the array `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to 0 and should be ignored. `nums2` has a length of `n`.

## Examples

### Example 1:
```
Input: nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3
Output: [1,2,2,3,5,6]
Explanation: The arrays we are merging are [1,2,3] and [2,5,6].
The result of the merge is [1,2,2,3,5,6] with the underlined elements coming from nums1.
```

### Example 2:
```
Input: nums1 = [1], m = 1, nums2 = [], n = 0
Output: [1]
Explanation: The arrays we are merging are [1] and [].
The result of the merge is [1].
```

### Example 3:
```
Input: nums1 = [0], m = 0, nums2 = [1], n = 1
Output: [1]
Explanation: The arrays we are merging are [] and [1].
The result of the merge is [1].
Note that because m = 0, there are no elements in nums1. The 0 is only there to ensure the merge result can fit in nums1.
```

## Constraints

- `nums1.length == m + n`
- `nums2.length == n`
- `0 <= m, n <= 200`
- `1 <= m + n <= 200`
- `-10^9 <= nums1[i], nums2[j] <= 10^9`

---

## Solution 1: Two Pointers (Optimal)

### Code

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        p1 = m - 1
        p2 = n - 1
        write = (m + n) - 1
        
        while p2 >= 0:
            if p1 >= 0 and nums1[p1] > nums2[p2]:
                nums1[write] = nums1[p1]
                p1 -= 1
            else:
                nums1[write] = nums2[p2]
                p2 -= 1
            write -= 1
```

### Line-by-Line Explanation

#### Line 1: `class Solution:`
**Why is this here?**  
LeetCode requires your code to be inside a `Solution` class. LeetCode will automatically create an object of this class and call the method. This line is not algorithm logic — it's LeetCode boilerplate.

#### Line 2: `def merge(self, nums1, m, nums2, n):`
**Why is this here?**  
This is the function LeetCode calls. The parameters are dictated by the problem.

**Why these parameters?**
- `nums1`: the array we must modify in place
- `m`: tells us how many real elements are in `nums1`
- `nums2`: the second sorted array
- `n`: tells us how many elements are in `nums2`

**Why do we need `m` and `n`?**
- Because `nums1` contains fake zeros at the end
- Without `m`, we wouldn't know which elements are real
- Without `n`, we wouldn't know where `nums2` ends

#### Line 3: `p1 = m - 1`
**Why does this line exist?**  
We need a pointer that tells us where the real data in `nums1` ends.

**Why `m - 1`?**
- Arrays are 0-indexed
- If there are `m` real elements, their last index is `m - 1`

**Why not `len(nums1) - 1`?**
- Because the last part of `nums1` is empty space (zeros)
- We must never treat zeros as real values

**What does `p1` represent?**  
"The index of the largest remaining real value in `nums1`."

#### Line 4: `p2 = n - 1`
**Why does this line exist?**  
We also need a pointer for `nums2`.

**Why `n - 1`?**
- Same reason: arrays are 0-indexed
- The last element of `nums2` is at index `n - 1`

**What does `p2` represent?**  
"The index of the largest remaining value in `nums2`."

#### Line 5: `write = m + n - 1`
**Why does this line exist?**  
We need a place to write the merged values.

**Why `m + n - 1`?**
- Total size of final array = `m + n`
- Last index = `(m + n) - 1`

**Why do we write from the back?**
- Because the back has empty space
- Writing from the front would overwrite values we still need

**What does `write` represent?**  
"Where the next largest element should be placed in `nums1`."

#### Line 7: `while p2 >= 0:`
**Why is this a while loop?**  
Because we don't know in advance how many steps each pointer will move. One array may finish before the other.

**Why is the condition `p2 >= 0`?**  
This is extremely important. `p2 >= 0` means:  
👉 "There are still elements left in `nums2`."

**Why don't we check `p1 >= 0` here?**
- Because if `nums2` is empty, the merge is already complete
- Any remaining `nums1` elements are already in the correct place

**Core idea:** We only must place all elements of `nums2`.

#### Line 8: `if p1 >= 0 and nums1[p1] > nums2[p2]:`
This line has two conditions, so we ask two "why" questions.

**First part: `p1 >= 0`**  
**Why do we check `p1 >= 0`?**
- Because `p1` might become `-1`
- `-1` means: no real elements left in `nums1`

**What happens if we don't check this?**
- We'd read invalid data
- We'd compare `nums2` values with garbage or zeros

**Second part: `nums1[p1] > nums2[p2]`**  
**Why do we compare these two values?**
- Both arrays are sorted
- The largest remaining element must be at one of these two positions

**Why "greater than"?**
- We want to place the largest value first
- Because we are filling from the back

**Why are both conditions combined with `and`?**
- We can only compare values if `p1` is valid
- If `p1 < 0`, `nums1` has no values left → must take from `nums2`

#### Line 9: `nums1[write] = nums1[p1]`
**Why does this line exist?**  
Because `nums1[p1]` is the largest remaining element.

**Why assign it to `write`?**
- `write` points to the correct final position
- We are building the array from right to left

#### Line 10: `p1 -= 1`
**Why do we decrease `p1`?**
- We just used `nums1[p1]`
- That value is now placed correctly
- So we move to the next largest value in `nums1`

#### Line 11 (else): `else:`
**Why does this exist?**  
This handles two cases:
1. `nums2[p2]` is larger
2. `nums1` has no elements left (`p1 < 0`)

In both cases:  
👉 We must take from `nums2`.

#### Line 12: `nums1[write] = nums2[p2]`
**Why does this line exist?**
- `nums2[p2]` is the largest remaining element
- Or `nums1` is empty

**Why write into `nums1`?**
- The problem requires in-place modification
- `nums1` is the final array

#### Line 13: `p2 -= 1`
**Why do we decrease `p2`?**
- We just placed `nums2[p2]`
- So we move to the next element in `nums2`

#### Line 14: `write -= 1`
**Why do we decrease `write`?**
- We just filled one position
- The next largest value goes one position to the left

### Final Big-Picture WHY (Most Important)

We compare from the back because:
- Largest values live at the back
- Empty space is at the back
- No overwriting happens

---

## Solution 2: Copy and Sort (Simple)

### Code

```python
class Solution(object):
    def merge(self, nums1, m, nums2, n):
        for i in range(n):
            nums1[m+i] = nums2[i]
        nums1.sort()
```

### Line-by-Line Explanation

#### Line 1: `class Solution(object):`
**What is going on?**  
This is just the class wrapper LeetCode requires. `(object)` is old-style Python 2 syntax — still accepted by LeetCode.

#### Line 2: `def merge(self, nums1, m, nums2, n):`
**What is going on?**  
This is the function LeetCode will call. `nums1` must be modified in place.

#### Line 3: `for i in range(n):`
**What is going on?**
- This loop runs `n` times
- `i` goes from `0` to `n-1`

**Why `range(n)`?**
- `nums2` has exactly `n` elements
- We want to process all elements of `nums2`

#### Line 4: `nums1[m+i] = nums2[i]`
This is the most important line.

**What is going on?**
- `m` = number of real elements in `nums1`
- Index `m` is the first zero / empty slot
- `m + i` moves through the empty space

So:
- When `i = 0` → `nums1[m]`
- When `i = 1` → `nums1[m+1]`
- ...
- When `i = n-1` → `nums1[m+n-1]`

Each empty slot gets filled with a value from `nums2`.

#### Line 5: `nums1.sort()`
**What is going on?**  
After copying all elements from `nums2` into `nums1`, we sort the entire array to get the final merged result.

**Why does this work?**  
All elements are now in `nums1`, so sorting produces the correct merged array.

---

## Complexity Analysis

### Solution 1 (Two Pointers)
- **Time Complexity:** O(m + n)
- **Space Complexity:** O(1)

### Solution 2 (Copy and Sort)
- **Time Complexity:** O((m + n) log(m + n))
- **Space Complexity:** O(1) or O(m + n) depending on sort implementation

**Solution 1 is optimal** because it merges in linear time without requiring a sort operation.