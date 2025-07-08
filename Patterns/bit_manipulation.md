# Design Pattern: Bit Manipulation

[< Back](index.md)

**Bit Manipulation** is a technique for solving problems by working directly with the binary representation of numbers. It involves using bitwise operators—such as `AND (&)`, `OR (|)`, `XOR (^)`, `NOT (~)`, and bit shifts (`<<`, `>>`)—to perform operations in a highly efficient manner.

This pattern can often lead to solutions with very low time and space complexity, but it requires a good understanding of how data is stored at the bit level.

## When to Use the Bit Manipulation Pattern

This pattern is a strong candidate for problems that involve properties of numbers related to their binary form. Look for it when the problem involves:

*   Finding a unique element in a list where all other elements appear a specific number of times (e.g., twice, three times).
*   Checking if a number is a power of two.
*   Counting the number of set bits (1s) in a number's binary representation.
*   Swapping two numbers without a temporary variable.
*   Efficiently managing a set of boolean flags using the bits of a single integer.
*   Problems where constraints on memory (e.g., O(1) space) or time are very strict, suggesting a non-standard approach is needed.

### Typical Complexity

*   **Time Complexity: O(N)** for problems that iterate through a list of numbers, or **O(1)** for operations on a single number. The bitwise operations themselves are constant time.
*   **Space Complexity: O(1)**, as these solutions typically only require a few variables and no auxiliary data structures.

---

## Sample Problem: Single Number

**LeetCode Link:** 136. Single Number

### Problem Statement

Given a **non-empty** array of integers `nums`, every element appears *twice* except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example 1:**
Input: `nums = [2,2,1]`
Output: `1`

**Example 2:**
Input: `nums = [4,1,2,1,2]`
Output: `4`

### Solution

```python
from typing import List

class Solution:
    def singleNumber(self, nums: List[int]) -> int:
        # Initialize a variable to 0. XOR with 0 returns the number itself.
        result = 0
        
        # Iterate through all numbers in the list
        for num in nums:
            # Apply the XOR operation
            result ^= num
            
        return result
```

---

## Detailed Code Analysis

This solution leverages the properties of the XOR (exclusive OR) bitwise operator to find the unique number in a single pass.

### Key Properties of XOR (`^`)

1.  **Identity Property**: Any number XORed with 0 is the number itself (`a ^ 0 = a`).
2.  **Self-Inverse Property**: Any number XORed with itself is 0 (`a ^ a = 0`).
3.  **Commutative Property**: The order of operation does not matter (`a ^ b = b ^ a`).

### How the Solution Works

1.  **Initialization**:
    *   `result = 0`: We initialize our result variable to 0. Based on the identity property, this ensures that the first XOR operation will correctly store the first number from the list.

2.  **The Loop (The Core Logic)**:
    *   `for num in nums:`: We iterate through each number in the input array.
    *   `result ^= num`: In each iteration, we XOR the `current` number with our `result`.

Let's trace this with the example `nums = [4, 1, 2, 1, 2]`:

*   `result` starts at `0`.
*   `result = 0 ^ 4`  --> `result` is `4`
*   `result = 4 ^ 1`  --> `result` is `4 ^ 1`
*   `result = (4 ^ 1) ^ 2` --> `result` is `4 ^ 1 ^ 2`
*   `result = (4 ^ 1 ^ 2) ^ 1` --> `result` is `4 ^ (1 ^ 1) ^ 2` --> `4 ^ 0 ^ 2` --> `4 ^ 2`
*   `result = (4 ^ 2) ^ 2` --> `result` is `4 ^ (2 ^ 2)` --> `4 ^ 0` --> `4`

As you can see, because of the commutative and self-inverse properties, all the numbers that appear twice effectively cancel each other out (`1^1=0`, `2^2=0`), leaving only the number that appeared once.

3.  **Final Return**:
    *   `return result`: After the loop completes, the `result` variable holds the value of the single, unique number.

This approach is incredibly efficient. It solves the problem in O(N) time because it iterates through the list once, and it uses O(1) space because it only requires a single extra variable (`result`), perfectly meeting the problem's constraints.

[< Back](index.md)
