# Design Pattern: Linked List

[< Back](index.md)

A **Linked List** is a fundamental linear data structure where elements are not stored at contiguous memory locations. Instead, elements are stored in **nodes**, where each node contains its data and a pointer (or reference) to the next node in the sequence. This structure allows for highly efficient insertions and deletions at the cost of O(N) access time for a specific element.

Manipulating linked lists often involves careful pointer management. A common sub-pattern is the **Fast and Slow Pointer** technique, used for cycle detection, finding the middle of a list, and more.

## When to Use the Linked List Pattern

This pattern is essential for problems that require efficient modifications to a sequence. Look for it when a problem involves:

*   Reversing a list or a sub-list.
*   Merging two or more sorted lists.
*   Detecting cycles within a sequence.
*   Finding the Nth node from the end of a list.
*   Adding or removing elements from the beginning or end of a sequence in O(1) time.
*   Problems where you need to "rewire" parts of a sequence.

The key characteristic is the need to manipulate a sequence of elements where direct index-based access is not a priority, but structural changes (insertions, deletions, reordering) are.

### Typical Complexity

*   **Time Complexity: O(N)** for most problems, as they typically require at least one full traversal of the list.
*   **Space Complexity: O(1)** for iterative solutions that only require a few pointer variables. Recursive solutions may take O(N) space on the call stack in the worst case (a long, unbranched list).

---

## Sample Problem: Reverse Linked List

**LeetCode Link:** [206. Reverse Linked List](https://leetcode.com/problems/reverse-linked-list/)

### Problem Statement

Given the `head` of a singly linked list, reverse the list, and return the *reversed list's head*.

**Example 1:**
Input: `head = [1,2,3,4,5]`
Output: `[5,4,3,2,1]`

Visual Representation:
`1 -> 2 -> 3 -> 4 -> 5 -> NULL`
becomes
`5 -> 4 -> 3 -> 2 -> 1 -> NULL`

**Example 2:**
Input: `head = [1,2]`
Output: `[2,1]`

### Solution

```python
from typing import Optional

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        prev_node = None
        current_node = head

        # Traverse the list, reversing pointers as we go
        while current_node:
            # 1. Store the next node before we overwrite the pointer
            next_node_temp = current_node.next
            
            # 2. Reverse the current node's pointer
            current_node.next = prev_node
            
            # 3. Move the 'prev' and 'current' pointers one step forward
            prev_node = current_node
            current_node = next_node_temp
        
        # When the loop ends, 'prev_node' is the new head
        return prev_node
```

---

## Detailed Code Analysis

This iterative solution elegantly reverses the list by manipulating pointers in a single pass.

1.  **Initialization**:
    *   `prev_node = None`: This pointer will track the node that comes *before* the current one in the *new, reversed* list. It's initialized to `None` because the original head node will become the tail of the reversed list, and its `next` pointer should be `None`.
    *   `current_node = head`: This is our main traversal pointer. It starts at the beginning of the original list.

2.  **The Reversal Loop**:
    *   `while current_node:`: The loop continues as long as there are nodes left to process in the original list.

3.  **Pointer Manipulation (The Core Logic)**:
    The loop body performs the reversal in three critical steps, which must happen in this specific order:
    *   **`next_node_temp = current_node.next`**: Before we change any pointers, we must save a reference to the next node in the original list. If we don't, we'll lose the rest of the list as soon as we perform the next step.
    *   **`current_node.next = prev_node`**: This is the actual reversal. We take the `current_node` and make its `next` pointer point "backwards" to the `prev_node`. In the first iteration, this points the original head node to `None`.
    *   **`prev_node = current_node`** and **`current_node = next_node_temp`**: We advance our pointers. The `current_node` becomes the new `prev_node` for the next iteration, and we move `current_node` forward to the node we saved in `next_node_temp`.

4.  **Final Return**:
    *   `return prev_node`: The loop terminates when `current_node` becomes `None`, which means we've just processed the last node of the original list. At this point, `prev_node` is pointing to that last node, which is now the head of our newly reversed list.

This approach carefully walks through the list, rewiring one node at a time, using only a few extra pointers. This results in an optimal O(N) time and O(1) space solution.

[< Back](index.md)
