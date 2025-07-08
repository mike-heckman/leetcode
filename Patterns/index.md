# LeetCode Patterns Flowchart

This flowchart is a decision tree to help identify the correct data structure or algorithm pattern for a given LeetCode problem. Click on a pattern name to see a detailed explanation and a sample problem.

```mermaid
graph LR
    A[Start: What's the main data structure?] --> B{Array / String};
    A --> C{Linked List};
    A --> D{Tree / Graph};
    A --> E{Need a helper structure?};
    A --> F{"Is it an optimization or counting problem<br/>with overlapping subproblems?"};

    B --> B1{Contiguous subarray/substring?};
    B1 --> B1a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/sliding_window.md">Sliding Window</a>];
    
    B --> B2{Process from ends inward?};
    B2 --> B2a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/two_pointers.md">Two Pointers</a>];
    
    B --> B3{Locally optimal choices?};
    B3 --> B3a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/greedy.md">Greedy</a>];
    
    B --> B4{Running totals/sums?};
    B4 --> B4a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/prefix_sum.md">Prefix Sum</a>];
    
    B --> B5{Rearranging or calculating a new array?};
    B5 --> B5a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/array_manipulation.md">Array Manipulation</a>];
    
    B --> B6{Building or modifying a string?};
    B6 --> B6a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/string_manipulation.md">String Manipulation</a>];

    C --> C1{Pointers at different speeds?};
    C1 --> C1a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/fast_and_slow_pointer.md">Fast & Slow Pointers</a>];
    
    C --> C2{Reversing the list?};
    C2 --> C2a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/linked_list.md">LinkedList Reversal</a>];

    D --> D1{Is it a Tree?};
    D1 -- Yes --> D2{Explore level-by-level?};
    D2 -- Yes --> D2a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/tree_bfs.md">Tree BFS</a>];
    
    D2 -- No --> D3{Explore one branch fully?};
    D3 -- Yes --> D3a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/tree_dfs.md">Tree DFS</a>];
    
    D3 -- No --> D4{Is it a Binary Search Tree?};
    D4 -- Yes --> D4a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/tree_bst.md">Tree BST</a>];
    
    D1 -- No, it's a Graph --> D5{Find connected components/traverse?};
    D5 -- Yes --> D5a(<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/graph_dfs.md">Graph DFS</a>);
    D5 -- Yes --> D5b(<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/union_find.md">Union-Find</a>);
    
    D5 -- No --> D6{Shortest path in unweighted graph?};
    D6 -- Yes --> D6a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/tree_bfs.md">Graph BFS</a>];
    
    D5 -- No --> D7{Find all possible paths/combinations?};
    D7 -- Yes --> D7a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/backtracking.md">Backtracking</a>];

    E --> E1{Count frequencies / fast lookups?};
    E1 --> E1a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/hash_map_set.md">HashMap / HashSet</a>];
    
    E --> E2{"Last-In, First-Out (LIFO)?"};
    E2 --> E2a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/stack.md">Stack</a>];
    
    E --> E3{"First-In, First-Out (FIFO)?"};
    E3 --> E3a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/queue.md">Queue</a>];
    
    E --> E4{Need to find Top 'K' elements?};
    E4 --> E4a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/heap.md">Heap / Priority Queue</a>];
    
    E --> E5{Need to find an element in a sorted collection?};
    E5 --> E5a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/binary_search.md">Binary Search</a>];
    
    E --> E6{Need to work with prefixes?};
    E6 --> E6a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/trie.md">Trie</a>];
    
    E --> E7{Need to work with individual bits?};
    E7 --> E7a[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/bit_manipulation.md">Bit Manipulation</a>];

    F --> Fa[<a href="https://github.com/mike-heckman/leetcode/blob/main/Patterns/dynamic_programming.md">Dynamic Programming</a>];
```


## Above Patterns in alphabetical order

* [Array Manipulation](array_manipulation.md)
* [Backtracking](backtracking.md)
* [Binary Search](binary_search.md)
* [Bit Manipulation](bit_manipulation.md)
* [Dynamic Programming](dynamic_programming.md)
* [Fast & Slow Pointers](fast_and_slow_pointer.md)
* [Graph BFS](tree_bfs.md)
* [Graph DFS](graph_dfs.md)
* [Greedy](greedy.md)
* [HashMap / HashSet](hash_map_set.md)
* [Heap / Priority Queue](heap.md)
* [Linked List Reversal](linked_list.md)
* [Prefix Sum](prefix_sum.md)
* [Queue](queue.md)
* [Sliding Window](sliding_window.md)
* [Stack](stack.md)
* [String Manipulation](string_manipulation.md)
* [Tree & Graph Traversals (General)](tree_and_graph_traversals.md)
* [Tree BFS](tree_bfs.md)
* [Tree BST](tree_bst.md)
* [Tree DFS](tree_dfs.md)
* [Trie](trie.md)
* [Two Pointers](two_pointers.md)
* [Union-Find](union_find.md)


## Other Patterns

Here are some additional patterns documented in this directory that are not explicitly represented in the flowchart above.

*   [Tree & Graph Traversals (General)](tree_and_graph_traversals.md)