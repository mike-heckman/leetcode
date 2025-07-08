# Design Pattern: Hash Map / Set

[< Back](index.md)

The **Hash Map** (or Dictionary in Python) and **Hash Set** are data structures that provide average-case O(1) time complexity for insertions, deletions, and lookups. This incredible speed makes them a go-to tool for optimizing algorithms that would otherwise be much slower.

*   A **Hash Map** stores key-value pairs.
*   A **Hash Set** stores only unique keys.

This pattern revolves around using these structures to store and retrieve information about elements as you iterate through a collection, often avoiding nested loops and reducing time complexity from O(N^2) to O(N).

## When to Use the Hash Map / Set Pattern

This pattern is one of the most common and versatile in programming. Look for it when a problem involves:

*   **Counting frequencies** of elements in a collection (e.g., "find the most frequent element").
*   **Finding duplicates** or checking for the existence of an element in a collection.
*   **Grouping** items based on a certain property (e.g., "group anagrams").
*   **Caching/Memoization** to store the results of expensive computations.
*   Problems that can be optimized by "remembering" a piece of information seen earlier in an iteration. A classic example is finding two numbers that sum to a target.

The core idea is trading space for time. By using extra space for a hash map/set, you can achieve significantly faster lookups.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the number of elements in the input. This is because you typically iterate through the input once, and each hash map/set operation is O(1) on average.
*   **Space Complexity: O(K)**, where K is the number of unique items being stored in the hash map or set. In the worst case, this can be O(N) if all elements are unique.

---

## Sample Problem: Two Sum

**LeetCode Link:** [1. Two Sum](https://leetcode.com/problems/two-sum/)

### Problem Statement

Given an array of integers `nums` and an integer `target`, return indices of the two numbers such that they add up to `target`.

You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

**Example 1:**
Input: `nums = [2,7,11,15]`, `target = 9`
Output: `[0,1]`
Explanation: Because `nums[0] + nums[1] == 9`, we return `[0, 1]`.

**Example 2:**
Input: `nums = [3,2,4]`, `target = 6`
Output: `[1,2]`

### Solution

```python
from typing import List

class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # A hash map to store seen numbers and their indices: {number: index}
        num_map = {}

        for i, num in enumerate(nums):
            # Calculate the complement we need to find
            complement = target - num
            
            # Check if the complement exists in our map
            if complement in num_map:
                # If it exists, we've found our pair
                return [num_map[complement], i]
            
            # If the complement is not found, add the current number and its
            # index to the map for future lookups.
            num_map[num] = i
        
        # According to the problem statement, a solution always exists,
        # so this part of the code should not be reached.
        return []
```

---

## Detailed Code Analysis

This solution is a textbook example of using a hash map to optimize a search problem. A brute-force approach would use two nested loops, resulting in O(N^2) time complexity. This solution achieves O(N).

1.  **Initialization**:
    *   `num_map = {}`: An empty dictionary is created to serve as our hash map. It will store numbers we have already seen as keys and their corresponding indices as values.

2.  **Iterating Through the Array**:
    *   `for i, num in enumerate(nums):`: We use `enumerate` to get both the index (`i`) and the value (`num`) of each element in a single pass.

3.  **The Core Logic**:
    *   `complement = target - num`: For each number `num`, we calculate the value that would be needed to reach the `target`. This is the `complement`.
    *   `if complement in num_map:`: This is the key step. We perform an O(1) average-time lookup in our hash map to see if we have *already* encountered the complement.
    *   `return [num_map[complement], i]`: If the complement is in the map, we have found our solution. We return the index of the complement (which we stored in the map) and the index of the current number.

4.  **Populating the Map**:
    *   `num_map[num] = i`: If the complement was *not* found in the map, it means we haven't found a pair yet. We add the current number `num` and its index `i` to the map. This makes it available for subsequent iterations to use as a potential complement.

By building the map as we iterate, we ensure we only need one pass through the array. For any given number, we check if its partner has been seen *before* it. This is why we add the number to the map *after* the check, which also elegantly handles the constraint that we cannot use the same element twice.

[< Back](index.md)
