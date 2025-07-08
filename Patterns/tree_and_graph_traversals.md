# Design Pattern: Tree & Graph Traversals (DFS & BFS)

[< Back](index.md)

**Tree and Graph Traversals** are fundamental algorithms for systematically visiting every node in a tree or graph structure. The two primary methods are **Depth-First Search (DFS)** and **Breadth-First Search (BFS)**. The choice between them depends entirely on the problem you are trying to solve.

*   **Depth-First Search (DFS):** Explores as far as possible down one branch before backtracking. It's like navigating a maze by taking one path to its end, and if it's a dead end, you backtrack to the last junction and try a different path. It is typically implemented using recursion (which uses the call stack) or an explicit stack.

*   **Breadth-First Search (BFS):** Explores all neighbor nodes at the present "level" before moving on to the nodes at the next level. It's like ripples spreading in a pond. It is implemented using a queue.

## When to Use These Traversal Patterns

### Use Depth-First Search (DFS) when:
*   The problem involves finding if a **path** exists between two nodes.
*   You need to explore a full branch to its conclusion before trying others (e.g., solving a maze, backtracking problems like Sudoku).
*   The problem involves finding **connected components** in a graph (as in the example below).
*   You need to perform a **topological sort** on a Directed Acyclic Graph (DAG).
*   You are short on memory, as DFS's space complexity (O(H)) can be much lower than BFS's if the graph is wide and not deep.

### Use Breadth-First Search (BFS) when:
*   The problem involves finding the **shortest path** in an **unweighted** graph.
*   You need to perform a **level-order traversal** of a tree.
*   You need to explore nodes in concentric layers from a starting point.

### Typical Complexity

For a graph with V vertices and E edges:
*   **Time Complexity: O(V + E)**, because in the worst case, every vertex and every edge will be visited exactly once. For a tree, this simplifies to O(N) where N is the number of nodes.
*   **Space Complexity:**
    *   **DFS:** O(H), where H is the maximum height of the tree or the longest path in the graph. This space is used by the recursion call stack. In the worst case (a skewed tree/graph), this can be O(V).
    *   **BFS:** O(W), where W is the maximum width of the tree or graph. This space is used by the queue. In the worst case (a complete binary tree or a dense graph), this can be O(V).

---

## Sample Problem: Number of Provinces

**LeetCode Link:** [547. Number of Provinces](https://leetcode.com/problems/number-of-provinces/)

### Problem Statement

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `i`th city and the `j`th city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return the total number of **provinces**.

**Example 1:**
Input: `isConnected = [[1,1,0],[1,1,0],[0,0,1]]`
Output: `2`

### Solution (Using DFS)

```python
from typing import List

class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        visited = set()
        provinces = 0

        def dfs(city):
            # Mark the current city as visited
            visited.add(city)
            # Visit all connected neighbors
            for neighbor in range(n):
                if isConnected[city][neighbor] == 1 and neighbor not in visited:
                    dfs(neighbor)

        # Iterate through all cities to find unvisited ones
        for city in range(n):
            if city not in visited:
                # This city is part of a new, undiscovered province.
                # Start a DFS from here to find all cities in this province.
                provinces += 1
                dfs(city)
        
        return provinces
```

---

## Detailed Code Analysis

This problem is a classic "find the number of connected components in a graph" problem, which is a perfect use case for DFS.

1.  **Initialization**:
    *   `n = len(isConnected)`: Get the total number of cities (nodes).
    *   `visited = set()`: A hash set is used to keep track of the cities we have already visited. This is crucial to prevent infinite loops in graphs with cycles and to avoid re-processing nodes. Lookups and insertions are O(1).
    *   `provinces = 0`: This counter will be incremented each time we discover a new, distinct group of connected cities.

2.  **The `dfs` Helper Function**:
    *   This function takes a `city` as input and performs a depth-first search starting from it.
    *   `visited.add(city)`: The first thing it does is mark the starting city as visited.
    *   `for neighbor in range(n):`: It then iterates through all possible neighbors of the current city.
    *   `if isConnected[city][neighbor] == 1 and neighbor not in visited:`: It checks two conditions:
        1.  Is there a connection (an edge) to this `neighbor`?
        2.  Have we already visited this `neighbor` as part of the current or a previous traversal?
    *   `dfs(neighbor)`: If both conditions are met, it makes a recursive call to `dfs` on the unvisited neighbor, continuing the exploration down that path.

3.  **The Main Loop**:
    *   `for city in range(n):`: The main part of the function iterates through every city from `0` to `n-1`.
    *   `if city not in visited:`: This is the key to counting the provinces. If we encounter a city that hasn't been visited yet, it means we have found the first city of a new, undiscovered province.
    *   `provinces += 1`: We increment our province counter.
    *   `dfs(city)`: We then launch a DFS starting from this city. The `dfs` function will recursively visit and mark every single city that is reachable from this starting point, effectively "coloring" the entire province.

4.  **Final Return**:
    *   `return provinces`: After the main loop has checked every city, the `provinces` variable will hold the total count of the distinct connected components we found.

This approach ensures that every city is visited exactly once. The main loop finds the starting points of new provinces, and the DFS traversal explores and marks all members of that province.

[< Back](index.md)
