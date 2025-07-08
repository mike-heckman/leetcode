# Design Pattern: Stack

[< Back](index.md)

The **Stack** is a fundamental data structure that follows the **Last-In, First-Out (LIFO)** principle. It's analogous to a stack of plates: you can only add a new plate to the top or remove the topmost plate. This simple concept is incredibly powerful for solving a wide range of problems involving sequences, hierarchies, or backtracking.

## When to Use the Stack Pattern

This pattern is particularly useful for problems that involve:

*   Processing nested structures (e.g., matching parentheses, parsing HTML/XML).
*   Evaluating expressions (e.g., postfix or infix notation).
*   Finding the "next greater element" or "previous smaller element" for items in a sequence (a sub-pattern known as a Monotonic Stack).
*   Simulating recursion or managing function calls.
*   Traversing graphs or trees (as part of Depth-First Search).
*   Implementing backtracking algorithms where you need to undo a state.

The key characteristic is the need to process elements in a reversed order of their appearance or to manage a sequence of operations that need to be undone.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the total number of elements in the input. Each element is typically pushed onto and popped from the stack at most once.
*   **Space Complexity: O(N)** in the worst case, where all elements might be pushed onto the stack before any are popped. In many problems, the space is O(W) where W is the maximum number of concurrent items on the stack at any given time.

---

## Sample Problem: Valid Parentheses

**LeetCode Link:** [20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)

### Problem Statement

Given a string `s` containing just the characters `(`, `)`, `{`, `}`, `[` and `]`, determine if the input string is valid.

An input string is valid if:
1.  Open brackets must be closed by the same type of brackets.
2.  Open brackets must be closed in the correct order.

**Example 1:**
Input: `s = "()[]{}"`
Output: `true`

**Example 2:**
Input: `s = "(]"`
Output: `false`

**Example 3:**
Input: `s = "{[]}"`
Output: `true`

### Solution

```python
class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        # A mapping of closing brackets to their opening counterparts
        mapping = {")": "(", "}": "{", "]": "["}

        for char in s:
            # If the character is a closing bracket
            if char in mapping:
                # Pop the top element from the stack if it's not empty.
                # Otherwise, assign a dummy value to top_element.
                top_element = stack.pop() if stack else '#'
                
                # The popped element must be the matching opening bracket
                if mapping[char] != top_element:
                    return False
            else:
                # It's an opening bracket, so push it onto the stack
                stack.append(char)
        
        # If the stack is empty, all brackets were matched correctly.
        return not stack
```

---

## Detailed Code Analysis

This solution elegantly uses a stack to validate the order and type of brackets.

1.  **Initialization**:
    *   `stack = []`: An empty list serves as our stack to keep track of open brackets as we encounter them.
    *   `mapping = {")": "(", ...}`: A hash map provides an O(1) lookup to find the corresponding opening bracket for any closing bracket.

2.  **Iterating Through the String**:
    *   The `for char in s:` loop processes the string one character at a time.

3.  **Handling Closing Brackets**:
    *   `if char in mapping:`: This condition efficiently checks if the current character is a closing bracket.
    *   If it is, we must check if it correctly closes the most recently opened bracket. The LIFO property of the stack is perfect for this. We `pop()` the top of the stack.
    *   `if stack else '#'`: An important edge case is handled here. If we encounter a closing bracket but the stack is empty (e.g., `s = "}"`), the string is invalid. We pop a dummy value `'#'` to ensure the subsequent comparison fails.
    *   `if mapping[char] != top_element:`: We compare the popped element with the expected opening bracket from our map. If they don't match (e.g., `s = "(]"`), the string is invalid, and we return `False`.

4.  **Handling Opening Brackets**:
    *   `else:`: If a character is not a closing bracket, it must be an opening one.
    *   `stack.append(char)`: We push the opening bracket onto the stack. It will wait there until its corresponding closing bracket is found.

5.  **Final Check**:
    *   `return not stack`: After the loop, if the stack is empty, it means every opening bracket found a matching closing bracket in the correct order. If the stack is not empty (e.g., `s = "(("`), it means there are unclosed opening brackets, making the string invalid. The expression `not stack` neatly returns `True` for an empty stack and `False` otherwise.

This approach ensures each character is processed exactly once, and stack operations are O(1), resulting in a clean and efficient O(N) solution.

[< Back](index.md)
