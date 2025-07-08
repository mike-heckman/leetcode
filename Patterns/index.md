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
    B1 --> B1a[Sliding Window];
    click B1a "sliding_window.md" "Sliding Window Pattern"
    
    B --> B2{Process from ends inward?};
    B2 --> B2a[Two Pointers];
    click B2a "two_pointers.md" "Two Pointers Pattern"
    
    B --> B3{Locally optimal choices?};
    B3 --> B3a[Greedy];
    click B3a "greedy.md" "Greedy Pattern"
    
    B --> B4{Running totals/sums?};
    B4 --> B4a[Prefix Sum];
    click B4a "prefix_sum.md" "Prefix Sum Pattern"
    
    B --> B5{Rearranging or calculating a new array?};
    B5 --> B5a[Array Manipulation];
    click B5a "array_manipulation.md" "Array Manipulation Pattern"
    
    B --> B6{Building or modifying a string?};
    B6 --> B6a[String Manipulation];
    click B6a "string_manipulation.md" "String Manipulation Pattern"

    C --> C1{Pointers at different speeds?};
    C1 --> C1a[Fast & Slow Pointers];
    click C1a "fast_and_slow_pointer.md" "Fast & Slow Pointer Pattern"
    
    C --> C2{Reversing the list?};
    C2 --> C2a[LinkedList Reversal];
    click C2a "linked_list.md" "LinkedList Pattern"

    D --> D1{Is it a Tree?};
    D1 -- Yes --> D2{Explore level-by-level?};
    D2 -- Yes --> D2a[Tree BFS];
    click D2a "tree_bfs.md" "Tree BFS Pattern"
    
    D2 -- No --> D3{Explore one branch fully?};
    D3 -- Yes --> D3a[Tree DFS];
    click D3a "tree_dfs.md" "Tree DFS Pattern"
    
    D3 -- No --> D4{Is it a Binary Search Tree?};
    D4 -- Yes --> D4a[Tree BST];
    click D4a "tree_bst.md" "Tree BST Pattern"
    
    D1 -- No, it's a Graph --> D5{Find connected components/traverse?};
    D5 -- Yes --> D5a(Graph DFS);
    click D5a "graph_dfs.md" "Graph DFS Pattern"
    D5 -- Yes --> D5b(Union-Find);
    click D5b "union_find.md" "Union-Find Pattern"
    
    D5 -- No --> D6{Shortest path in unweighted graph?};
    D6 -- Yes --> D6a[Graph BFS];
    click D6a "tree_bfs.md" "Tree BFS Pattern"
    
    D5 -- No --> D7{Find all possible paths/combinations?};
    D7 -- Yes --> D7a[Backtracking];
    click D7a "backtracking.md" "Backtracking Pattern"

    E --> E1{Count frequencies / fast lookups?};
    E1 --> E1a[HashMap / HashSet];
    click E1a "hash_map_set.md" "HashMap/Set Pattern"
    
    E --> E2{"Last-In, First-Out (LIFO)?"};
    E2 --> E2a[Stack];
    click E2a "stack.md" "Stack Pattern"
    
    E --> E3{"First-In, First-Out (FIFO)?"};
    E3 --> E3a[Queue];
    click E3a "queue.md" "Queue Pattern"
    
    E --> E4{Need to find Top 'K' elements?};
    E4 --> E4a[Heap / Priority Queue];
    click E4a "heap.md" "Heap Pattern"
    
    E --> E5{Need to find an element in a sorted collection?};
    E5 --> E5a[Binary Search];
    click E5a "binary_search.md" "Binary Search Pattern"
    
    E --> E6{Need to work with prefixes?};
    E6 --> E6a[Trie];
    click E6a "trie.md" "Trie Pattern"
    
    E --> E7{Need to work with individual bits?};
    E7 --> E7a[Bit Manipulation];
    click E7a "bit_manipulation.md" "Bit Manipulation Pattern"

    F --> Fa[Dynamic Programming];
    click Fa "dynamic_programming.md" "Dynamic Programming Pattern"
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