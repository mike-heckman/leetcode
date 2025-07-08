# Design Pattern: Two Pointers

[< Back](index.md)

The **Two Pointers** pattern is a common and highly efficient technique for problems involving sorted arrays or strings. It works by using two pointers that typically start at opposite ends of the array and move towards each other, or sometimes move in the same direction at different speeds. This approach is used to find a pair or a set of elements that satisfy a certain condition.

## When to Use the Two Pointers Pattern

This pattern is particularly effective for problems where you need to:

*   Find a pair of elements in a **sorted array** that meet a specific condition (e.g., a target sum).
*   Reverse an array or string in-place.
*   Detect palindromes.
*   Solve problems where you need to process an array from both ends simultaneously.

The key requirement is often that the array is **sorted**, which allows for intelligent decisions about which pointer to move.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the number of elements in the array. Each of the two pointers traverses the array at most once.
*   **Space Complexity: O(1)**, as it only requires a few variables to store the pointers and state, without needing extra data structures.

---

## Sample Problem: Two Sum Closest

### Problem Statement

Given a sorted array of integers `nums` and a target integer `target`, find two distinct indices `i` and `j` such that `nums[i] + nums[j]` is closest to the `target`.

Return the absolute difference between the sum and the target.

**Example 1:**
Input: `nums = [-1, 2, 5, 8]`, `target = 8`
Output: `1`
Explanation: The sum of `2 + 5 = 7` is the closest to 8, with a difference of 1.

**Example 2:**
Input: `nums = [10, 20, 30, 40, 50]`, `target = 77`
Output: `3`
Explanation: The sum `30 + 50 = 80` has a difference of `abs(80 - 77) = 3`.

### Solution

```python
import math
from typing import List

class Solution:
    def twoSumClosest(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1
        min_diff = math.inf

        while left < right:
            current_sum = nums[left] + nums[right]
            current_diff = abs(target - current_sum)

            min_diff = min(min_diff, current_diff)

            if current_sum < target:
                left += 1
            elif current_sum > target:
                right -= 1
            else:
                # If the sum is exactly the target, the difference is 0.
                return 0
        
        return min_diff
```

---

## Detailed Code Analysis

The provided solution is a classic example of the two-pointers pattern.

1.  **Initialization**:
    *   `left, right = 0, len(nums) - 1`: Two pointers are initialized. `left` points to the first element of the sorted array, and `right` points to the last element.
    *   `min_diff = math.inf`: A variable is initialized to positive infinity to track the smallest difference found so far between a pair's sum and the target.

2.  **The Loop**:
    *   The `while left < right:` loop continues as long as the two pointers have not crossed each other. This ensures we check all possible pairs.
    *   `current_sum = nums[left] + nums[right]`: Inside the loop, we calculate the sum of the values at the current `left` and `right` positions.
    *   `min_diff = min(min_diff, current_diff)`: We update our `min_diff` if the absolute difference of the `current_sum` and the `target` is smaller than what we've seen before.

3.  **Moving the Pointers (The Core Logic)**:
    *   This is the most critical part of the algorithm. Because the array is sorted, we can make an intelligent decision about how to move the pointers to get closer to the `target`.
    *   `if current_sum < target:`: If our sum is too small, we need to increase it. We do this by moving the `left` pointer one step to the right (`left += 1`), as this will bring in a larger number.
    *   `elif current_sum > target:`: If our sum is too large, we need to decrease it. We move the `right` pointer one step to the left (`right -= 1`) to bring in a smaller number.
    *   `else:`: If the `current_sum` is exactly equal to the `target`, the difference is `0`. This is the smallest possible difference, so we can immediately return `0`.

4.  **Final Return**:
    *   Once the loop terminates (when `left` and `right` meet or cross), `min_diff` will hold the minimum difference found among all pairs. We return this value.

This approach efficiently finds the closest pair by eliminating a large portion of the search space with each iteration, resulting in an O(N) time complexity.

[< Back](index.md)
