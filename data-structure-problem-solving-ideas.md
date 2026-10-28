# Data Structure Problem Solving Ideas

This guide explains **how to choose the right data structure** and apply it with confidence.
Each section includes practical intuition and examples.

---

## 1) Arrays

### Best for
- Indexed access and sequential scans.
- Sliding window / two pointers.

### Example use case
Find max sum subarray of size `k` using sliding window on array.

### Trade-offs
- Pros: O(1) random access, cache-friendly.
- Cons: costly middle insert/delete (`O(n)`).

---

## 2) Strings

### Best for
- Parsing, matching, substring checks.

### Example use case
Check if two strings are anagrams with char frequency count.

### Patterns
- Two pointers (palindrome)
- Sliding window (longest unique substring)
- Hash map (frequency)

---

## 3) Linked Lists

### Best for
- Frequent insert/delete with node reference.

### Example use case
Reverse linked list in-place.

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
Validate parentheses string.

### Advanced usage
- Monotonic stack for next greater element / histogram area.

---

## 5) Queue / Deque

### Best for
- FIFO processing, BFS traversal.
- Deque for sliding window max/min.

### Example use case
Level-order traversal of binary tree with queue.

---

## 6) Hash Table (Map/Set)

### Best for
- Fast lookup, counting, deduplication.

### Example use case
Find first non-repeating character with frequency map.

### Interview hint
If you see "find quickly", "exists", "count", think hash map/set first.

---

## 7) Tree (Binary Tree / N-ary Tree)

### Best for
- Hierarchical relationships.

### Example use case
Compute max depth using DFS recursion.

### Traversal toolkit
- DFS: preorder/inorder/postorder
- BFS: level order

---

## 8) Binary Search Tree (BST)

### Best for
- Ordered data, predecessor/successor.

### Example use case
Validate BST using min/max constraints.

### Caveat
Unbalanced BST can degrade to linked-list-like performance.

---

## 9) Heap (Priority Queue)

### Best for
- Repeatedly extract smallest/largest.
- Top-k problems, scheduling.

### Example use case
Find k largest elements using min-heap of size k.

---

## 10) Graph

### Best for
- Network/relationship modeling.

### Example use case
Shortest path in unweighted graph via BFS.

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
Autocomplete: all words starting with "int".

### Trade-off
Fast prefix operations, higher memory usage.

---

## 12) Union-Find (Disjoint Set)

### Best for
- Dynamic connectivity with merge/find operations.

### Example use case
Detect cycle while adding undirected edges.

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

- **Problem:** top 10 frequent search terms from stream  
  **Choice:** hash map + min-heap size 10

- **Problem:** detect if user graph has connection path  
  **Choice:** BFS/DFS graph traversal

- **Problem:** keep median of incoming numbers  
  **Choice:** two heaps (max-heap left, min-heap right)

- **Problem:** quickly check if banned prefix appears  
  **Choice:** trie for prefix checks
