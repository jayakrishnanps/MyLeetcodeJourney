# 110. Balanced Binary Tree

## 🧠 Problem
Given a binary tree, determine if it is **height-balanced**.

A binary tree is height-balanced if:
> The depth of the two subtrees of every node never differs by more than one.

---

## 📌 Examples

### Example 1
**Input:**  
`root = [3,9,20,null,null,15,7]`  
**Output:**  
`true`

### Example 2
**Input:**  
`root = [1,2,2,3,3,null,null,4,4]`  
**Output:**  
`false`

### Example 3
**Input:**  
`root = []`  
**Output:**  
`true`

---

## ⚙️ Constraints
- Number of nodes: `[0, 5000]`
- `-10⁴ <= Node.val <= 10⁴`

---

## 💡 Optimal Approach (O(n))
Instead of checking height repeatedly (which gives **O(n²)**), we:
- Compute height
- Check balance  
**in the same recursion**

We use a special value `-1` as a **failure signal** to stop extra work early.

---

## ✅ Code

```python
class Solution(object):
    def isBalanced(self, root):

        def height(node):
            if not node:
                return 0

            left = height(node.left)
            if left == -1:
                return -1

            right = height(node.right)
            if right == -1:
                return -1

            if abs(left - right) > 1:
                return -1

            return max(left, right) + 1

        return height(root) != -1
```

## 🪄 Line-by-Line Explanation

- **`class Solution(object):`** — Create a blueprint named `Solution` to check if the tree is balanced.
- **`def isBalanced(self, root):`** — Entry method: receives the tree root and returns whether it's balanced.
- **`def height(node):`** — Helper that computes subtree height while checking balance simultaneously.
- **`if not node:`** — Base case: node doesn't exist (reached beyond a leaf).
  - `return 0` — Empty subtree has height zero.
- **`left = height(node.left)`** — Recursively get the height of the left subtree.
- **`if left == -1:`** — Check if the left subtree already reported being unbalanced.
  - `return -1` — Stop early; propagate failure signal upward.
- **`right = height(node.right)`** — Recursively get the height of the right subtree.
- **`if right == -1:`** — Check if the right subtree already reported being unbalanced.
  - `return -1` — Stop early; propagate failure signal upward.
- **`if abs(left - right) > 1:`** — Compare left and right heights; check if difference exceeds one.
  - `return -1` — Heights are too different; send failure signal.
- **`return max(left, right) + 1`** — Everything is fine; return the height of this subtree.
- **`return height(root) != -1`** — Start recursion at root; check that result is not the failure signal.

## 🌳 Why -1 is Used

The value `-1` acts as a **failure signal** that:
- Travels upward immediately when an imbalance is detected
- Stops unnecessary calculations early (preventing O(n²) work)
- Keeps the time complexity at O(n)

## ⏱️ Complexity

- **Time Complexity:** O(n) — every node is visited once.
- **Space Complexity:** O(h) — recursion stack depth equals tree height.