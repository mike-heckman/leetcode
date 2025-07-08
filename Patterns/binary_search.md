# Design Pattern: Binary Search

[< Back](index.md)

**Binary Search** is a highly efficient divide-and-conquer search algorithm. It works by repeatedly dividing a **sorted** collection in half. If the value of the search key is less than the item in the middle of the interval, the search is narrowed to the lower half. Otherwise, it's narrowed to the upper half. This process is repeated until the value is found or the interval is empty.

## When to Use the Binary Search Pattern

This pattern is the go-to solution for searching within a sorted, one-dimensional collection. It can also be adapted for more complex problems where the "search space" itself is monotonic. Look for it when:

*   You need to find an element in a **sorted array**.
*   The problem asks to find the minimum or maximum value that satisfies a certain condition. This is a more advanced variant called **Binary Search on the Answer**, where you search for the answer itself within a range of possibilities.
*   You can determine whether to discard the left or right half of the search space with a single check.

**Crucial Prerequisite:** The data or search space must be sorted or have a monotonic property.

### Typical Complexity

*   **Time Complexity: O(log N)**, where N is the number of elements in the search space. This is because the search space is halved in each iteration.
*   **Space Complexity: O(1)** for an iterative implementation.

---

## Sample Problem: Binary Search

**LeetCode Link:** [704. Binary Search](https://leetcode.com/problems/binary-search/)

### Problem Statement

Given an array of integers `nums` which is sorted in ascending order, and an integer `target`, write a function to search for `target` in `nums`. If `target` exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

**Example 1:**
Input: `nums = [-1,0,3,5,9,12]`, `target = 9`
Output: `4`
Explanation: 9 exists in nums and its index is 4

**Example 2:**
Input: `nums = [-1,0,3,5,9,12]`, `target = 2`
Output: `-1`
Explanation: 2 does not exist in nums so return -1

### Solution

```python
from typing import List

class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            # Calculate the middle index to avoid potential overflow
            mid = left + (right - left) // 2

            if nums[mid] == target:
                return mid
            elif nums[mid] < target:
                # The target must be in the right half
                left = mid + 1
            else: # nums[mid] > target
                # The target must be in the left half
                right = mid - 1
        
        # If the loop finishes, the target was not found
        return -1
```

---

## Detailed Code Analysis

This iterative solution is a textbook implementation of the binary search algorithm.

1.  **Initialization**:
    *   `left, right = 0, len(nums) - 1`: Two pointers, `left` and `right`, are initialized to define the boundaries of our search space. `left` starts at the first index (0), and `right` starts at the last index.

2.  **The Loop**:
    *   `while left <= right:`: The loop continues as long as our search space is valid (i.e., the left pointer has not crossed the right pointer). The `<=` is important to correctly handle the case where the search space shrinks to a single element.

3.  **The Core Logic (Divide and Conquer)**:
    *   `mid = left + (right - left) // 2`: We calculate the middle index of the current search space. This formula is preferred over `(left + right) // 2` because it prevents potential integer overflow in languages with fixed-size integers, and it's a good habit in Python as well.
    *   `if nums[mid] == target:`: We check if the element at the middle index is our target. If it is, we've found our answer and can return `mid` immediately.
    *   `elif nums[mid] < target:`: If the middle element is less than the target, we know that the target (if it exists) **must** be in the right half of the current search space, since the array is sorted. We discard the left half by moving our `left` pointer to `mid + 1`.
    *   `else:`: If the middle element is greater than the target, the target **must** be in the left half. We discard the right half by moving our `right` pointer to `mid - 1`.

4.  **Final Return**:
    *   `return -1`: If the `while` loop terminates, it means `left` has become greater than `right`, and the search space is empty. This indicates that the `target` was not found in the array, so we return -1 as required.

This approach efficiently hones in on the target's location by halving the search space with every comparison, resulting in the optimal O(log N) time complexity.

[< Back](index.md)
