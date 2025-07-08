# Design Pattern: Tree BST (Binary Search Tree)

[< Back](index.md)

A **Binary Search Tree (BST)** is a special type of binary tree that satisfies the BST property: for any given node, all values in its left subtree are less than the node's value, and all values in its right subtree are greater than the node's value. This property must hold true for all nodes in the tree.

This ordering allows for highly efficient searching, insertion, and deletion operations, as it enables us to discard half of the remaining tree at each step, similar to a binary search on an array.

## When to Use the Tree BST Pattern

This pattern is the go-to solution for problems that require maintaining a sorted set of data that can be modified efficiently. Look for it when the problem involves:

*   **Searching for an element** in a collection that can be modeled as a sorted tree.
*   **Validating a BST**: Checking if a given tree adheres to the BST properties.
*   **Inserting or deleting elements** while maintaining a sorted order.
*   Finding the **k-th smallest/largest element** in a set of values.
*   Finding the **successor or predecessor** of a node (the next smallest or largest value).
*   An in-order traversal of a BST will always yield its elements in sorted order.

### Typical Complexity

The complexity of BST operations depends on the **height (H)** of the tree.
*   **Time Complexity: O(H)**. For a balanced BST, the height is `log N`, leading to O(log N) time. For a completely unbalanced (skewed) tree, the height is `N`, leading to a worst-case time of O(N).
*   **Space Complexity: O(H)** for recursive solutions due to the call stack. Iterative solutions can achieve O(1) space.

---

## Sample Problem: Search in a Binary Search Tree

**LeetCode Link:** 700. Search in a Binary Search Tree

### Problem Statement

You are given the `root` of a binary search tree (BST) and an integer `val`.

Find the node in the BST that the node's value equals `val` and return the subtree rooted with that node. If such a node does not exist, return `null`.

**Example 1:**
Input: `root = [4,2,7,1,3]`, `val = 2`
Output: `[2,1,3]` (The subtree rooted at the node with value 2)

**Example 2:**
Input: `root = [4,2,7,1,3]`, `val = 5`
Output: `[]` (An empty array representing null)

### Solution (Recursive)

```python
from typing import Optional

# Definition for a binary tree node.
class TreeNode:
    def __init__(self, val=0, left=None, right=None):
        self.val = val
        self.left = left
        self.right = right

class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # Base Case 1: The tree is empty or we've reached a null child.
        if not root:
            return None
        
        # Base Case 2: We found the node with the target value.
        if root.val == val:
            return root

        # Recursive Step: Decide whether to go left or right.
        if val < root.val:
            # The value must be in the left subtree.
            return self.searchBST(root.left, val)
        else: # val > root.val
            # The value must be in the right subtree.
            return self.searchBST(root.right, val)
```

---

## Detailed Code Analysis

The recursive solution elegantly leverages the BST property to quickly narrow down the search space.

1.  **Base Cases**: The recursion needs stopping conditions.
    *   `if not root:`: If the current node is `None`, it means we've either started with an empty tree or we've searched down a path and didn't find the value. In this case, the value does not exist, so we return `None`.
    *   `if root.val == val:`: If the current node's value is the one we're looking for, we've found our target. We return the `root` node itself, which is the head of the desired subtree.

2.  **Recursive Step (The Core Logic)**:
    *   This is where the BST property is used to make an intelligent decision. We compare the target `val` with the current `root.val`.
    *   `if val < root.val:`: If the target value is less than the current node's value, we know that if the value exists, it **must** be in the left subtree. We can completely ignore the right subtree. We make a recursive call on `root.left`.
    *   `else:`: If the target value is greater than the current node's value, we know it **must** be in the right subtree. We make a recursive call on `root.right`.

3.  **Final Return**:
    *   The function returns the result of the recursive calls. This chain of returns will either pass the found `node` all the way back up to the initial call, or it will pass `None` back up if the value is never found.

This approach effectively performs a binary search on the tree, discarding half of the remaining nodes at each level, leading to an efficient O(H) time complexity.

[< Back](index.md)
