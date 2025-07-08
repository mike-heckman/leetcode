# Design Pattern: String Manipulation

[< Back](index.md)

The **String Manipulation** pattern is a broad category that covers problems focused on transforming, parsing, or constructing strings. Unlike more complex algorithmic patterns, these problems often test a developer's fluency with their language's built-in string and array/list functionalities.

Solutions typically involve a sequence of operations like splitting a string into parts, reversing or modifying those parts, and then joining them back together. The key is to understand the properties of strings (e.g., immutability in many languages) and to choose the most efficient built-in methods for the task.

## When to Use the String Manipulation Pattern

This pattern applies to a wide variety of problems that are explicitly about strings. Look for it when the problem involves:

*   Reversing a string or parts of a string.
*   Parsing a string to extract specific information.
*   Building a new string from a collection of characters or smaller strings.
*   Finding, replacing, or removing substrings.
*   Validating if a string conforms to a specific format.

### Typical Complexity

*   **Time Complexity: O(N)**, where N is the length of the string. Most operations require iterating through the string at least once.
*   **Space Complexity: O(N)**. Because strings are often immutable, most manipulations require creating new strings or intermediate data structures (like a list of words), which can take up space proportional to the original string's length.

---

## Sample Problem: Reverse Words in a String

**LeetCode Link:** [151. Reverse Words in a String](https://leetcode.com/problems/reverse-words-in-a-string/)

### Problem Statement

Given an input string `s`, reverse the order of the **words**.

A **word** is a sequence of non-space characters. The words in `s` will be separated by at least one space.

Return a string of the words in reverse order concatenated by a single space.

Note that `s` may contain leading or trailing spaces or multiple spaces between two words. The returned string should only have a single space separating the words. Do not include any extra spaces.

**Example 1:**
Input: `s = "the sky is blue"`
Output: `"blue is sky the"`

**Example 2:**
Input: `s = "  hello world  "`
Output: `"world hello"`
Explanation: Your reversed string should not contain leading or trailing spaces.

### Solution

```python
class Solution:
    def reverseWords(self, s: str) -> str:
        # 1. Split the string by spaces. The split() method without arguments
        #    is powerful: it handles multiple spaces between words and also
        #    removes any leading or trailing empty strings.
        words = s.split()
        
        # 2. Reverse the list of words. Python's list slicing `[::-1]`
        #    is a concise way to create a reversed copy of the list.
        reversed_words = words[::-1]
        
        # 3. Join the reversed list of words back into a single string,
        #    with a single space as the separator.
        return " ".join(reversed_words)
```

---

## Detailed Code Analysis

This Python solution is a clean and idiomatic example of string manipulation that leverages high-level built-in functions.

1.  **Splitting the String**:
    *   `words = s.split()`: This is the most important step. In Python, calling `s.split()` with no arguments does more than just split on a single space. It splits on any sequence of whitespace and discards any empty strings that result from the split.
    *   For an input like `"  hello   world  "`, `s.split()` correctly produces the list `['hello', 'world']`, handling all the extra spacing automatically.

2.  **Reversing the List**:
    *   `reversed_words = words[::-1]`: This is a standard Python idiom for reversing a list. It creates a new list containing the elements of `words` in reverse order. For `['hello', 'world']`, this produces `['world', 'hello']`.

3.  **Joining the Words**:
    *   `return " ".join(reversed_words)`: The `join` method is called on the desired separator string (`" "`). It concatenates the elements of the `reversed_words` list, placing the separator between each element.
    *   This step efficiently builds the final output string `"world hello"`.

This three-step process (split, reverse, join) is a very common and effective strategy for solving word-based string manipulation problems. It's readable, concise, and leverages the power of the language's standard library.

[< Back](index.md)
