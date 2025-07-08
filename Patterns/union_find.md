# Design Pattern: Union-Find (Disjoint Set Union)

[< Back](index.md)

The **Union-Find** (or **Disjoint Set Union - DSU**) data structure is a powerful tool for keeping track of a set of elements partitioned into a number of disjoint (non-overlapping) subsets. It provides two primary operations:

1.  **`find(i)`**: Determines the representative (or root) of the set that element `i` belongs to.
2.  **`union(i, j)`**: Merges the two sets containing elements `i` and `j` into a single set.

The structure is incredibly fast when implemented with two key optimizations: **Path Compression** and **Union by Rank/Size**.

## When to Use the Union-Find Pattern

This pattern is the go-to solution for dynamic connectivity problems. It often provides a more efficient alternative to graph traversal (DFS/BFS) for specific tasks. Look for it when the problem involves:

*   **Finding connected components** in a graph (as in the sample problem).
*   **Detecting cycles** in an undirected graph.
*   Determining if a connection between two nodes is redundant.
*   Problems where you need to efficiently group items and check if two items belong to the same group.

### Typical Complexity

With both path compression and union by rank/size optimizations:
*   **Time Complexity: Nearly constant time on average for each operation**, often expressed as O(α(N)), where α(N) is the inverse Ackermann function. This function grows so slowly that it's less than 5 for any practical value of N, making the operations virtually O(1) amortized time.
*   **Space Complexity: O(N)** to store the parent and rank/size arrays for N elements.

---

## Sample Problem: Number of Provinces

**LeetCode Link:** 547. Number of Provinces

### Problem Statement

There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `i`th city and the `j`th city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return the total number of **provinces**.

### Solution

```python
from typing import List

class Solution:
    def findCircleNum(self, isConnected: List[List[int]]) -> int:
        n = len(isConnected)
        parent = list(range(n))
        rank = [1] * n
        provinces = n

        def find(i):
            # Find the root of the set for element i
            if parent[i] == i:
                return i
            # Path Compression: Set the parent directly to the root
            parent[i] = find(parent[i])
            return parent[i]

        def union(i, j):
            nonlocal provinces
            root_i = find(i)
            root_j = find(j)

            if root_i != root_j:
                # Union by Rank: Attach smaller tree to the root of the larger tree
                if rank[root_i] < rank[root_j]:
                    parent[root_i] = root_j
                elif rank[root_i] > rank[root_j]:
                    parent[root_j] = root_i
                else:
                    parent[root_j] = root_i
                    rank[root_i] += 1
                
                # A successful union means one less province
                provinces -= 1

        # Iterate through the adjacency matrix to union connected cities
        for i in range(n):
            for j in range(i + 1, n):
                if isConnected[i][j] == 1:
                    union(i, j)
        
        return provinces
```

---

## Detailed Code Analysis

This solution models the problem as finding the number of disjoint sets among the cities.

1.  **Initialization**:
    *   `parent = list(range(n))`: This array represents the "forest" of sets. Initially, each city `i` is its own parent, meaning there are `n` distinct sets. `parent[i] = i`.
    *   `rank = [1] * n`: This array is used for the "union by rank" optimization. It tracks the height (or rank) of each tree in our forest.
    *   `provinces = n`: We start by assuming every city is its own province. This count will decrease each time we successfully merge two previously disconnected sets.

2.  **The `find(i)` Function**:
    *   This function is the heart of the DSU. It finds the ultimate representative (root) of the set containing element `i`.
    *   The recursion continues up the parent chain until it finds a node that is its own parent.
    *   `parent[i] = find(parent[i])`: This is the **path compression** optimization. As the recursion unwinds, it sets the parent of every node along the path directly to the root. This dramatically flattens the tree, making future `find` calls for these nodes extremely fast.

3.  **The `union(i, j)` Function**:
    *   This function merges the sets containing `i` and `j`.
    *   It first finds the roots of both elements. If the roots are the same (`root_i == root_j`), the elements are already in the same set, and we do nothing.
    *   If the roots are different, we perform the merge. The **union by rank** logic ensures we keep our trees as flat as possible by always attaching the root of the shorter tree to the root of the taller tree.
    *   `provinces -= 1`: Crucially, every time we merge two distinct sets, we reduce the total number of provinces by one.

4.  **Main Logic**:
    *   We iterate through the `isConnected` matrix. We only need to check the upper triangle (`j` from `i + 1`) to avoid processing connections twice.
    *   `if isConnected[i][j] == 1:`: If two cities are directly connected, we call `union(i, j)` to merge their sets. The `union` function handles the logic of checking if they are already connected and updating the province count.

5.  **Final Return**:
    *   `return provinces`: After checking all connections, the `provinces` variable holds the final count of disjoint sets, which is our answer.

This Union-Find approach is highly efficient and is a powerful pattern to recognize for problems involving connectivity and grouping.

[< Back](index.md)
