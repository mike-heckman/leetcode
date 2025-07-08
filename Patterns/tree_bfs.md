# Design Pattern: Tree BFS (Breadth-First Search)

[< Back](index.md)

The **Tree Breadth-First Search (BFS)** pattern is a fundamental traversal algorithm that explores a tree level by level. It starts at the root and explores all the neighbor nodes at the present depth prior to moving on to the nodes at the next depth level. This level-by-level traversal is achieved by using a **Queue** data structure.

## When to Use the Tree BFS Pattern

This pattern is the go-to solution for any problem that involves traversing a tree in a level-by-level fashion. Look for it when the problem asks to:

*   Perform a level-order traversal (as in the sample problem).
*   Find the shortest path from the root to a target node in a tree.
*   Solve problems that involve processing nodes layer by layer, such as finding the "right side view" of a tree, connecting nodes at the same level, or finding the minimum depth.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the number of nodes in the tree. Each node is enqueued and dequeued exactly once.
*   **Space Complexity: O(W)**, where W is the maximum width or number of nodes in any single level of the tree. This is the maximum space the queue will occupy. In the worst case (a complete, balanced binary tree), the last level can contain up to N/2 nodes, making the space complexity O(N).

---

## Sample Problem: Binary Tree Level Order Traversal

**LeetCode Link:** [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Problem Statement

Given the `root` of a binary tree, return the *level order traversal* of its nodes' values. (i.e., from left to right, level by level).

**Example 1:**
Input: `root = [3,9,20,null,null,15,7]`
Output: `[[3],[9,20],[15,7]]`

**Example 2:**
Input: `root = []`
Output: `[]`

### Solution

```python
from collections import deque
from typing import List, Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []

        result = []
        # Use collections.deque for an efficient O(1) queue implementation
        queue = deque([root])

        while queue:
            # Get the number of nodes at the current level
            level_size = len(queue)
            current_level = []

            # Process all nodes for the current level
            for _ in range(level_size):
                # Dequeue the node from the front of the queue (FIFO)
                node = queue.popleft()
                current_level.append(node.val)

                # Enqueue its children to be processed in the next level
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            result.append(current_level)
            
        return result
```

---

## Detailed Code Analysis

This solution is a classic implementation of the BFS pattern for trees.

1.  **Initialization**:
    *   An empty `result` list is created to store the lists of node values for each level.
    *   A `queue` (using `collections.deque` for efficiency) is initialized and the `root` node is added to it.

2.  **The Traversal Loop**:
    *   `while queue:`: The loop continues as long as there are nodes to process.
    *   `level_size = len(queue)`: This is a crucial step. We take a "snapshot" of the queue's size. This tells us exactly how many nodes are on the current level.
    *   `for _ in range(level_size):`: This inner loop iterates exactly `level_size` times, ensuring we only process the nodes for the current level.

3.  **Dequeue, Process, and Enqueue Children**:
    *   `node = queue.popleft()`: We remove a node from the front of the queue.
    *   `current_level.append(node.val)`: We process the node by adding its value to the list for the current level.
    *   `if node.left:` and `if node.right:`: We add the node's children (if they exist) to the back of the queue. These children will be processed in a future iteration of the main `while` loop.

4.  **Final Result**:
    *   `result.append(current_level)`: Once the inner `for` loop completes, the `current_level` list is complete, and we add it to our final `result`.

[< Back](index.md)
