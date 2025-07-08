# Design Pattern: Array Manipulation

[< Back](index.md)

The **Array Manipulation** pattern covers a broad category of problems where the goal is to create a new array or modify an existing one based on a set of rules or calculations derived from its elements. Unlike more specific patterns like "Sliding Window" or "Two Pointers," this pattern often involves one or more passes over the array to gather information (like prefix or suffix products/sums) before calculating the final result for each index.

## When to Use the Array Manipulation Pattern

This pattern is a good fit for problems where the value at a given index `i` in the result depends on elements to the left and/or right of `i` in the original array. Look for it when:

*   You need to compute values based on prefixes and suffixes of the array.
*   The problem can be solved by iterating through the array multiple times, with each pass building up a piece of the final answer.
*   You need to perform in-place modifications that change the structure or values of the array based on its own elements.
*   The constraints prevent a simple brute-force solution (e.g., O(N^2)), suggesting a more clever O(N) approach is needed.

### Typical Complexity

*   **Time Complexity: O(N)**, as solutions typically involve a constant number of passes over the array.
*   **Space Complexity: O(1) or O(N)**. If the problem allows modifying the array in-place or if the output array is not counted as extra space, the complexity can be O(1). Otherwise, it is O(N) to store the result or any intermediate prefix/suffix arrays.

---

## Sample Problem: Product of Array Except Self

**LeetCode Link:** [238. Product of Array Except Self](https://leetcode.com/problems/product-of-array-except-self/)

### Problem Statement

Given an integer array `nums`, return an array `answer` such that `answer[i]` is equal to the product of all the elements of `nums` except `nums[i]`.

The product of any prefix or suffix of `nums` is **guaranteed** to fit in a 32-bit integer.

You must write an algorithm that runs in O(n) time and without using the division operation.

**Example 1:**
Input: `nums = [1,2,3,4]`
Output: `[24,12,8,6]`

**Example 2:**
Input: `nums = [-1,1,0,-3,3]`
Output: `[0,0,9,0,0]`

### Solution

```python
from typing import List

class Solution:
    def productExceptSelf(self, nums: List[int]) -> List[int]:
        n = len(nums)
        # The output array, which will also be used to store prefix products
        result = [1] * n

        # First pass: Calculate prefix products
        # result[i] will contain the product of all elements to the left of i
        prefix_product = 1
        for i in range(n):
            result[i] = prefix_product
            prefix_product *= nums[i]

        # Second pass: Calculate suffix products and combine with prefix products
        # Multiply each prefix product by the corresponding suffix product
        suffix_product = 1
        for i in range(n - 1, -1, -1):
            result[i] *= suffix_product
            suffix_product *= nums[i]
            
        return result
```

---

## Detailed Code Analysis

This solution is a classic example of the array manipulation pattern, solving the problem in two passes without division and with O(1) extra space (as the output array is not counted).

The core idea is that the product of all elements except `self` at index `i` is equal to `(product of all elements to the left of i) * (product of all elements to the right of i)`.

1.  **Initialization**:
    *   `n = len(nums)`: Get the length of the array.
    *   `result = [1] * n`: Initialize the `result` array with all `1`s. This array will serve a dual purpose: first to store prefix products, and then to store the final answer.

2.  **First Pass (Left to Right for Prefix Products)**:
    *   `prefix_product = 1`: This variable will keep a running product of elements from the left.
    *   `for i in range(n):`: We iterate through the array from left to right.
    *   `result[i] = prefix_product`: For each index `i`, we first set `result[i]` to the current `prefix_product`. This stores the product of all elements *before* `nums[i]`.
    *   `prefix_product *= nums[i]`: We then update the `prefix_product` by multiplying it with the current element `nums[i]`, preparing it for the next iteration.
    *   After this pass, `result` for `[1,2,3,4]` will be `[1, 1, 2, 6]`.

3.  **Second Pass (Right to Left for Suffix Products)**:
    *   `suffix_product = 1`: This variable will keep a running product of elements from the right.
    *   `for i in range(n - 1, -1, -1):`: We iterate through the array from right to left.
    *   `result[i] *= suffix_product`: We take the existing value in `result[i]` (which is the prefix product) and multiply it by the current `suffix_product`. This `suffix_product` represents the product of all elements *after* `nums[i]`. The result is the final answer for index `i`.
    *   `suffix_product *= nums[i]`: We then update the `suffix_product` by multiplying it with the current element `nums[i]`, preparing it for the next iteration to the left.

4.  **Final Return**:
    *   `return result`: The `result` array now contains the product of all elements except self for each position. For `[1,2,3,4]`, the second pass modifies `[1, 1, 2, 6]` into the final answer `[24, 12, 8, 6]`.

This two-pass approach cleverly builds the final result without needing division or a separate O(N) space for prefix and suffix arrays.

[< Back](index.md)
