# Design Pattern: Trie (Prefix Tree)

[< Back](index.md)

A **Trie** (pronounced "try"), also known as a **Prefix Tree**, is a specialized tree-like data structure used to store a dynamic set of strings. Unlike a binary tree, each node in a Trie represents a single character of a string. A path from the root to a specific node represents a prefix, and if that node is marked as the end of a word, it represents a complete word stored in the set.

Each node typically contains an array or hash map of pointers to its children (one for each possible character) and a boolean flag indicating if the node marks the end of a word.

## When to Use the Trie Pattern

This pattern is the go-to solution for problems involving string prefixes and dictionary lookups. It's highly efficient for these tasks. Look for it when the problem involves:

*   **Implementing an Autocomplete** or "typeahead" feature.
*   **Spell Checking**: Quickly verifying if a word exists in a dictionary.
*   **Prefix Matching**: Finding all words in a set that start with a given prefix.
*   Any scenario where you need to perform fast lookups on a large dictionary of words.

The key advantage is that the time complexity for search, insert, and delete operations depends on the length of the word (L), not the number of words (N) in the dictionary.

### Typical Complexity

For a Trie storing words, where L is the length of the word being processed:
*   **Time Complexity: O(L)** for insertion, search, and prefix lookup. Each operation involves traversing the Trie from the root, one character at a time.
*   **Space Complexity: O(C * S)**, where C is the size of the character set (e.g., 26 for lowercase English letters) and S is the total number of characters across all words in the Trie.

---

## Sample Problem: Implement Trie (Prefix Tree)

**LeetCode Link:** [208. Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/)

### Problem Statement

A Trie is a tree data structure used to efficiently store and retrieve keys in a dataset of strings. Implement the Trie class:

*   `Trie()` Initializes the trie object.
*   `void insert(String word)` Inserts the string `word` into the trie.
*   `boolean search(String word)` Returns `true` if the string `word` is in the trie (i.e., was inserted before), and `false` otherwise.
*   `boolean startsWith(String prefix)` Returns `true` if there is a previously inserted string `word` that has the prefix `prefix`, and `false` otherwise.

### Solution

```python
class TrieNode:
    def __init__(self):
        # Each node has a dictionary for its children and a flag.
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        # The Trie itself only needs a single root node to start.
        self.root = TrieNode()

    def insert(self, word: str) -> None:
        current = self.root
        for char in word:
            # If the character path doesn't exist, create it.
            if char not in current.children:
                current.children[char] = TrieNode()
            # Move to the next node in the path.
            current = current.children[char]
        # Mark the end of the word at the final node.
        current.is_end_of_word = True

    def search(self, word: str) -> bool:
        current = self.root
        for char in word:
            if char not in current.children:
                return False
            current = current.children[char]
        # The word exists only if the path exists AND it's marked as an end.
        return current.is_end_of_word

    def startsWith(self, prefix: str) -> bool:
        current = self.root
        for char in prefix:
            if char not in current.children:
                return False
            current = current.children[char]
        # For a prefix, we only need the path to exist.
        return True
```

---

## Detailed Code Analysis

The solution consists of two classes: `TrieNode` to represent each character's position and `Trie` to manage the overall structure and operations.

1.  **`TrieNode` Class**:
    *   `self.children = {}`: A dictionary (hash map) is used to store child nodes. The key is the character, and the value is the `TrieNode` object. This is more space-efficient than a fixed-size array if the character set is large or sparse.
    *   `self.is_end_of_word = False`: This boolean flag is crucial. It distinguishes between a path that is merely a prefix of another word (e.g., "app") and one that is a complete word itself (e.g., "apple").

2.  **`Trie.insert(word)`**:
    *   `current = self.root`: We start traversing from the root node.
    *   `for char in word:`: We iterate through each character of the word.
    *   `if char not in current.children:`: For each character, we check if a path already exists from the `current` node.
    *   `current.children[char] = TrieNode()`: If the path doesn't exist, we create a new `TrieNode` and add it to the children of the current node.
    *   `current = current.children[char]`: We then move our `current` pointer down to that child node.
    *   `current.is_end_of_word = True`: After the loop finishes, `current` is at the node corresponding to the last character of the word. We mark this node as an end to signify that a complete word terminates here.

3.  **`Trie.search(word)`**:
    *   This method is very similar to `insert`'s traversal. It walks down the tree following the characters of the `word`.
    *   `if char not in current.children:`: If at any point a character does not have a corresponding child node, it means the word does not exist in the Trie, so we can immediately return `False`.
    *   `return current.is_end_of_word`: If the loop completes successfully, it means the entire path for the word exists. However, we must also check the `is_end_of_word` flag. If it's `False`, the word is just a prefix of another longer word (e.g., searching for "app" when only "apple" was inserted), so we return `False`. It must be a complete, marked word.

4.  **`Trie.startsWith(prefix)`**:
    *   The traversal is identical to `search`.
    *   `return True`: The only difference is the return condition. If the loop completes, it means a path for the prefix exists. For this method, that's all that matters. We don't care if it's a complete word or not, so we can immediately return `True`.

[< Back](index.md)
