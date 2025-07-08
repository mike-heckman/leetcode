# Design Pattern: Heap (Priority Queue)

[< Back](index.md)

A **Heap** is a specialized tree-based data structure that satisfies the heap property: in a **min-heap**, for any given node, its value is less than or equal to the values of its children. In a **max-heap**, the node's value is greater than or equal to its children's values. This structure ensures that the element with the minimum (or maximum) value is always at the root of the tree.

Heaps are the perfect data structure for implementing a **Priority Queue**, as they provide efficient O(log N) time complexity for insertions and for removing the highest-priority (min/max) element.

## When to Use the Heap Pattern

This pattern is the go-to solution for problems that involve tracking the "Top K" elements from a collection or processing elements based on a priority system. Look for it when the problem involves:

*   Finding the **Kth largest or smallest element** in a collection.
*   Finding the **Top K** most frequent/largest/smallest elements.
*   Finding the **median in a stream** of numbers (using two heaps).
*   Merging **K sorted lists**.
*   Any problem where you repeatedly need to find and remove the minimum or maximum element from a changing set of items.

### Typical Complexity

For a heap of size K:
*   **Time Complexity: O(N log K)** for problems that iterate through N elements while maintaining a heap of size K.
    *   Insertion (`heappush`): O(log K)
    *   Deletion (`heappop`): O(log K)
*   **Space Complexity: O(K)** to store the K elements in the heap.

---

## Sample Problem: Kth Largest Element in an Array

**LeetCode Link:** 215. Kth Largest Element in an Array

### Problem Statement

Given an integer array `nums` and an integer `k`, return the `k`th largest element in the array.

Note that it is the `k`th largest element in the sorted order, not the `k`th distinct element.

**Example 1:**
Input: `nums = [3,2,1,5,6,4]`, `k = 2`
Output: `5`

**Example 2:**
Input: `nums = [3,2,3,1,2,4,5,5,6]`, `k = 4`
Output: `4`

### Solution

```python
import heapq
from typing import List

class Solution:
    def findKthLargest(self, nums: List[int], k: int) -> int:
        # Python's heapq is a min-heap. We can use it to keep track
        # of the K largest elements seen so far.
        min_heap = []

        for num in nums:
            # Push the current number onto the heap
            heapq.heappush(min_heap, num)

            # If the heap size exceeds K, it means we have more than K "largest"
            # elements. The smallest of these is at the root of the min-heap.
            # We can safely remove it.
            if len(min_heap) > k:
                heapq.heappop(min_heap)
        
        # After iterating through all numbers, the heap contains the K largest
        # elements of the entire array. The root of the min-heap is the
        # smallest of these K elements, which is the Kth largest overall.
        return min_heap[0]
```

---

## Detailed Code Analysis

This solution cleverly uses a **min-heap** of a fixed size `k` to find the `k`th largest element in a single pass.

1.  **Initialization**:
    *   `min_heap = []`: An empty list is initialized to serve as our min-heap.

2.  **Iterating Through the Numbers**:
    *   The `for num in nums:` loop processes each number in the input array.

3.  **Maintaining the Heap of Size K (The Core Logic)**:
    *   `heapq.heappush(min_heap, num)`: We add the current number to our min-heap. The heap property is maintained, taking O(log K) time, where K is the current size of the heap.
    *   `if len(min_heap) > k:`: This is the key insight. We only care about the `k` largest elements. If our heap's size grows to `k+1`, it means it contains an element that is "too small" to be in the top `k`.
    *   `heapq.heappop(min_heap)`: Since it's a min-heap, the smallest element is always at the root (`min_heap[0]`). `heappop` removes and returns this smallest element. By doing this, we ensure our heap never stores more than `k` elements, and it always contains the `k` largest numbers encountered *so far*.

4.  **Final Return**:
    *   `return min_heap[0]`: After the loop has processed all `N` numbers, our `min_heap` contains exactly the `k` largest elements from the original array. The smallest among these `k` elements is, by definition, the `k`th largest element overall. This element resides at the root of the min-heap, which we can access with `min_heap[0]`.

This approach is much more efficient than sorting the entire array (which would be O(N log N)). By maintaining a small heap of size `k`, the overall time complexity is O(N log K), which is a significant improvement if `k` is much smaller than `N`.

[< Back](index.md)
