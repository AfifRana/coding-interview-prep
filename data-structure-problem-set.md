# Data Structure Problem Set

Each problem lists a statement, example, the data structure idea, and complexity. See [data-structure-problem-solving-ideas.md](data-structure-problem-solving-ideas.md) for when to choose each structure.

## Table of Contents
1. [Arrays](#1-arrays)
2. [Strings](#2-strings)
3. [Linked Lists](#3-linked-lists)
4. [Stacks / Queues](#4-stacks--queues)
5. [Hash Tables](#5-hash-tables)
6. [Trees](#6-trees)
7. [Heaps](#7-heaps)
8. [Graphs](#8-graphs)
9. [Tries](#9-tries)
10. [Union-Find](#10-union-find)

---

## 1) Arrays

### Easy: Remove Duplicates from Sorted Array
**Problem:** Remove duplicates in place from a sorted array and return the new length `k`; the first `k` elements hold the unique values.

**Example:** `[0,0,1,1,1,2,2,3,3,4]` -> `k = 5`, `[0,1,2,3,4,...]`

**Idea:** Slow/fast pointers. `fast` scans; when `nums[fast] != nums[slow]`, increment `slow` and copy.

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: Product of Array Except Self
**Problem:** Return `answer` where `answer[i]` is the product of all elements except `nums[i]`, without using division, in `O(n)`.

**Example:** `[1,2,3,4]` -> `[24,12,8,6]`

**Idea:** Left pass stores the product of everything to the left. Right pass multiplies by a running product of everything to the right.

**Complexity:** Time `O(n)`, Space `O(1)` extra

### Hard: First Missing Positive
**Problem:** Find the smallest missing positive integer in `O(n)` time and `O(1)` extra space.

**Example:** `[3,4,-1,1]` -> `2`

**Idea:** Use the array as a hash: place each value `v` in `1..n` at index `v - 1` by swapping (cyclic sort). The first index `i` where `nums[i] != i + 1` gives answer `i + 1`; otherwise `n + 1`.

**Complexity:** Time `O(n)`, Space `O(1)`

---

## 2) Strings

### Easy: Valid Palindrome
**Problem:** After lowercasing and removing non-alphanumerics, is the string a palindrome?

**Example:** `"A man, a plan, a canal: Panama"` -> `true`

**Idea:** Two pointers from both ends, skipping non-alphanumerics.

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: Longest Palindromic Substring
**Problem:** Return the longest palindromic substring.

**Example:** `"babad"` -> `"bab"` (or `"aba"`)

**Idea:** Expand around each center (both odd and even centers) and track the longest. (Manacher's algorithm gives `O(n)`.)

**Complexity:** Time `O(n^2)`, Space `O(1)`

### Hard: Regular Expression Matching
**Problem:** Implement matching with `.` (any single char) and `*` (zero or more of the preceding element) over the entire string.

**Example:** `s = "aab", p = "c*a*b"` -> `true`

**Idea:** DP with `dp[i][j]` = first `i` chars of `s` match first `j` chars of `p`. If `p[j-1] == '*'`: either skip `x*` (`dp[i][j-2]`) or consume one char when `p[j-2]` matches `s[i-1]` (`dp[i-1][j]`). Otherwise match the single character or `.` with `dp[i-1][j-1]`.

**Complexity:** Time `O(m * n)`, Space `O(m * n)`

---

## 3) Linked Lists

### Easy: Reverse Linked List
**Problem:** Reverse a singly linked list.

**Example:** `1->2->3->4->5` -> `5->4->3->2->1`

**Idea:** Iterate with `prev`, `curr`, `next`; point `curr.next = prev` and advance.

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: Add Two Numbers
**Problem:** Two non-empty lists store non-negative integers in reverse digit order. Return their sum as a list.

**Example:** `2->4->3` + `5->6->4` -> `7->0->8`

**Idea:** Walk both lists with a `carry`, using a dummy head. Continue while either list or the carry remains.

**Complexity:** Time `O(max(m, n))`, Space `O(max(m, n))`

### Hard: Reverse Nodes in k-Group
**Problem:** Reverse the nodes of a list `k` at a time; leftover nodes (fewer than `k`) stay as they are.

**Example:** `1->2->3->4->5, k = 2` -> `2->1->4->3->5`

**Idea:** For each group, first check that `k` nodes remain. Reverse that segment in place, then reconnect the previous group's tail to the new head and the new tail to the next group. A dummy head simplifies the first group.

**Complexity:** Time `O(n)`, Space `O(1)`

---

## 4) Stacks / Queues

### Easy: Valid Parentheses
**Problem:** Check whether a string of `()[]{}` is validly nested.

**Example:** `"([]){}"` -> `true`; `"(]"` -> `false`

**Idea:** Push opening brackets; on a closing bracket, the stack top must be its match. The stack must be empty at the end.

**Complexity:** Time `O(n)`, Space `O(n)`

### Medium: Implement Queue using Stacks
**Problem:** Implement `push`, `pop`, `peek`, `empty` for a FIFO queue using only stacks.

**Example:** `push(1), push(2), peek() -> 1, pop() -> 1, empty() -> false`

**Idea:** Use an `in` stack and an `out` stack. On `pop`/`peek`, if `out` is empty, move everything from `in` to `out` (reversing order).

**Complexity:** Amortised `O(1)` per operation, Space `O(n)`

### Hard: Largest Rectangle in Histogram
**Problem:** Find the largest rectangle area in a histogram.

**Example:** `[2,1,5,6,2,3]` -> `10`

**Idea:** Increasing stack of indices; on a shorter bar, pop and compute the area with the popped height and the width between neighbouring stack entries. A sentinel `0` flushes the stack.

**Complexity:** Time `O(n)`, Space `O(n)`

---

## 5) Hash Tables

### Easy: Two Sum
**Problem:** Return indices of two numbers adding up to `target`.

**Example:** `[2,7,11,15], target = 9` -> `[0,1]`

**Idea:** Map value -> index; for each number check whether `target - num` is already in the map.

**Complexity:** Time `O(n)`, Space `O(n)`

### Medium: LRU Cache
**Problem:** Design a cache with `get(key)` and `put(key, value)` in `O(1)` that evicts the least recently used key when over capacity.

**Example:** capacity 2: `put(1,1), put(2,2), get(1) -> 1, put(3,3)` evicts key 2.

**Idea:** Hash map from key -> node of a doubly linked list. Move a node to the front on every access; evict from the tail. Use dummy head/tail nodes.

**Complexity:** `O(1)` per operation, Space `O(capacity)`

### Hard: All O`one Data Structure
**Problem:** Support `inc(key)`, `dec(key)`, `getMaxKey()`, `getMinKey()` all in `O(1)`.

**Example:** `inc("a"), inc("b"), inc("b")` -> max is `"b"`, min is `"a"`.

**Idea:** A doubly linked list of buckets ordered by count, each bucket holding a set of keys, plus a map key -> bucket. `inc`/`dec` moves a key to the neighbouring bucket (creating or deleting buckets as needed). Min and max are the list's ends.

**Complexity:** `O(1)` per operation, Space `O(n)`

---

## 6) Trees

### Easy: Symmetric Tree
**Problem:** Is a binary tree a mirror of itself?

**Example:** `[1,2,2,3,4,4,3]` -> `true`

**Idea:** Recursively compare `isMirror(a, b)`: values equal, `a.left` vs `b.right`, and `a.right` vs `b.left`.

**Complexity:** Time `O(n)`, Space `O(h)`

### Medium: Binary Tree Level Order Traversal
**Problem:** Return node values level by level.

**Example:** `[3,9,20,null,null,15,7]` -> `[[3],[9,20],[15,7]]`

**Idea:** BFS with a queue; process exactly `len(queue)` nodes per level.

**Complexity:** Time `O(n)`, Space `O(w)` where `w` is the maximum width

### Hard: Serialize and Deserialize Binary Tree
**Problem:** Convert a binary tree to a string and back.

**Example:** `[1,2,3,null,null,4,5]` -> `"1,2,#,#,3,4,#,#,5,#,#"` -> same tree

**Idea:** Preorder traversal writing a marker (`#`) for null children. Deserialize by consuming tokens recursively in the same order. A level-order encoding also works.

**Complexity:** Time `O(n)`, Space `O(n)`

---

## 7) Heaps

### Easy: Last Stone Weight
**Problem:** Repeatedly smash the two heaviest stones; if weights differ, the difference remains. Return the last stone's weight (or `0`).

**Example:** `[2,7,4,1,8,1]` -> `1`

**Idea:** Max-heap. Pop two, push back `a - b` if non-zero.

**Complexity:** Time `O(n log n)`, Space `O(n)`

### Medium: K Closest Points to Origin
**Problem:** Return the `k` points closest to `(0,0)`.

**Example:** `points = [[1,3],[-2,2]], k = 1` -> `[[-2,2]]`

**Idea:** Max-heap of size `k` keyed by squared distance (no square root needed). Pop the farthest when the size exceeds `k`. (Quickselect gives `O(n)` average.)

**Complexity:** Time `O(n log k)`, Space `O(k)`

### Hard: Merge k Sorted Lists
**Problem:** Merge `k` sorted linked lists into one sorted list.

**Example:** `[[1,4,5],[1,3,4],[2,6]]` -> `[1,1,2,3,4,4,5,6]`

**Idea:** Min-heap holding the current head of each list. Pop the smallest, append it, and push that node's `next`. (Divide-and-conquer pairwise merging is an alternative.)

**Complexity:** Time `O(N log k)`, Space `O(k)`

---

## 8) Graphs

### Easy: Find if Path Exists in Graph
**Problem:** Is there a path from `source` to `destination` in an undirected graph?

**Example:** `n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2` -> `true`

**Idea:** Build an adjacency list; BFS/DFS from `source` with a visited set.

**Complexity:** Time `O(V + E)`, Space `O(V + E)`

### Medium: Clone Graph
**Problem:** Deep-copy a connected undirected graph given a reference to a node.

**Example:** adjacency list `[[2,4],[1,3],[2,4],[1,3]]` -> an identical but separate graph.

**Idea:** DFS/BFS with a map `original -> clone`. When first visiting a node, create its clone and register it before visiting neighbours, which handles cycles.

**Complexity:** Time `O(V + E)`, Space `O(V)`

### Hard: Critical Connections in a Network
**Problem:** Find all bridges (edges whose removal disconnects the graph).

**Example:** `n = 4, connections = [[0,1],[1,2],[2,0],[1,3]]` -> `[[1,3]]`

**Idea:** Tarjan's algorithm. DFS assigns discovery times `disc[u]` and tracks `low[u]` (earliest reachable discovery time via back edges). A tree edge `(u, v)` is a bridge if `low[v] > disc[u]`. Skip the parent edge.

**Complexity:** Time `O(V + E)`, Space `O(V + E)`

---

## 9) Tries

### Easy: Implement Trie (Prefix Tree)
**Problem:** Implement `insert`, `search`, and `startsWith`.

**Example:** `insert("apple"), search("apple") -> true, search("app") -> false, startsWith("app") -> true`

**Idea:** Each node has children keyed by character and an `isEnd` flag.

**Complexity:** `O(L)` per operation, Space `O(total characters)`

### Medium: Design Add and Search Words Data Structure
**Problem:** Support `addWord(word)` and `search(word)` where `.` matches any one letter.

**Example:** `addWord("bad"), search("b.d") -> true, search("..d") -> true, search("b..d") -> false`

**Idea:** Trie plus DFS for search. On `.`, try every child; otherwise follow the matching child.

**Complexity:** `addWord` `O(L)`; `search` worst case `O(26^L)` with many wildcards

### Hard: Word Search II
**Problem:** Given a board of letters and a list of words, return all words that can be formed by paths of adjacent cells (no cell reused within a word).

**Example:** `board = [["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]], words = ["oath","pea","eat","rain"]` -> `["eat","oath"]`

**Idea:** Build a trie of the words and DFS from each cell, following trie edges so invalid prefixes are pruned immediately. Store the full word at terminal nodes, add it to the result, and clear it to avoid duplicates. Optionally prune empty trie branches.

**Complexity:** Time about `O(m * n * 4 * 3^(L-1))`, Space `O(total word characters)`

---

## 10) Union-Find

### Easy: Number of Provinces
**Problem:** Given an `n x n` adjacency matrix `isConnected`, count groups of directly or indirectly connected cities.

**Example:** `[[1,1,0],[1,1,0],[0,0,1]]` -> `2`

**Idea:** Union connected pairs; each successful union reduces the component count from `n`.

**Complexity:** Time `O(n^2 * alpha(n))`, Space `O(n)`

### Medium: Redundant Connection
**Problem:** A tree with `n` nodes had one extra undirected edge added. Return the edge that can be removed to restore the tree (the last such edge in the input).

**Example:** `[[1,2],[1,3],[2,3]]` -> `[2,3]`

**Idea:** Process edges in order; the first edge whose endpoints already share a root closes a cycle.

**Complexity:** Time `O(n * alpha(n))`, Space `O(n)`

### Hard: Number of Islands II
**Problem:** Starting with a water grid, `positions` turns cells into land one at a time. After each addition, return the number of islands.

**Example:** `m = 3, n = 3, positions = [[0,0],[0,1],[1,2],[2,1]]` -> `[1,1,2,3]`

**Idea:** Map cell `(r, c)` to `r * n + c`. When adding land (ignore duplicates), increment the count, then union with each land neighbour; each successful union decrements the count. Record the count after each addition.

**Complexity:** Time `O(k * alpha(m * n))` for `k` positions, Space `O(m * n)`
