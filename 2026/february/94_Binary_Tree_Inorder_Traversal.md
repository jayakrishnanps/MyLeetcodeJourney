# 94. Binary Tree Inorder Traversal

## Problem Description

Given the `root` of a binary tree, return the **inorder traversal** of its nodes' values.

## Examples

### Example 1:
```
Input: root = [1,null,2,3]
Output: [1,3,2]
```

**Visual representation:**
```
    1
     \
      2
     /
    3
```

### Example 2:
```
Input: root = [1,2,3,4,5,null,8,null,null,6,7,9]
Output: [4,2,6,5,7,1,3,9,8]
```

**Visual representation:**
```
          1
        /   \
       2     3
      / \     \
     4   5     8
        / \   /
       6   7 9
```

### Example 3:
```
Input: root = []
Output: []
```

### Example 4:
```
Input: root = [1]
Output: [1]
```

## Constraints

- The number of nodes in the tree is in the range `[0, 100]`
- `-100 <= Node.val <= 100`

---

## What is Inorder Traversal?

Inorder traversal visits nodes in this order:
1. **Left** subtree first
2. **Current** node
3. **Right** subtree last

For a binary search tree, inorder traversal returns values in **sorted order**.

---

## Solution: Recursive Approach

### Code

```python
# Definition for a binary tree node.
# class TreeNode(object):
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution(object):
    def inorderTraversal(self, root):
        result = []
        
        def helper(node):
            if node is None:
                return
            
            helper(node.left)
            result.append(node.val)
            helper(node.right)
        
        helper(root)
        return result
```

---

## Line-by-Line Explanation

### Line 3: `result = []`

**WHY do we create `result`?**  
Because the problem asks us to return a list of values.

**WHY here and not inside recursion?**  
Because:
- Recursion runs many times
- We want ONE shared list
- All recursive calls append to the SAME list

If we created `result` inside the helper function, each recursive call would create a new list, and we'd lose all previously collected values.

---

### Line 4: `def helper(node):`

**WHY do we create a helper function?**  
Because recursion needs a function that can call itself.

**WHY not recurse directly in `inorderTraversal`?**  
Because:
- `inorderTraversal` is only called once by LeetCode
- `helper` is called many times by recursion
- Helper function provides a cleaner interface for recursive calls

---

### Line 5: `if node is None:`

**WHY check this?**  
Because:
- Some nodes don't have children
- `None` means "no node here"

**WHY is this important?**  
This is the **base case**.

Without it, recursion would never stop and would crash when trying to access `.left` or `.right` on `None`.

---

### Line 6: `return`

**WHY return nothing?**  
Because:
- There is nothing to add to the result
- Traversal of an empty subtree is finished

This is how recursion knows when to stop going deeper.

---

### Line 7: `helper(node.left)`

**WHY call `helper` on `node.left`?**  
Because inorder traversal **ALWAYS** starts with the left subtree.

**WHY not `node.left.val`?**  
Because:
- The left child might have its own children
- We must traverse the ENTIRE left subtree, not just one value
- Recursion handles the entire subtree automatically

---

### Line 8: `result.append(node.val)`

**WHY append here?**  
Because inorder order is:
```
LEFT → NODE → RIGHT
```

So we add the value:
- **AFTER** left finishes
- **BEFORE** right starts

This line is what makes it "inorder" traversal. The placement determines the traversal type:
- **Preorder**: append before left → `node, left, right`
- **Inorder**: append between left and right → `left, node, right`
- **Postorder**: append after right → `left, right, node`

---

### Line 9: `helper(node.right)`

**WHY do this last?**  
Because inorder traversal always ends with the right subtree.

Same logic as left:
- Right may have children
- Recursion handles it fully
- We trust the recursive call to process the entire right subtree

---

### Line 10: `helper(root)`

**WHY call `helper` with `root`?**  
Because this starts traversal from the top of the tree.

This is the single line that **kicks off recursion**.

---

### Line 11: `return result`

**WHY return `result` here?**  
Because:
- Recursion has finished
- `result` now contains values in inorder order
- LeetCode expects this list as the answer

---

## The ONE Sentence You Should Memorize

> **Each recursive call fully solves the problem for its subtree.**

### What This Means

**You don't:**
- Move pointers manually
- Grab `.left.val` directly
- Loop through nodes

**You:**
- Trust recursion
- Follow `left → node → right`
- Collect values in one shared list

---

## How Recursion Works (Step-by-Step)

Let's trace through Example 1: `root = [1,null,2,3]`

```
    1
     \
      2
     /
    3
```

**Execution flow:**

```
1. helper(1)
   - node = 1
   - helper(1.left) → helper(None) → returns immediately
   - append 1 to result → result = [1]
   - helper(1.right) → helper(2)
   
2. helper(2)
   - node = 2
   - helper(2.left) → helper(3)
   
3. helper(3)
   - node = 3
   - helper(3.left) → helper(None) → returns
   - append 3 to result → result = [1, 3]
   - helper(3.right) → helper(None) → returns
   - return to helper(2)
   
4. Back in helper(2):
   - append 2 to result → result = [1, 3, 2]
   - helper(2.right) → helper(None) → returns
   - return to helper(1)
   
5. Back in helper(1):
   - all done, return to main function

Final result: [1, 3, 2]
```

---

## Key Insights

### 1. **The order matters**
```python
helper(node.left)      # Process left subtree FIRST
result.append(node.val) # Then current node
helper(node.right)     # Finally right subtree
```

Changing this order creates different traversals:
- **Preorder**: `append → left → right`
- **Inorder**: `left → append → right`
- **Postorder**: `left → right → append`

### 2. **Base case prevents infinite recursion**
```python
if node is None:
    return
```
Without this, we'd try to access `None.left` and crash.

### 3. **Shared result list**
All recursive calls modify the **same** `result` list. This is why we declare it outside the helper function.

---

## Complexity Analysis

- **Time Complexity:** O(n)
  - We visit each node exactly once
  - n = number of nodes in the tree

- **Space Complexity:** O(n)
  - Recursion stack can go as deep as the tree height
  - In the worst case (skewed tree), this is O(n)
  - In the best case (balanced tree), this is O(log n)
  - The result list also takes O(n) space

---

## Alternative Approach: Iterative with Stack

While the recursive solution is elegant, you can also solve this iteratively using a stack:

```python
class Solution(object):
    def inorderTraversal(self, root):
        result = []
        stack = []
        current = root
        
        while current or stack:
            # Go to the leftmost node
            while current:
                stack.append(current)
                current = current.left
            
            # Process the node
            current = stack.pop()
            result.append(current.val)
            
            # Move to right subtree
            current = current.right
        
        return result
```

**When to use each:**
- **Recursive**: Cleaner, easier to understand, standard for tree problems
- **Iterative**: Uses explicit stack, sometimes required for follow-up questions

---

## Follow-Up Challenge

> Can you do it iteratively?

Yes! See the alternative approach above. The iterative solution simulates the call stack manually using a stack data structure.

---

## Summary

Inorder traversal is one of the three fundamental tree traversal methods. The recursive solution:
- Uses a helper function to traverse the tree
- Follows the pattern: **left → node → right**
- Collects values in a shared result list
- Relies on the base case (`node is None`) to stop recursion

**Remember:** Trust recursion. Each call solves the problem completely for its subtree.