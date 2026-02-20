# 108. Convert Sorted Array to Binary Search Tree

## 🧠 Problem
Given an integer array `nums` where the elements are sorted in ascending order, convert it into a **height-balanced Binary Search Tree (BST)**.

A height-balanced BST is a binary tree in which the depth of the two subtrees of every node never differs by more than one.

---

## 📌 Examples

### Example 1
**Input:**  
`nums = [-10, -3, 0, 5, 9]`  

**Output:**  
`[0, -3, 9, -10, null, 5]`  
(Other balanced BST structures are also valid.)

### Example 2
**Input:**  
`nums = [1, 3]`  

**Output:**  
`[3, 1]`  
(` [1, null, 3]` is also valid.)

---

## ⚙️ Constraints
- `1 <= nums.length <= 10^4`
- `-10^4 <= nums[i] <= 10^4`
- `nums` is sorted in **strictly increasing order**

---

## 💡 Approach
- Use **divide and conquer**
- Always pick the **middle element** as root to maintain balance
- Recursively build left and right subtrees

---

## 🧾 Code

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution(object):
    def sortedArrayToBST(self, nums):
        def build(left, right):
            if left > right:
                return None

            mid = (left + right) // 2
            root = TreeNode(nums[mid])

            root.left = build(left, mid - 1)
            root.right = build(mid + 1, right)

            return root

        return build(0, len(nums) - 1)
```

## 🪄 Line-by-Line Explanation

- **`class Solution(object):`** — Create a blueprint named `Solution` to hold the solution method.
- **`def sortedArrayToBST(self, nums):`** — Entry method: receives the sorted list and returns the BST root.
- **`def build(left, right):`** — Helper that constructs the tree from the array slice indexed from `left` to `right`.
- **`if left > right:`** — Base case: the slice is empty.
  - `return None` — Indicates no node for this subtree.
- **`mid = (left + right) // 2`** — Choose the middle index to keep the tree balanced.
- **`root = TreeNode(nums[mid])`** — Create the current root node using the middle value.
- **`root.left = build(left, mid - 1)`** — Recursively build and attach the left subtree.
- **`root.right = build(mid + 1, right)`** — Recursively build and attach the right subtree.
- **`return root`** — Return the subtree rooted at this node.
- **`return build(0, len(nums) - 1)`** — Start building using the whole array.

## ⏱️ Complexity

- **Time Complexity:** O(n) — each element is used once.
- **Space Complexity:** O(log n) — recursion stack (balanced tree).

