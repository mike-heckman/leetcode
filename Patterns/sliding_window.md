# Design Pattern: Sliding Window

[< Back](index.md)

The **Sliding Window** is a powerful and efficient algorithmic technique used for problems that involve finding a contiguous subarray or substring in an array or string that satisfies a certain condition. It typically reduces a brute-force O(N^2) or O(N^3) solution to an optimal O(N) solution.

## When to Use the Sliding Window Pattern

This pattern is particularly useful for problems that ask for things like:

*   The longest/shortest subarray/substring that meets a condition.
*   The minimum/maximum value of a subarray/substring.
*   The number of subarrays/substrings that meet a condition.

The key characteristic is that the problem deals with a **contiguous** sequence of elements.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the total number of elements in the input array or string. This is because each pointer (`left` and `right`) traverses the input at most once.
*   **Space Complexity: O(1)**, if the algorithm only needs a few variables to track the state of the window. In some cases, it might be O(K) where K is the number of unique elements in the window (e.g., if using a hash map to store character counts).

---

## Sample Problem: Longest Subarray with At Most K Odd Numbers

### Problem Statement

Given an array of integers `nums` and an integer `k`, return the length of the longest subarray of `nums` that contains at most `k` odd numbers.

**Example 1:**
Input: `nums = [1, 2, 3, 4, 5]`, `k = 2`
Output: `4`
Explanation: The subarray `[2, 3, 4, 5]` has two odd numbers (3 and 5) and has a length of 4.

**Example 2:**
Input: `nums = [2, 4, 6, 8]`, `k = 1`
Output: `4`
Explanation: The subarray `[2, 4, 6, 8]` has zero odd numbers, which is less than or equal to k. The length is 4.

### Solution

```python
from typing import List

class Solution:
    def longestSubarrayWithKOdds(self, nums: List[int], k: int) -> int:
        left = 0
        odd_count = 0
        max_len = 0

        for right in range(len(nums)):
            # Expand the window by including the element at the 'right' pointer
            if nums[right] % 2 != 0:
                odd_count += 1

            # If the window is invalid (too many odd numbers), shrink it from the left.
            while odd_count > k:
                # The element at 'left' is about to be removed from the window
                if nums[left] % 2 != 0:
                    odd_count -= 1
                left += 1
            
            # The window is now valid, update the max length
            max_len = max(max_len, right - left + 1)
        
        return max_len
```

---

## Detailed Code Analysis

The provided solution perfectly demonstrates the sliding window pattern. Let's break it down step-by-step.

1.  **Initialization**:
    *   `left = 0`: This pointer marks the beginning of our sliding window. It starts at the first element.
    *   `odd_count = 0`: This variable tracks the number of odd numbers currently inside our window. This is the "condition" we need to maintain.
    *   `max_len = 0`: This variable will store the maximum length of any valid subarray found so far.

2.  **Expanding the Window**:
    *   The `for right in range(len(nums))` loop iterates through the array, using the `right` pointer to expand the window one element at a time.
    *   For each new element `nums[right]` that enters the window, we check if it's odd (`nums[right] % 2 != 0`). If it is, we increment `odd_count`.

3.  **Shrinking the Window (Maintaining the Condition)**:
    *   The `while odd_count > k:` loop is the key to the pattern's efficiency. It checks if our window has become "invalid" by containing more than `k` odd numbers.
    *   If the window is invalid, we must shrink it from the left until it becomes valid again.
    *   We check if the element being removed (`nums[left]`) is odd. If so, we decrement `odd_count` because it's no longer part of the window.
    *   We then increment `left`, effectively sliding the window's start to the right.
    *   This `while` loop ensures that after it finishes, the window between `left` and `right` is always valid.

4.  **Updating the Result**:
    *   After each step of the `for` loop (and after the `while` loop ensures validity), we have a valid window `[left...right]`.
    *   We calculate its length as `right - left + 1`.
    *   We then update our answer with `max_len = max(max_len, right - left + 1)`. This ensures we are always tracking the longest valid window seen so far.

5.  **Final Return**:
    *   After the `for` loop has finished, `max_len` holds the length of the longest valid subarray found across the entire array, which we return.

This approach guarantees that each element is visited by the `left` and `right` pointers at most once, resulting in a highly efficient O(N) solution.

[< Back](index.md)
