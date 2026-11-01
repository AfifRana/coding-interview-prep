# Problem Set (Algorithms)

Each pattern has one **Easy**, one **Medium**, and one **Hard** problem. Every problem includes a statement, an example, the key idea, and complexity. Read the matching section in [problem-solving-ideas.md](problem-solving-ideas.md) first, then try each problem on your own before reading the approach.

## Table of Contents
1. [Two Pointers](#1-two-pointers)
2. [Sliding Window](#2-sliding-window)
3. [Hash Map / Set](#3-hash-map--set)
4. [Binary Search](#4-binary-search)
5. [Prefix Sum](#5-prefix-sum)
6. [DFS / BFS](#6-dfs--bfs)
7. [Dynamic Programming](#7-dynamic-programming)
8. [Greedy](#8-greedy)
9. [Heap / Priority Queue](#9-heap--priority-queue)
10. [Monotonic Stack](#10-monotonic-stack)
11. [Backtracking](#11-backtracking)
12. [Union-Find](#12-union-find)
13. [Topological Sort](#13-topological-sort)
14. [Bit Manipulation](#14-bit-manipulation)

---

## 1) Two Pointers

### Easy: Two Sum II (sorted array)
**Problem:** Given a 1-indexed sorted array `numbers` and a `target`, return the indices of the two numbers that add up to `target`. Exactly one solution exists.

**Example:** `numbers = [2,7,11,15], target = 9` -> `[1,2]`

**Idea:** `left` at start, `right` at end. If sum is too small move `left++`, if too big move `right--`.

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: 3Sum
**Problem:** Return all unique triplets in `nums` that sum to `0`.

**Example:** `nums = [-1,0,1,2,-1,-4]` -> `[[-1,-1,2],[-1,0,1]]`

**Idea:** Sort. Fix index `i`, then run two pointers on the rest looking for `-nums[i]`. Skip duplicate values for `i`, `left`, and `right` to avoid repeated triplets.

**Complexity:** Time `O(n^2)`, Space `O(1)` extra (ignoring output/sort)

### Hard: Trapping Rain Water
**Problem:** Given `n` non-negative integers representing bar heights, compute how much water is trapped after raining.

**Example:** `height = [0,1,0,2,1,0,1,3,2,1,2,1]` -> `6`

**Idea:** Water above a bar = `min(maxLeft, maxRight) - height`. Keep `left`/`right` pointers and `leftMax`/`rightMax`. Always move the side with the smaller max, because that side's water level is already determined.

**Complexity:** Time `O(n)`, Space `O(1)`

---

## 2) Sliding Window

### Easy: Maximum Average Subarray I
**Problem:** Find the contiguous subarray of length `k` with the maximum average and return that average.

**Example:** `nums = [1,12,-5,-6,50,3], k = 4` -> `12.75`

**Idea:** Compute the sum of the first `k` elements, then slide: add the new element, subtract the one leaving. Track the max sum.

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: Longest Substring Without Repeating Characters
**Problem:** Return the length of the longest substring with all distinct characters.

**Example:** `s = "abcabcbb"` -> `3` (`"abc"`)

**Idea:** Expand `right`; store last-seen index of each char in a map. If a char was seen inside the current window, jump `left` to `lastSeen + 1`. Update the best length each step.

**Complexity:** Time `O(n)`, Space `O(min(n, alphabet))`

### Hard: Minimum Window Substring
**Problem:** Given strings `s` and `t`, return the smallest substring of `s` containing every character of `t` (with multiplicity), or `""`.

**Example:** `s = "ADOBECODEBANC", t = "ABC"` -> `"BANC"`

**Idea:** Count needed chars of `t`. Expand `right` and decrement counts; track how many required chars are satisfied (`formed`). While `formed == required`, record the window and shrink from `left`.

**Complexity:** Time `O(|s| + |t|)`, Space `O(|t|)`

---

## 3) Hash Map / Set

### Easy: Contains Duplicate
**Problem:** Return `true` if any value appears at least twice.

**Example:** `nums = [1,2,3,1]` -> `true`

**Idea:** Insert into a set; if already present, return `true`.

**Complexity:** Time `O(n)`, Space `O(n)`

### Medium: Group Anagrams
**Problem:** Group strings that are anagrams of each other.

**Example:** `["eat","tea","tan","ate","nat","bat"]` -> `[["eat","tea","ate"],["tan","nat"],["bat"]]`

**Idea:** Use a canonical key per word (sorted letters, or a 26-count tuple). Map key -> list of words.

**Complexity:** Time `O(n * k log k)` with sorting (`O(n * k)` with counts), Space `O(n * k)`

### Hard: Substring with Concatenation of All Words
**Problem:** Given `s` and an array `words` of equal length, return all starting indices of substrings that are a concatenation of every word exactly once (any order).

**Example:** `s = "barfoothefoobarman", words = ["foo","bar"]` -> `[0,9]`

**Idea:** Let `w` = word length. For each offset `0..w-1`, slide a window over `s` in steps of `w`, counting words in a map and comparing to the target count. Shrink when a word appears too often or is not in `words`.

**Complexity:** Time `O(n * w)`, Space `O(m * w)` where `m = len(words)`

---

## 4) Binary Search

### Easy: Binary Search
**Problem:** Given a sorted array and `target`, return its index or `-1`.

**Example:** `nums = [-1,0,3,5,9,12], target = 9` -> `4`

**Idea:** Classic `lo`/`hi` loop with `mid = lo + (hi - lo) / 2`.

**Complexity:** Time `O(log n)`, Space `O(1)`

### Medium: Search in Rotated Sorted Array
**Problem:** A sorted array of distinct values was rotated at an unknown pivot. Find `target`'s index or `-1`.

**Example:** `nums = [4,5,6,7,0,1,2], target = 0` -> `4`

**Idea:** At each `mid`, one half is always sorted. Check whether `target` lies in the sorted half; if so go there, otherwise go to the other half.

**Complexity:** Time `O(log n)`, Space `O(1)`

### Hard: Median of Two Sorted Arrays
**Problem:** Find the median of two sorted arrays in `O(log(m + n))`.

**Example:** `nums1 = [1,3], nums2 = [2]` -> `2.0`

**Idea:** Binary search the partition of the smaller array. Choose `i` elements from `A` and `j = (m+n+1)/2 - i` from `B` so the left halves hold half the elements. Valid when `A[i-1] <= B[j]` and `B[j-1] <= A[i]`. Median comes from the border values.

**Complexity:** Time `O(log(min(m, n)))`, Space `O(1)`

---

## 5) Prefix Sum

### Easy: Range Sum Query - Immutable
**Problem:** Design a class that answers many `sumRange(left, right)` queries on a fixed array.

**Example:** `nums = [-2,0,3,-5,2,-1]`; `sumRange(0,2)` -> `1`, `sumRange(2,5)` -> `-1`

**Idea:** Build `prefix[i+1] = prefix[i] + nums[i]`. Answer is `prefix[right+1] - prefix[left]`.

**Complexity:** Build `O(n)`, Query `O(1)`, Space `O(n)`

### Medium: Subarray Sum Equals K
**Problem:** Count contiguous subarrays whose sum equals `k` (values can be negative).

**Example:** `nums = [1,1,1], k = 2` -> `2`

**Idea:** A subarray sum is `prefix[j] - prefix[i]`. Keep a map of prefix-sum frequencies; at each step add `count[prefix - k]` to the answer. Initialise `count[0] = 1`.

**Complexity:** Time `O(n)`, Space `O(n)`

### Hard: Count of Range Sum
**Problem:** Count the number of range sums `S(i, j)` that lie in `[lower, upper]`.

**Example:** `nums = [-2,5,-1], lower = -2, upper = 2` -> `3`

**Idea:** With prefix sums, count pairs `i < j` where `lower <= P[j] - P[i] <= upper`. Use merge sort on the prefix array: while merging, two pointers count how many right-half values fall in `[P[i] + lower, P[i] + upper]` for each left-half value. (A Fenwick tree with coordinate compression also works.)

**Complexity:** Time `O(n log n)`, Space `O(n)`

---

## 6) DFS / BFS

### Easy: Maximum Depth of Binary Tree
**Problem:** Return the depth of a binary tree.

**Example:** `root = [3,9,20,null,null,15,7]` -> `3`

**Idea:** DFS: `1 + max(depth(left), depth(right))`, `0` for null. BFS level count also works.

**Complexity:** Time `O(n)`, Space `O(h)`

### Medium: Number of Islands
**Problem:** Count connected groups of `'1'` (land) in a grid of `'0'`/`'1'` using 4-directional adjacency.

**Example:**
```
1 1 0 0 0
1 1 0 0 0
0 0 1 0 0
0 0 0 1 1
```
-> `3`

**Idea:** Scan cells; on an unvisited `'1'`, increment the count and flood-fill (DFS/BFS), marking cells visited.

**Complexity:** Time `O(m * n)`, Space `O(m * n)` worst case

### Hard: Word Ladder II
**Problem:** Given `beginWord`, `endWord`, and a dictionary, return **all** shortest transformation sequences changing one letter at a time, where each intermediate word is in the dictionary.

**Example:** `begin = "hit", end = "cog", list = ["hot","dot","dog","lot","log","cog"]` -> `[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]`

**Idea:** BFS from `beginWord` to record each word's distance and its parents (only edges going to the next level). Then DFS/backtrack from `endWord` through the parents to rebuild all paths.

**Complexity:** Time roughly `O(N * L^2 + paths)`, Space `O(N * L)`

---

## 7) Dynamic Programming

### Easy: Climbing Stairs
**Problem:** You can climb 1 or 2 steps. How many distinct ways to reach step `n`?

**Example:** `n = 5` -> `8`

**Idea:** `dp[i] = dp[i-1] + dp[i-2]`. Keep only the last two values.

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: Coin Change
**Problem:** Given coin denominations and an `amount`, return the fewest coins needed, or `-1`.

**Example:** `coins = [1,2,5], amount = 11` -> `3` (5+5+1)

**Idea:** `dp[a] = min(dp[a - c] + 1)` over coins `c <= a`, with `dp[0] = 0` and others `Infinity`.

**Complexity:** Time `O(amount * coins)`, Space `O(amount)`

### Hard: Edit Distance
**Problem:** Minimum number of insert/delete/replace operations to convert `word1` to `word2`.

**Example:** `word1 = "horse", word2 = "ros"` -> `3`

**Idea:** `dp[i][j]` = cost for the first `i` chars of `word1` and first `j` of `word2`. If chars match: `dp[i-1][j-1]`. Otherwise `1 + min(dp[i-1][j] (delete), dp[i][j-1] (insert), dp[i-1][j-1] (replace))`. Base: `dp[i][0] = i`, `dp[0][j] = j`.

**Complexity:** Time `O(m * n)`, Space `O(n)` with rolling rows

---

## 8) Greedy

### Easy: Assign Cookies
**Problem:** Each child `i` has greed `g[i]`; each cookie `j` has size `s[j]`. A cookie satisfies a child if `s[j] >= g[i]`. Maximise satisfied children (one cookie each).

**Example:** `g = [1,2,3], s = [1,1]` -> `1`

**Idea:** Sort both. Use the smallest cookie that can satisfy the least greedy unsatisfied child.

**Complexity:** Time `O(n log n + m log m)`, Space `O(1)`

### Medium: Jump Game
**Problem:** `nums[i]` is the max jump length from index `i`. Can you reach the last index from index 0?

**Example:** `[2,3,1,1,4]` -> `true`; `[3,2,1,0,4]` -> `false`

**Idea:** Track `farthest`. If `i > farthest` return `false`; otherwise `farthest = max(farthest, i + nums[i])`.

**Complexity:** Time `O(n)`, Space `O(1)`

### Hard: Candy
**Problem:** Children stand in a line with ratings. Each child needs at least 1 candy, and a child with a higher rating than a neighbour must get more candy than that neighbour. Return the minimum candies.

**Example:** `ratings = [1,0,2]` -> `5` (2,1,2)

**Idea:** Two passes. Left-to-right: if `r[i] > r[i-1]`, `candy[i] = candy[i-1] + 1`. Right-to-left: if `r[i] > r[i+1]`, `candy[i] = max(candy[i], candy[i+1] + 1)`. Sum.

**Complexity:** Time `O(n)`, Space `O(n)`

---

## 9) Heap / Priority Queue

### Easy: Kth Largest Element in a Stream
**Problem:** Design a class that returns the `k`-th largest element after each `add(val)`.

**Example:** `k = 3`, initial `[4,5,8,2]`; `add(3)` -> `4`, `add(5)` -> `5`, `add(10)` -> `5`

**Idea:** Maintain a min-heap of size `k`. The top is the answer. Push, and pop if size exceeds `k`.

**Complexity:** `add` is `O(log k)`, Space `O(k)`

### Medium: Top K Frequent Elements
**Problem:** Return the `k` most frequent elements.

**Example:** `nums = [1,1,1,2,2,3], k = 2` -> `[1,2]`

**Idea:** Count frequencies, keep a min-heap of size `k` ordered by frequency. (Bucket sort by frequency gives `O(n)`.)

**Complexity:** Time `O(n log k)`, Space `O(n)`

### Hard: Find Median from Data Stream
**Problem:** Support `addNum(x)` and `findMedian()` over a growing stream.

**Example:** add 1, add 2 -> `1.5`; add 3 -> `2.0`

**Idea:** Two heaps: a max-heap `low` for the smaller half and a min-heap `high` for the larger half. Keep `len(low) >= len(high)` and `len(low) - len(high) <= 1`, and `max(low) <= min(high)`. Median is `low.top` or the average of both tops.

**Complexity:** `addNum` `O(log n)`, `findMedian` `O(1)`

---

## 10) Monotonic Stack

### Easy: Next Greater Element I
**Problem:** For each element of `nums1` (a subset of `nums2`), find the next greater element to its right in `nums2`, or `-1`.

**Example:** `nums1 = [4,1,2], nums2 = [1,3,4,2]` -> `[-1,3,-1]`

**Idea:** Scan `nums2` with a decreasing stack; when a bigger value arrives, pop and record it as the answer for popped values in a map. Look up `nums1` in the map.

**Complexity:** Time `O(n + m)`, Space `O(n)`

### Medium: Daily Temperatures
**Problem:** For each day, how many days until a warmer temperature? Use `0` if none.

**Example:** `[73,74,75,71,69,72,76,73]` -> `[1,1,4,2,1,1,0,0]`

**Idea:** Stack of indices with decreasing temperatures. When `temp[i] > temp[top]`, pop `top` and set `answer[top] = i - top`.

**Complexity:** Time `O(n)`, Space `O(n)`

### Hard: Largest Rectangle in Histogram
**Problem:** Given bar heights (width 1), find the area of the largest rectangle.

**Example:** `heights = [2,1,5,6,2,3]` -> `10`

**Idea:** Keep an increasing stack of indices. When a shorter bar arrives, pop: the popped bar's height spans from the new stack top + 1 to `i - 1`. Append a sentinel `0` bar at the end to flush the stack.

**Complexity:** Time `O(n)`, Space `O(n)`

---

## 11) Backtracking

### Easy: Binary Watch
**Problem:** A binary watch has 4 LEDs for hours and 6 for minutes. Given `turnedOn` LEDs, return all possible times.

**Example:** `turnedOn = 1` -> `["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]`

**Idea:** Enumerate hours `0..11` and minutes `0..59`; keep pairs where `popcount(h) + popcount(m) == turnedOn`. (Or choose `turnedOn` LEDs by backtracking.)

**Complexity:** Time `O(1)` (fixed 720 combos)

### Medium: Combination Sum
**Problem:** Given distinct `candidates` and a `target`, return all unique combinations summing to `target`. A number may be reused.

**Example:** `candidates = [2,3,6,7], target = 7` -> `[[2,2,3],[7]]`

**Idea:** `dfs(start, remaining, path)`. Try each candidate from `start` onward (this avoids duplicates), recurse with the same index to allow reuse, and pop after returning. Stop when `remaining < 0`; record when `remaining == 0`.

**Complexity:** Time exponential, about `O(2^t)` bounded by `target / min(candidates)` depth; Space `O(depth)`

### Hard: N-Queens
**Problem:** Place `n` queens on an `n x n` board so no two attack each other. Return all distinct boards.

**Example:** `n = 4` -> 2 solutions, e.g. `[".Q..","...Q","Q...","..Q."]`

**Idea:** Place one queen per row. Track used columns, `row - col` diagonals, and `row + col` anti-diagonals in sets. For each column in the row, skip if conflicting, place, recurse, then undo.

**Complexity:** Time about `O(n!)`, Space `O(n)`

---

## 12) Union-Find

### Easy: Find if Path Exists in Graph
**Problem:** Given `n` nodes and undirected edges, is there a path from `source` to `destination`?

**Example:** `n = 3, edges = [[0,1],[1,2],[2,0]], source = 0, destination = 2` -> `true`

**Idea:** Union all edges, then check `find(source) == find(destination)`.

**Complexity:** Time `O(n + e * alpha(n))`, Space `O(n)`

### Medium: Number of Connected Components in an Undirected Graph
**Problem:** Count components given `n` nodes and edges.

**Example:** `n = 5, edges = [[0,1],[1,2],[3,4]]` -> `2`

**Idea:** Start with `n` components; every successful `union` (different roots) decrements the count.

**Complexity:** Time `O(n + e * alpha(n))`, Space `O(n)`

### Hard: Redundant Connection II
**Problem:** A rooted tree (directed, each node has one parent) had one extra directed edge added. Return the edge to remove so it is a valid rooted tree again (if several answers, the one last in the input).

**Example:** `edges = [[1,2],[1,3],[2,3]]` -> `[2,3]`

**Idea:** Two issues can occur: a node with two parents (candidates `A` and `B`), and/or a cycle. Find a node with two parents; skip `B` and run union-find. If a cycle still forms, answer is `A` (if there was a two-parent node) or the cycle-forming edge. If no cycle forms, answer is `B`. With no two-parent node, the answer is the edge that closes the cycle.

**Complexity:** Time `O(n * alpha(n))`, Space `O(n)`

---

## 13) Topological Sort

### Easy: Course Schedule (can finish?)
**Problem:** Given `numCourses` and prerequisite pairs `[a, b]` (take `b` before `a`), can all courses be finished?

**Example:** `numCourses = 2, prerequisites = [[1,0]]` -> `true`; `[[1,0],[0,1]]` -> `false`

**Idea:** Kahn's algorithm. Build indegrees, process indegree-0 nodes, and count processed nodes. If the count equals `numCourses`, there is no cycle.

**Complexity:** Time `O(V + E)`, Space `O(V + E)`

### Medium: Course Schedule II
**Problem:** Return a valid order to take all courses, or an empty array if impossible.

**Example:** `numCourses = 4, prerequisites = [[1,0],[2,0],[3,1],[3,2]]` -> `[0,1,2,3]` (or `[0,2,1,3]`)

**Idea:** Same as above, but append each popped node to the result. Return the result only if its length is `numCourses`.

**Complexity:** Time `O(V + E)`, Space `O(V + E)`

### Hard: Alien Dictionary
**Problem:** Given words sorted in an alien language's lexicographic order, derive a valid letter order, or `""` if invalid.

**Example:** `words = ["wrt","wrf","er","ett","rftt"]` -> `"wertf"`

**Idea:** Compare adjacent words; the first differing characters give an edge `c1 -> c2`. Watch for the invalid case where a longer word is a prefix-preceded by a shorter one (`["abc","ab"]`). Topologically sort all distinct letters; a cycle means invalid.

**Complexity:** Time `O(C)` total characters, Space `O(U + E)` where `U` is unique letters

---

## 14) Bit Manipulation

### Easy: Single Number
**Problem:** Every element appears twice except one. Find it in `O(n)` time and `O(1)` space.

**Example:** `nums = [4,1,2,1,2]` -> `4`

**Idea:** XOR all numbers. Pairs cancel (`a ^ a = 0`).

**Complexity:** Time `O(n)`, Space `O(1)`

### Medium: Single Number III
**Problem:** Exactly two elements appear once; all others appear twice. Return the two.

**Example:** `nums = [1,2,1,3,2,5]` -> `[3,5]`

**Idea:** XOR everything to get `x = a ^ b`. Isolate a set bit `diff = x & -x`. Partition numbers by that bit and XOR each group separately.

**Complexity:** Time `O(n)`, Space `O(1)`

### Hard: Maximum XOR of Two Numbers in an Array
**Problem:** Find the maximum `nums[i] XOR nums[j]`.

**Example:** `nums = [3,10,5,25,2,8]` -> `28` (5 XOR 25)

**Idea:** Build a binary trie of the numbers (MSB first). For each number, walk the trie preferring the opposite bit at each level to maximise the XOR. (A prefix-set approach, building the answer bit by bit, also works.)

**Complexity:** Time `O(32 * n)`, Space `O(32 * n)`
