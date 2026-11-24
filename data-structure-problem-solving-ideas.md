# Data Structure Problem Solving Ideas

This guide explains **how to choose the right data structure** and apply it with confidence.
Each section includes practical intuition and examples.

---

## 1) Arrays

### Best for
- Indexed access and sequential scans.
- Sliding window / two pointers.

### Example use case
[Find the maximum sum subarray of size `k`](data-structure-problem-set.md#14-maximum-sum-subarray-of-size-k-easy) using a sliding window on an array.

### Trade-offs
- Pros: O(1) random access, cache-friendly.
- Cons: costly middle insert/delete (`O(n)`).

---

## 2) Strings

### Best for
- Parsing, matching, substring checks.

### Example use case
Check if two strings are anagrams with a character frequency count: [Group Anagrams](data-structure-problem-set.md#24-group-anagrams-medium).

### Patterns
- Two pointers (palindrome)
- Sliding window (longest unique substring)
- Hash map (frequency)

---

## 3) Linked Lists

### Best for
- Frequent insert/delete with node reference.

### Example use case
Reverse a linked list in place: [Reverse Linked List](data-structure-problem-set.md#31-reverse-linked-list-easy).

### Core tricks
- Dummy head for edge cases.
- Fast/slow pointers for middle/cycle.

### Trade-offs
- Pros: flexible insertion/deletion.
- Cons: no O(1) random index access.

---

## 4) Stack

### Best for
- LIFO logic: nested structures, undo, expression parsing.

### Example use case
Validate a parentheses string: [Valid Parentheses](data-structure-problem-set.md#41-valid-parentheses-easy).

### Advanced usage
- Monotonic stack for next greater element / histogram area.

---

## 5) Queue / Deque

### Best for
- FIFO processing, BFS traversal.
- Deque for sliding window max/min.

### Example use case
Traverse a binary tree level by level with a queue: [Binary Tree Level Order Traversal](data-structure-problem-set.md#62-binary-tree-level-order-traversal-medium).

---

## 6) Hash Table (Map/Set)

### Best for
- Fast lookup, counting, deduplication.

### Example use case
Find the first non-repeating character with a frequency map: [First Non-Repeating Character](data-structure-problem-set.md#25-first-non-repeating-character-easy).

### Interview hint
If you see "find quickly", "exists", "count", think hash map/set first.

---

## 7) Tree (Binary Tree / N-ary Tree)

### Best for
- Hierarchical relationships.

### Example use case
Compute maximum depth with recursive DFS: [Maximum Depth of Binary Tree](data-structure-problem-set.md#63-maximum-depth-of-binary-tree-easy).

### Traversal toolkit
- DFS: preorder/inorder/postorder
- BFS: level order

---

## 8) Binary Search Tree (BST)

### Best for
- Ordered data, predecessor/successor.

### Example use case
Validate a BST using minimum and maximum bounds: [Validate Binary Search Tree](data-structure-problem-set.md#64-validate-binary-search-tree-medium).

### Caveat
Unbalanced BST can degrade to linked-list-like performance.

---

## 9) Heap (Priority Queue)

### Best for
- Repeatedly extract smallest/largest.
- Top-k problems, scheduling.

### Example use case
Find the `k` largest elements with a min-heap of size `k`: [K Largest Elements](data-structure-problem-set.md#73-k-largest-elements-medium).

---

## 10) Graph

### Best for
- Network/relationship modeling.

### Example use case
Find the shortest path in an unweighted graph with BFS: [Shortest Path in an Unweighted Graph](data-structure-problem-set.md#84-shortest-path-in-an-unweighted-graph-medium).

### Representation
- Adjacency list preferred for sparse graphs.

### Common tasks
- Connectivity
- Cycle detection
- Topological ordering

---

## 11) Trie

### Best for
- Prefix queries and dictionary search.

### Example use case
Find all words starting with `"int"`: [Autocomplete](data-structure-problem-set.md#92-autocomplete-medium).

### Trade-off
Fast prefix operations, higher memory usage.

---

## 12) Union-Find (Disjoint Set)

### Best for
- Dynamic connectivity with merge/find operations.

### Example use case
Detect a cycle while adding undirected edges: [Redundant Connection](data-structure-problem-set.md#102-redundant-connection-medium).

### Optimization
Path compression + union by rank/size.

---

## How to choose quickly in interviews

1. Need fast existence/count? → **Hash map/set**
2. Need ordering with repeated min/max? → **Heap**
3. Need shortest path (unweighted)? → **BFS + Queue**
4. Need hierarchical traversal? → **Tree DFS/BFS**
5. Need prefix search? → **Trie**
6. Need dynamic groups/connectivity? → **Union-Find**
7. Need contiguous segment logic? → **Array + Sliding Window**

---

## Mini examples (decision based)

- **Problem:** [top 10 frequent search terms from a stream](data-structure-problem-set.md#54-top-search-terms-from-a-stream-medium); **Choice:** hash map + min-heap size 10

- **Problem:** [check whether a path exists in a graph](data-structure-problem-set.md#81-find-if-path-exists-in-graph-easy); **Choice:** BFS/DFS graph traversal

- **Problem:** [maintain the median of incoming numbers](data-structure-problem-set.md#75-find-median-from-data-stream-hard); **Choice:** two heaps (max-heap left, min-heap right)

- **Problem:** [find words matching a prefix](data-structure-problem-set.md#92-autocomplete-medium); **Choice:** trie for prefix checks

## Matching practice problems

| Guide example | Problem set exercise |
|---|---|
| Maximum sum subarray of size `k` | [Maximum Sum Subarray of Size K](data-structure-problem-set.md#14-maximum-sum-subarray-of-size-k-easy) |
| Anagram grouping | [Group Anagrams](data-structure-problem-set.md#24-group-anagrams-medium) |
| Reverse linked list | [Reverse Linked List](data-structure-problem-set.md#31-reverse-linked-list-easy) |
| Validate parentheses | [Valid Parentheses](data-structure-problem-set.md#41-valid-parentheses-easy) |
| Histogram area with a monotonic stack | [Largest Rectangle in Histogram](data-structure-problem-set.md#43-largest-rectangle-in-histogram-hard) |
| Level-order tree traversal | [Binary Tree Level Order Traversal](data-structure-problem-set.md#62-binary-tree-level-order-traversal-medium) |
| First non-repeating character | [First Non-Repeating Character](data-structure-problem-set.md#25-first-non-repeating-character-easy) |
| Maximum tree depth | [Maximum Depth of Binary Tree](data-structure-problem-set.md#63-maximum-depth-of-binary-tree-easy) |
| Validate a BST | [Validate Binary Search Tree](data-structure-problem-set.md#64-validate-binary-search-tree-medium) |
| Find `k` largest elements | [K Largest Elements](data-structure-problem-set.md#73-k-largest-elements-medium) |
| Keep a running median from incoming values | [Find Median from Data Stream](data-structure-problem-set.md#75-find-median-from-data-stream-hard) |
| Unweighted graph shortest path | [Shortest Path in an Unweighted Graph](data-structure-problem-set.md#84-shortest-path-in-an-unweighted-graph-medium) |
| Autocomplete by prefix | [Autocomplete](data-structure-problem-set.md#92-autocomplete-medium) |
| Detect a cycle while adding edges | [Redundant Connection](data-structure-problem-set.md#102-redundant-connection-medium) |
