# Design Pattern: Fast and Slow Pointers

[< Back](index.md)

The **Fast and Slow Pointers** pattern, also known as the "tortoise and hare" algorithm, is a powerful technique that uses two pointers moving through a sequence (usually a linked list) at different speeds. The "slow" pointer typically moves one step at a time, while the "fast" pointer moves two steps. This difference in speed inevitably leads to them either meeting at a certain point or one reaching the end before the other, which can reveal important properties about the data structure.

## When to Use the Fast and Slow Pointers Pattern

This pattern is almost exclusively used for problems involving **linked lists**. It's the go-to solution for:

*   Detecting a cycle in a linked list.
*   Finding the middle of a linked list.
*   Finding the start of a cycle in a linked list.
*   Problems involving finding the Nth node from the end of a list.

The key characteristic is the need to find a specific position or detect a structural property within a linked list without knowing its length beforehand.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the number of nodes in the linked list. Although the fast pointer moves faster, both pointers traverse the list at most once.
*   **Space Complexity: O(1)**, as it only requires two pointer variables.

---

## Sample Problem: Middle of the Linked List

**LeetCode Link:** [876. Middle of the Linked List](https://leetcode.com/problems/middle-of-the-linked-list/)

### Problem Statement

Given the `head` of a singly linked list, return *the middle node of the linked list*.

If there are two middle nodes, return the second middle node.

**Example 1:**
Input: `head = [1,2,3,4,5]`
Output: `Node 3` from the list
Explanation: The middle node of the list is node 3.

**Example 2:**
Input: `head = [1,2,3,4,5,6]`
Output: `Node 4` from the list
Explanation: There are two middle nodes, 3 and 4. We return the second one.

### Solution

```python
from typing import Optional

# Definition for singly-linked list.
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

class Solution:
    def middleNode(self, head: Optional[ListNode]) -> Optional[ListNode]:
        slow = head
        fast = head

        # The loop continues as long as the fast pointer is not at the end
        while fast and fast.next:
            slow = slow.next
            fast = fast.next.next
        
        # When the fast pointer reaches the end, the slow pointer is at the middle
        return slow
```

---

## Detailed Code Analysis

The provided solution is a classic application of the fast and slow pointer technique.

1.  **Initialization**:
    *   `slow = head` and `fast = head`: Two pointers are initialized to point to the start of the linked list.

2.  **The Loop**:
    *   `while fast and fast.next:`: The loop continues as long as the `fast` pointer and the node *after* it are not `None`. This condition is crucial because it gracefully handles both even and odd length lists and prevents an error when trying to access `fast.next.next`.
        *   If the list has an odd number of nodes, `fast` will end up on the very last node, and `fast.next` will be `None`, terminating the loop.
        *   If the list has an even number of nodes, `fast` will end up on the second-to-last node, and then `fast.next.next` will move it to `None`, terminating the loop in the next check.

3.  **Moving the Pointers (The Core Logic)**:
    *   `slow = slow.next`: The slow pointer advances one step.
    *   `fast = fast.next.next`: The fast pointer advances two steps.
    *   Because the fast pointer moves twice as fast as the slow pointer, by the time the fast pointer has reached the end of the list, the slow pointer will be positioned exactly at the middle.

4.  **Final Return**:
    *   `return slow`: When the loop terminates, the `slow` pointer is at the desired middle node.
        *   For a list like `[1,2,3,4,5]`, `fast` ends on node `5`, and `slow` ends on node `3`.
        *   For a list like `[1,2,3,4,5,6]`, `fast` becomes `None` (after being on `5`), and `slow` ends on node `4`, which is the correct second middle node.

This approach efficiently finds the middle in a single pass, resulting in an optimal O(N) time and O(1) space solution.

[< Back](index.md)
