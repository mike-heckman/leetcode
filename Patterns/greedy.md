# Design Pattern: Greedy

[< Back](index.md)

A **Greedy** algorithm is an approach for solving optimization problems by making the locally optimal choice at each stage with the hope of finding a global optimum. In other words, a greedy algorithm picks the best immediate option at every step without considering the overall future consequences. For many problems, this strategy works and produces a globally optimal solution.

## When to Use the Greedy Pattern

This pattern is a strong candidate for optimization problems that ask for a minimum or maximum result. A key first step in many greedy algorithms is to **sort the input** based on a specific criterion, which then allows you to make a sequence of locally optimal choices.

Look for this pattern when the problem involves:

*   Finding the minimum number of "things" to accomplish a task (e.g., minimum platforms for trains, minimum arrows to burst balloons).
*   Finding the maximum number of "things" you can fit given constraints (e.g., maximum non-overlapping meetings, maximum profit).
*   Problems where making the "best-looking" choice right now seems like it will lead to the best overall outcome.

**Warning:** The greedy approach does not work for all optimization problems. It's crucial to prove (or be confident) that the local choices will indeed lead to the global optimum.

### Typical Complexity

*   **Time Complexity: O(N log N)**, which is typically dominated by the initial sorting step. The subsequent iteration is usually O(N).
*   **Space Complexity: O(1) or O(N)**, depending on the space required by the sorting algorithm. Python's `Timsort` can use up to O(N) space.

---

## Sample Problem: Non-overlapping Intervals

**LeetCode Link:** [435. Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/)

### Problem Statement

Given an array of `intervals` where `intervals[i] = [start_i, end_i]`, return *the minimum number of intervals you need to remove to make the rest of the intervals non-overlapping*.

**Example 1:**
Input: `intervals = [[1,2],[2,3],[3,4],[1,3]]`
Output: `1`
Explanation: `[1,3]` can be removed and the rest of the intervals are non-overlapping.

**Example 2:**
Input: `intervals = [[1,2],[1,2],[1,2]]`
Output: `2`
Explanation: You need to remove two `[1,2]` to make the rest of the intervals non-overlapping.

### Solution

```python
from typing import List

class Solution:
    def eraseOverlapIntervals(self, intervals: List[List[int]]) -> int:
        if not intervals:
            return 0

        # The greedy choice: Sort by the end time of the intervals.
        # This allows us to always consider the interval that finishes earliest.
        intervals.sort(key=lambda x: x[1])

        # The end time of the last interval we decided to keep
        last_kept_end = intervals[0][1]
        
        # We always keep the first interval, so we start with 1 kept interval.
        kept_count = 1

        for i in range(1, len(intervals)):
            current_start, current_end = intervals[i]

            # If the current interval does not overlap with the last one we kept,
            # we can keep this one as well.
            if current_start >= last_kept_end:
                kept_count += 1
                last_kept_end = current_end
        
        # The number to remove is the total minus the number we kept.
        return len(intervals) - kept_count
```

---

## Detailed Code Analysis

This solution is a classic application of the greedy strategy. The core insight is that to maximize the number of non-overlapping intervals, we should always choose the interval that finishes earliest.

1.  **Edge Case**:
    *   `if not intervals: return 0`: If the input is empty, no intervals need to be removed.

2.  **The Greedy Choice (Sorting)**:
    *   `intervals.sort(key=lambda x: x[1])`: This is the most critical step. We sort the intervals based on their **end times**. Why? Because by picking the interval that finishes earliest, we leave the maximum possible room for subsequent intervals. If we sorted by start time, we might pick a very long interval that blocks out many other shorter intervals.

3.  **Initialization**:
    *   `last_kept_end = intervals[0][1]`: After sorting, the first interval is the one that finishes earliest. We make the greedy choice to keep it. We store its end time.
    *   `kept_count = 1`: We initialize our count of non-overlapping intervals to 1, since we've already decided to keep the first one.

4.  **Iteration and Decision Making**:
    *   We iterate through the rest of the sorted intervals starting from the second one.
    *   `if current_start >= last_kept_end:`: For each interval, we check if its start time is greater than or equal to the end time of the last interval we kept.
        *   If this condition is true, it means the current interval does not overlap with the previous one we kept.
        *   We then make the greedy choice to keep this interval as well. We increment `kept_count` and update `last_kept_end` to the end time of the *current* interval.
    *   If the condition is false, it means the current interval overlaps with the last one we kept. Since we sorted by end times, the current interval *must* end later than the one we already kept. To maximize the remaining room, we do nothing and effectively "discard" the current interval, leaving `last_kept_end` as it is.

5.  **Final Return**:
    *   `return len(intervals) - kept_count`: The problem asks for the minimum number of intervals to *remove*. This is simply the total number of intervals minus the maximum number of non-overlapping intervals we were able to keep.

This approach ensures we find the maximum number of compatible intervals in a single pass after sorting, leading to an optimal O(N log N) time complexity.

[< Back](index.md)
