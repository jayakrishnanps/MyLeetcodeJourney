# 101. Symmetric Tree

## Problem Statement

Given the `root` of a binary tree, check whether it is a mirror of itself (i.e., symmetric around its center).

---

## Examples

### Example 1

**Input:** root = [1,2,2,3,4,4,3]  
**Output:** true

---

### Example 2

**Input:** root = [1,2,2,null,3,null,3]  
**Output:** false

---

## Constraints
- The number of nodes in the tree is in the range `[1, 1000]`.
- `-100 <= Node.val <= 100`

---

## Key Observation

A tree is symmetric if the left subtree is a **mirror image** of the right subtree. This requires a "cross-comparison" where:
1. The **left** child of the left subtree is compared to the **right** child of the right subtree.
2. The **right** child of the left subtree is compared to the **left** child of the right subtree.

---

## Approach

1. **Start at the Root**: If the tree is empty, it is symmetric.
2. **Helper Function**: We create a helper function `isMirror` that takes two nodes (a left side and a right side) to compare them.
3. **Base Cases**: 
   - If both nodes are null, we've reached the end of a matching branch.
   - If only one node is null, or the values don't match, the symmetry is broken.
4. **Recursive Step**: We recursively check the "outer" children and the "inner" children.

---

## Code (Accepted Solution)

```python
class Solution(object):
    def isSymmetric(self, root):
        if not root:
            return True
        return self.isMirror(root.left, root.right)

    def isMirror(self, left, right):
        if not left and not right:
            return True
        if not left or not right or left.val != right.val:
            return False
        return self.isMirror(left.left, right.right) and self.isMirror(left.right, right.left)