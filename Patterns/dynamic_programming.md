# Design Pattern: Dynamic Programming (DP)

[< Back](index.md)

**Dynamic Programming (DP)** is a powerful algorithmic technique for solving complex problems by breaking them down into simpler, overlapping subproblems. The results of these subproblems are stored (or "memoized") to avoid redundant computations. For a problem to be solvable with DP, it must exhibit two key properties:

1.  **Overlapping Subproblems**: The algorithm solves the same subproblem multiple times.
2.  **Optimal Substructure**: The optimal solution to the overall problem can be constructed from the optimal solutions of its subproblems.

There are two main approaches to DP:
*   **Memoization (Top-Down)**: A recursive approach where you solve the main problem by breaking it down. You store the result of each subproblem in a cache (e.g., a hash map or array) the first time you compute it and return the cached result on subsequent calls.
*   **Tabulation (Bottom-Up)**: An iterative approach where you solve the smallest possible subproblems first and build up to the final solution. This is often done by filling out a DP table (usually an array or a 2D grid).

## When to Use the Dynamic Programming Pattern

This pattern is the go-to solution for a wide range of optimization and counting problems. Look for it when the problem asks:

*   For the **maximum/minimum** value of something (e.g., max profit, min cost path).
*   To find the **number of ways** to do something.
*   To determine if a solution is **possible** under certain constraints.
*   When a brute-force recursive solution would be too slow due to re-computing the same subproblems over and over.

Classic examples include the Knapsack problem, Longest Common Subsequence, and pathfinding in a grid.

### Typical Complexity

*   **Time Complexity: Typically polynomial**, such as O(N), O(N*M), or O(N^2). This is a massive improvement over the exponential complexity of a naive recursive solution.
*   **Space Complexity: O(N) or O(N*M)** to store the DP table or cache. This can often be optimized if the solution for a state only depends on a few previous states.

---

## Sample Problem: Climbing Stairs

**LeetCode Link:** [70. Climbing Stairs](https://leetcode.com/problems/climbing-stairs/)

### Problem Statement

You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?

**Example 1:**
Input: `n = 2`
Output: `2`
Explanation: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

**Example 2:**
Input: `n = 3`
Output: `3`
Explanation: There are three ways to climb to the top.
1. 1 step + 1 step + 1 step
2. 1 step + 2 steps
3. 2 steps + 1 step

### Solution (Bottom-Up with Space Optimization)

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        # Base cases for the first two steps
        if n <= 2:
            return n

        # We only need to store the results for the previous two steps.
        # This is a space-optimized tabulation approach.
        two_steps_back = 1  # Ways to reach step 1
        one_step_back = 2   # Ways to reach step 2

        # Iterate from the 3rd step up to n
        for _ in range(3, n + 1):
            # The number of ways to reach the current step is the sum of the ways
            # to reach the previous two steps.
            current_ways = one_step_back + two_steps_back
            
            # Update the pointers for the next iteration
            two_steps_back = one_step_back
            one_step_back = current_ways
            
        return one_step_back
```

---

## Detailed Code Analysis

This problem is a perfect illustration of DP because the number of ways to reach step `i` is the sum of the ways to reach step `i-1` (and taking one step) and the ways to reach step `i-2` (and taking two steps). This defines our **recurrence relation**: `ways(i) = ways(i-1) + ways(i-2)`. This is the Fibonacci sequence!

1.  **Base Cases**:
    *   `if n <= 2: return n`: We handle the first two steps directly. There is 1 way to reach step 1, and 2 ways to reach step 2. This stops the logic from running for trivial inputs.

2.  **Initialization (Space-Optimized Tabulation)**:
    *   A full tabulation solution would use an array `dp` of size `n+1`. However, we can see that `ways(i)` only depends on `ways(i-1)` and `ways(i-2)`. We don't need the whole array.
    *   `two_steps_back = 1`: Stores the number of ways to reach step `i-2`. We initialize it to the ways to reach step 1.
    *   `one_step_back = 2`: Stores the number of ways to reach step `i-1`. We initialize it to the ways to reach step 2.

3.  **The Loop (Building the Solution)**:
    *   `for _ in range(3, n + 1):`: We iterate from the 3rd step up to our target `n`.
    *   `current_ways = one_step_back + two_steps_back`: We apply the recurrence relation to find the number of ways to reach the current step.
    *   `two_steps_back = one_step_back`: We update our pointers. The previous `one_step_back` now becomes the new `two_steps_back` for the next iteration.
    *   `one_step_back = current_ways`: The `current_ways` we just calculated becomes the new `one_step_back` for the next iteration.

4.  **Final Return**:
    *   `return one_step_back`: When the loop finishes, `one_step_back` holds the number of ways to reach the final step, `n`.

This bottom-up approach iteratively builds the solution from the base cases, efficiently calculating the result in O(N) time. By only storing the last two results, we optimize the space complexity from O(N) for a full DP table down to O(1).

[< Back](index.md)
