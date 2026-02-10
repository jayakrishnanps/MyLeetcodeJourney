# 83. Remove Duplicates from Sorted List

## Problem Statement

Given the `head` of a **sorted linked list**, delete all duplicates such that each element appears **only once**.  
Return the linked list sorted as well.

---

## Examples

### Example 1
**Input**
head = [1,1,2]

**Output**
[1,2]


---

### Example 2
**Input**
head = [1,1,2,3,3]

**Output**
[1,2,3]


---

## Constraints
- The number of nodes in the list is in the range `[0, 300]`
- `-100 <= Node.val <= 100`
- The list is guaranteed to be sorted in ascending order

---

## Key Observation

Because the linked list is **sorted**, all duplicate values will appear **next to each other**.

This allows us to remove duplicates in **one pass** by comparing each node with its next node.

---

## Approach

1. Start from the head of the list.
2. Compare the current node’s value with the next node’s value.
3. If they are equal, skip the next node by changing the pointer.
4. If they are not equal, move forward to the next node.
5. Continue until the end of the list.

No new nodes are created and no values are copied.  
Only pointers are updated.

---

## Code (Accepted Solution)

```python
class Solution(object):
    def deleteDuplicates(self, head):
        current = head

        while current and current.next:
            if current.val == current.next.val:
                current.next = current.next.next
            else:
                current = current.next

        return head