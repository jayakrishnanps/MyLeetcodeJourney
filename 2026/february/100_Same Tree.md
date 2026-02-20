# 100. Same Tree

## Problem Statement

Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.  
Two binary trees are considered the same if they are **structurally identical**, and the nodes have the same value.

---

## Examples

### Example 1
**Input** p = [1,2,3], q = [1,2,3]  
**Output** true

---

### Example 2
**Input** p = [1,2], q = [1,null,2]  
**Output** false

---

### Example 3
**Input** p = [1,2,1], q = [1,1,2]  
**Output** false

---

## Constraints
- The number of nodes in both trees is in the range `[0, 100]`.
- `-10^4 <= Node.val <= 10^4`

---

## Key Observation

A binary tree is a recursive data structure. To determine if two trees are identical, we must verify three conditions simultaneously:
1. The **values** of the current nodes are equal.
2. The **left subtrees** are identical.
3. The **right subtrees** are identical.

If the structure differs (e.g., one has a left child and the other doesn't), they are not the same.

---

## Approach

1. **Check Base Cases**: First, handle the scenario where we reach the end of the branches (null nodes).
2. **Compare Values**: If both nodes exist, compare their stored values.
3. **Structural Check**: Ensure both trees have nodes in the same positions.
4. **Recurse**: Move down to the next level by calling the same function on the children.

---

## Code (Accepted Solution)

```python
class Solution(object):
    def isSameTree(self, p, q):
        if not p and not q:
            return True
        if not p or not q or p.val != q.val:
            return False
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)