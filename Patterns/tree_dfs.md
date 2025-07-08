# Design Pattern: Tree DFS (Depth-First Search)

[< Back](index.md)

The **Tree Depth-First Search (DFS)** pattern is a fundamental traversal algorithm that explores as far as possible along each branch before backtracking. Unlike BFS which explores level by level, DFS goes deep first. It's naturally implemented using **recursion** (which uses the system's call stack) or an explicit **stack**. The three main variations of DFS are pre-order, in-order, and post-order traversal.

## When to Use the Tree DFS Pattern

This pattern is ideal for problems where you need to explore a tree's structure exhaustively down each path. Look for it when the problem asks to:

*   Find if a path exists from the root to a certain node that meets some criteria (e.g., "Path Sum").
*   Find the maximum depth or diameter of a tree.
*   Perform pre-order, in-order, or post-order traversals.
*   Solve problems where you need to consider a node and its descendants before moving to a sibling node.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the number of nodes in the tree. Each node is visited exactly once.
*   **Space Complexity: O(H)**, where H is the height of the tree. This space is used by the recursion call stack. In the worst case of a completely unbalanced (skewed) tree, the height can be N, making the space complexity O(N). For a balanced tree, it's O(log N).

---

## Sample Problem: Maximum Depth of Binary Tree

**LeetCode Link:** [104. Maximum Depth of Binary Tree](https://leetcode.com/problems/maximum-depth-of-binary-tree/)

### Problem Statement

Given the `root` of a binary tree, return its maximum depth.

A binary tree's maximum depth is the number of nodes along the longest path from the root node down to the farthest leaf node.

**Example 1:**
Input: `root = [3,9,20,null,null,15,7]`
Output: `3`

**Example 2:**
Input: `root = [1,null,2]`
Output: `2`

### Solution

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # Base case: If the node is None, its depth is 0.
        if not root:
            return 0
        
        # Recursively find the depth of the left and right subtrees.
        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)
        
        # The depth of the tree is 1 (for the current node) plus the
        # maximum of the depths of its left and right subtrees.
        return 1 + max(left_depth, right_depth)
```

---

## Detailed Code Analysis

This solution is a classic, elegant example of solving a tree problem with recursive DFS.

1.  **Base Case**:
    *   `if not root: return 0`: This is the most important part of any recursive function. It defines the stopping condition. When we reach a `None` node (i.e., the child of a leaf node), we have gone past the end of a path. The depth at this "null" spot is 0. This allows the recursion to unwind.

2.  **Recursive Step (The Core Logic)**:
    *   `left_depth = self.maxDepth(root.left)`: The function calls itself on the left child. This is the "depth-first" part. The program will continue to call `maxDepth` on the left children until it hits a `None` node and the base case returns 0.
    *   `right_depth = self.maxDepth(root.right)`: After the entire left subtree has been explored and its depth calculated and returned, the function does the same for the right subtree.
    *   The call stack manages the process of "backtracking." When `maxDepth(root.left)` returns a value, the execution resumes at that point in the parent's function call.

3.  **Combining Results**:
    *   `return 1 + max(left_depth, right_depth)`: Once the depths of both the left and right subtrees are known, the depth of the tree rooted at the `current` node is `1` (for the current node itself) plus the greater of the two subtree depths. This result is then returned up the call stack to the previous caller.

4.  **Final Return**:
    *   The process continues until all recursive calls have returned, and the final result from the initial call on the original `root` is the maximum depth of the entire tree.

This approach perfectly mirrors the definition of a tree's depth, breaking the problem down into smaller, identical subproblems.

[< Back](index.md)
