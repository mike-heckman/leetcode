[< Back](index.md)

# Design Pattern: Backtracking

**Backtracking** is a general algorithmic technique for solving problems recursively by trying to build a solution incrementally, one piece at a time, and removing those solutions that fail to satisfy the constraints of the problem at any point in time (by "backtracking").

It is a refined brute-force approach that systematically explores a set of all possible solutions. It does this by building a "search tree" of choices. When a path in the tree leads to a dead end (a state that cannot result in a valid solution), the algorithm backtracks to the previous decision point and explores a different path.

## When to Use the Backtracking Pattern

This pattern is the go-to solution for problems that ask for all possible solutions, or a single valid solution, from a large set of candidates. It's particularly well-suited for constraint satisfaction problems. Look for it when the problem involves:

*   Generating all permutations, combinations, or subsets (e.g., Subsets, Permutations).
*   Finding a path in a grid or maze (e.g., Word Search, Rat in a Maze).
*   Solving puzzles with constraints (e.g., Sudoku Solver, N-Queens).
*   Problems where you need to make a sequence of choices, and each choice constrains the subsequent ones.

The core idea is to **choose, explore, and un-choose**.

### Typical Complexity

*   **Time Complexity: Often exponential**, such as O(N * C^L) or O(N!), where N is the number of starting points, C is the number of choices at each step, and L is the length of the solution path. This is because the algorithm may need to explore a large portion of the search space.
*   **Space Complexity: O(L)**, where L is the maximum depth of the recursion. This space is used by the call stack.

---

## Sample Problem: Word Search

**LeetCode Link:** [79. Word Search](https://leetcode.com/problems/word-search/)

### Problem Statement

Given an `m x n` grid of characters `board` and a string `word`, return `true` if `word` exists in the grid.

The word can be constructed from letters of sequentially adjacent cells, where "adjacent" cells are horizontally or vertically neighboring. The same letter cell may not be used more than once in a single word.

**Example 1:**
Input: `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, `word = "ABCCED"`
Output: `true`

**Example 2:**
Input: `board = [["A","B","C","E"],["S","F","C","S"],["A","D","E","E"]]`, `word = "SEE"`
Output: `true`

### Solution

```python
from typing import List

class Solution:
    def exist(self, board: List[List[str]], word: str) -> bool:
        rows, cols = len(board), len(board[0])

        def backtrack(r, c, index):
            # Base Case 1: We have found the entire word.
            if index == len(word):
                return True

            # Base Case 2: Check for out-of-bounds or character mismatch.
            if not (0 <= r < rows and 0 <= c < cols) or board[r][c] != word[index]:
                return False

            # --- The Backtracking Core ---
            # 1. Choose: Mark the current path to avoid reuse.
            temp, board[r][c] = board[r][c], '#'
            
            # 2. Explore: Recursively check all 4 directions.
            found = (backtrack(r + 1, c, index + 1) or
                     backtrack(r - 1, c, index + 1) or
                     backtrack(r, c + 1, index + 1) or
                     backtrack(r, c - 1, index + 1))
            
            # 3. Un-choose: Restore the board for other paths.
            board[r][c] = temp
            
            return found

        # Iterate through every cell as a potential starting point
        for r in range(rows):
            for c in range(cols):
                if backtrack(r, c, 0):
                    return True
        
        return False
```

---

## Detailed Code Analysis

This solution uses a recursive helper function, `backtrack`, to explore paths starting from each cell.

1.  **Main Function `exist`**:
    *   It gets the dimensions of the board.
    *   It then iterates through every cell `(r, c)` in the grid. For each cell, it initiates a backtracking search by calling `backtrack(r, c, 0)`, starting with the first character of the `word` (index 0).
    *   If any of these initial calls return `True`, it means a path was found, and the function immediately returns `True`.
    *   If the loops complete without finding the word, it returns `False`.

2.  **Recursive Helper `backtrack(r, c, index)`**:
    *   **Base Cases**: The function first checks for termination conditions.
        *   If `index == len(word)`, it means we have successfully matched all characters of the word in sequence, so we return `True`.
        *   It also checks for invalid states: if the current cell `(r, c)` is outside the grid boundaries, or if the character in `board[r][c]` does not match the character we are currently looking for (`word[index]`). In these cases, this path is a dead end, so we return `False`.

3.  **The "Choose, Explore, Un-choose" Logic**:
    *   **Choose**: `temp, board[r][c] = board[r][c], '#'`. This is the crucial step where we "make a choice." We temporarily modify the board at the current cell to a special character (`#`) to mark it as visited for the *current recursive path*. This prevents us from using the same letter cell more than once. We save the original character in `temp`.
    *   **Explore**: `found = (backtrack(...) or ...)`: We recursively call `backtrack` for all four adjacent cells (down, up, right, left), advancing the word index by one (`index + 1`). The `or` short-circuits, so as soon as any direction finds a valid path, the exploration stops and `found` becomes `True`.
    *   **Un-choose**: `board[r][c] = temp`. This is the "backtracking" step. After exploring all possibilities from the current cell, we **must** restore the cell to its original state. This is critical because it allows the cell to be used in other, completely different paths that might start from a different initial cell.

4.  **Return Value**: The function returns `found`, which indicates whether a valid path was discovered from the current cell `(r, c)`.

This systematic exploration guarantees that all possible paths are checked, and the marking/un-marking ensures that the constraints of the problem are always met.

[< Back](index.md)
