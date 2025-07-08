# Design Pattern: Queue

[< Back](index.md)

The **Queue** is a fundamental data structure that follows the **First-In, First-Out (FIFO)** principle. It's analogous to a line of people waiting for a service: the first person to get in line is the first person to be served. Elements are added to the back (an operation called `enqueue`) and removed from the front (`dequeue`). This behavior is perfect for processing elements in the order they are received or discovered.

## When to Use the Queue Pattern

This pattern is the cornerstone of the **Breadth-First Search (BFS)** algorithm and is ideal for problems that involve:

*   **Level-Order Traversal**: Visiting nodes in a tree or graph level by level.
*   **Shortest Path in Unweighted Graphs**: BFS is the standard algorithm for finding the shortest path between two nodes in a graph where all edge weights are equal.
*   **Task Scheduling**: Managing a sequence of tasks or requests that must be processed in the order they were received (e.g., a printer queue, a web server's request handler).
*   **Streaming Data**: Processing a stream of incoming data, such as calculating a moving average over the last N elements.

The key characteristic is the need to explore or process elements in layers or in their order of arrival.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the total number of elements or nodes to process. In most queue-based algorithms, each element is enqueued and dequeued exactly once.
*   **Space Complexity: O(W)**, where W is the maximum "width" of the data structure at any point. For a tree traversal, this is the maximum number of nodes in any single level. In the worst case (e.g., a complete binary tree), this can be O(N).

---

## Sample Problem: Binary Tree Level Order Traversal

**LeetCode Link:** [102. Binary Tree Level Order Traversal](https://leetcode.com/problems/binary-tree-level-order-traversal/)

### Problem Statement

Given the `root` of a binary tree, return the *level order traversal* of its nodes' values. (i.e., from left to right, level by level).

**Example 1:**
Input: `root = [3,9,20,null,null,15,7]`
Output: `[[3],[9,20],[15,7]]`
Visual Representation:
```
      3
     / \
    9  20
      /  \
     15   7
```

**Example 2:**
Input: `root = [1]`
Output: `[[1]]`

**Example 3:**
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

This solution is a classic implementation of Breadth-First Search (BFS) using a queue to achieve a level-by-level traversal.

1.  **Initialization**:
    *   `if not root: return []`: A base case to handle an empty tree.
    *   `result = []`: This list will store the list of values for each level.
    *   `queue = deque([root])`: We initialize the queue. In Python, `collections.deque` is highly optimized for fast appends and pops from both ends, making it ideal. We start by enqueuing the `root` node, which is the first and only node at level 0.

2.  **The Main Traversal Loop**:
    *   `while queue:`: The loop continues as long as there are nodes in the queue to be processed. When the queue is empty, it means we have visited every node in the tree.

3.  **Processing by Level**:
    *   `level_size = len(queue)`: This is a crucial step. Before processing a level, we take a "snapshot" of the current size of the queue. This tells us exactly how many nodes are on the current level.
    *   `current_level = []`: A temporary list to hold the values of the nodes for the current level.
    *   `for _ in range(level_size):`: This inner loop iterates exactly `level_size` times. This ensures that we only process the nodes that were present at the start of the level, and we don't mix them with their children (which belong to the next level).

4.  **Dequeue, Process, and Enqueue Children**:
    *   `node = queue.popleft()`: We remove the node from the *front* of the queue, adhering to the FIFO principle.
    *   `current_level.append(node.val)`: We process the node by adding its value to our list for the current level.
    *   `if node.left: queue.append(node.left)` and `if node.right: queue.append(node.right)`: We add the node's children (if they exist) to the *back* of the queue. These children will be processed in a future iteration of the main `while` loop, after all nodes on the current level are finished.

5.  **Final Result**:
    *   `result.append(current_level)`: Once the inner `for` loop completes, `current_level` contains all the node values for that level, which we append to our final `result`.

This approach guarantees that every node is visited once, and the queue's FIFO nature ensures the traversal happens one complete level at a time.

[< Back](index.md)
