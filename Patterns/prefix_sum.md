# Design Pattern: Prefix Sum

[< Back](index.md)

The **Prefix Sum** (also known as cumulative sum) is a powerful technique that involves pre-calculating a running sum of an array. A prefix sum array `P` is one where `P[i]` is the sum of all elements from the original array `A` from index `0` up to `i`. This pre-computation allows for extremely fast calculation of the sum of any contiguous subarray.

The core idea is that the sum of a subarray from index `i` to `j` (inclusive) can be found in O(1) time using the formula: `sum(i, j) = P[j] - P[i-1]`.

## When to Use the Prefix Sum Pattern

This pattern is extremely useful for problems involving queries on range sums of an array. It's a key building block for more complex problems. Look for it when you need to:

*   Find the sum of a contiguous subarray multiple times.
*   Find a subarray or the number of subarrays whose sum equals a target value `k`.
*   Solve problems where a brute-force check of all subarray sums would be too slow (O(N^2)).
*   Problems involving equilibrium points or balancing parts of an array.

### Typical Complexity

*   **Time Complexity:**
    *   **Pre-computation:** O(N) to build the prefix sum array.
    *   **Query:** O(1) to find the sum of any subarray.
    *   For problems using a hash map with prefix sums, the overall time complexity is typically O(N).
*   **Space Complexity: O(N)** to store the prefix sum array or the hash map of prefix sum frequencies.

---

## Sample Problem: Subarray Sum Equals K

**LeetCode Link:** [560. Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/)

### Problem Statement

Given an array of integers `nums` and an integer `k`, return the total number of continuous subarrays whose sum equals `k`.

**Example 1:**
Input: `nums = [1,1,1]`, `k = 2`
Output: `2`
Explanation: The two subarrays are `[1,1]` and `[1,1]`.

**Example 2:**
Input: `nums = [1,2,3]`, `k = 3`
Output: `2`
Explanation: The two subarrays are `[1,2]` and `[3]`.

### Solution

```python
from typing import List

class Solution:
    def subarraySum(self, nums: List[int], k: int) -> int:
        count = 0
        current_sum = 0
        # A hash map to store the frequency of prefix sums: {prefix_sum: count}
        prefix_sums = {0: 1}

        for num in nums:
            # Calculate the current prefix sum
            current_sum += num

            # We are looking for a previous prefix sum 'x' such that:
            # current_sum - x = k
            # which rearranges to:
            # x = current_sum - k
            # So, we check if this 'complement' sum exists in our map.
            complement = current_sum - k
            if complement in prefix_sums:
                count += prefix_sums[complement]
            
            # Add the current prefix sum to the map, or increment its count
            prefix_sums[current_sum] = prefix_sums.get(current_sum, 0) + 1
            
        return count
```

---

## Detailed Code Analysis

This solution cleverly combines the prefix sum concept with a hash map to achieve an O(N) time complexity.

1.  **Initialization**:
    *   `count = 0`: This variable will store our final result, the total number of valid subarrays.
    *   `current_sum = 0`: This tracks the running prefix sum as we iterate through the array.
    *   `prefix_sums = {0: 1}`: This is the most critical part of the initialization. We use a hash map to store the cumulative frequencies of the prefix sums we've encountered. We initialize it with `{0: 1}` to handle the edge case where a subarray starting from index `0` has a sum equal to `k`. For example, in `[3, 4, 7]` with `k=7`, when `current_sum` is `7`, the `complement` is `7 - 7 = 0`. The entry `{0: 1}` allows us to correctly count this subarray.

2.  **Iterating Through the Array**:
    *   The `for num in nums:` loop processes each number in the input array once.

3.  **Calculating Prefix Sum and Finding Complements**:
    *   `current_sum += num`: In each iteration, we update our running sum, which represents the prefix sum up to the current element.
    *   `complement = current_sum - k`: This is the core logic. If the sum of a subarray `[i..j]` is `k`, then `prefix_sum[j] - prefix_sum[i-1] = k`. As we iterate, our `current_sum` is `prefix_sum[j]`. We are looking for a previous `prefix_sum[i-1]` that satisfies the equation. Rearranging it, we get `prefix_sum[i-1] = prefix_sum[j] - k`. Our `complement` variable is the value of the previous prefix sum we need to have seen for the current subarray to sum to `k`.
    *   `if complement in prefix_sums:`: We check our hash map to see if this required `complement` has occurred before.
    *   `count += prefix_sums[complement]`: If it has, we add its frequency to our `count`. We add the frequency because there could be multiple subarrays ending at different previous positions that all result in the same required `complement` prefix sum.

4.  **Updating the Prefix Sum Map**:
    *   `prefix_sums[current_sum] = prefix_sums.get(current_sum, 0) + 1`: After checking for a complement, we must update our map with the `current_sum`. We either add it to the map with a count of 1 or, if it's already there, we increment its frequency. This makes the current prefix sum available for future iterations to use as a potential complement.

5.  **Final Return**:
    *   After the loop finishes, `count` holds the total number of subarrays that sum to `k`, which we return.

This approach avoids the O(N^2) complexity of checking every possible subarray by using O(N) extra space for the hash map, resulting in a highly efficient O(N) time solution.

[< Back](index.md)
