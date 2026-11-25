# Problem Solving Ideas (Algorithms)

This document explains *how to think* when solving algorithmic interview questions.
For each pattern, you get:

1. **When to use it**
2. **Why it works**
3. **Step-by-step example**
4. **Complexity notes**
5. **Common mistakes**

These are common foundations, not an exhaustive catalog; interview problems often combine patterns or require adapting them to the constraints.

## 1) Two Pointers

### When to use
- Input is sorted (array/string), or
- You need pairs/subarrays with a target condition, or
- You need in-place partitioning/reversal.

### Why it works
Two indices move with rules that reduce search space from `O(n^2)` to `O(n)` in many pair problems.

### Example: Pair sum in sorted array
**Practice:** [Two Sum II — Input Array Is Sorted](problem-set.md#11-two-sum-ii-input-array-is-sorted-easy)

**Problem:** Given sorted `nums = [1,2,4,6,10]`, find if a pair sums to `8`.

- `left=0` (1), `right=4` (10) → sum=11 (too big) → move `right--`
- `left=0` (1), `right=3` (6) → sum=7 (too small) → move `left++`
- `left=1` (2), `right=3` (6) → sum=8 ✅

### Complexity
- Time: `O(n)`
- Space: `O(1)`

### Common mistakes
- Using two pointers on unsorted data without sorting (or without preserving original index needs).
- Moving both pointers incorrectly when only one condition is met.

---

## 2) Sliding Window

### When to use
- Contiguous subarray/substring problems.
- “Longest/shortest window satisfying condition”.

### Why it works
Instead of recomputing from scratch for each subarray, you expand and shrink one moving window.

### Example: Longest substring without repeating characters
**Practice:** [Longest Substring Without Repeating Characters](problem-set.md#22-longest-substring-without-repeating-characters-medium)

`"abcabcbb"`

- Start window `[a]` length 1
- Expand to `[ab]`, `[abc]` length 3
- Next char `a` repeats → shrink from left until unique again
- Continue; max length remains 3

### Complexity
- Time: usually `O(n)` (each pointer moves at most `n` times)
- Space: `O(k)` for map/set

### Common mistakes
- Forgetting to shrink while condition is invalid.
- Updating answer at wrong time (before/after shrink).

---

## 3) Hash Map / Hash Set

### When to use
- Fast lookup/count/frequency checks.
- Need complement existence (e.g., two-sum style).

### Why it works
Average `O(1)` insert/lookup removes nested loops.

### Example: Two Sum (unsorted)
**Practice:** [Two Sum (Unsorted Array)](problem-set.md#34-two-sum-unsorted-array-easy)

`nums=[2,7,11,15], target=9`
- seen={}
- num=2 → need 7 not in seen → seen[2]=0
- num=7 → need 2 in seen ✅ return indices `[0,1]`

### Complexity
- Time: `O(n)`
- Space: `O(n)`

### Common mistakes
- Not handling duplicates correctly.
- Overwriting needed earlier indices unintentionally.

---

## 4) Binary Search

### When to use
- Sorted array search.
- Monotonic condition search space (“minimum feasible x”, “first true”).

### Why it works
Halves search range each step.

### Example: Find first value >= target
**Practice:** [First Value At Least Target](problem-set.md#44-first-value-at-least-target-medium)

`arr=[1,3,3,5,8], target=4`
- mid=2 (3) < 4 → left=3
- mid=3 (5) >=4 → candidate=3, right=2
- stop, answer index 3 (value 5)

### Complexity
- Time: `O(log n)`
- Space: `O(1)` iterative

### Common mistakes
- Infinite loops from wrong mid/updates.
- Confusing `<=` vs `<` boundaries.

---

## 5) Prefix Sum

### When to use
- Frequent range sum queries.
- Counting subarrays with target sum.

### Why it works
Transforms repeated range computation into constant-time arithmetic.

### Example: Range sum
**Practice:** [Range Sum Query — Immutable](problem-set.md#51-range-sum-query-immutable-easy)

`nums=[3,1,4,1,5]`
Prefix: `[0,3,4,8,9,14]`
Sum from index 1..3 = `prefix[4]-prefix[1] = 9-3 = 6`

### Complexity
- Build: `O(n)`
- Query: `O(1)`

### Common mistakes
- Off-by-one indexing in prefix array.

---

## 6) DFS (Depth-First Search)

### When to use
- Explore full paths/components.
- Tree recursion, backtracking, graph traversal.

### Why it works
Go deep along one path, then backtrack systematically.

### Example: Count islands
**Practice:** [Number of Islands](problem-set.md#62-number-of-islands-medium)

Grid with land/water. For each unvisited land cell:
- DFS mark all connected land
- increment island count once per DFS start

### Complexity
- Time: `O(V+E)` graph, `O(m*n)` grid
- Space: recursion stack / explicit stack

### Common mistakes
- Not marking visited early → repeated work / cycles.

---

## 7) BFS (Breadth-First Search)

### When to use
- Shortest path in unweighted graph.
- Level-order traversal.

### Why it works
Queue expands layer by layer; first time reaching node gives shortest steps.

### Example: All shortest word ladders
**Practice:** [Word Ladder II](problem-set.md#63-word-ladder-ii-hard)

Treat each valid one-letter transformation as an edge. BFS finds the shortest distance; to return every shortest sequence, record all parent words that reach a word at the same BFS depth, then backtrack from the end word.

### Complexity
- Time: `O(V+E)`
- Space: `O(V)`

### Common mistakes
- Forgetting visited set causes revisits/explosion.

---

## 8) Dynamic Programming (DP)

### When to use
- Overlapping subproblems + optimal substructure.
- “min/max/count ways” type questions.

### Why it works
Store solved subproblems to avoid recomputation.

### Example: Climbing stairs
**Practice:** [Climbing Stairs](problem-set.md#71-climbing-stairs-easy)

`dp[i] = dp[i-1] + dp[i-2]`
- `dp[1]=1, dp[2]=2`
- `dp[3]=3, dp[4]=5, dp[5]=8`

### Complexity
- Time: often `O(n)` to `O(n^2)`
- Space: optimized from table to rolling vars when possible

### Common mistakes
- Wrong state definition.
- Incorrect base cases.

### Memoization and partition DP
Memoization stores the result of a recursive state the first time it is computed; bottom-up DP fills states in dependency order. For string segmentation, define `dp[i]` as whether the prefix ending at `i` can be formed, then try dictionary words ending at each position.

**Practice:** [Word Break](problem-set.md#74-word-break-medium)

Partition DP uses the same idea when each transition chooses a split point or final segment. State precisely what the prefix or subrange represents, and account for all legal split points.

---

## 9) Greedy

### When to use
- Local best choice can be proven globally optimal.

### Why it works
Certain structures (interval scheduling, Huffman-like choices, jump reachability) support safe local decisions.

### Example: Jump Game
**Practice:** [Jump Game](problem-set.md#82-jump-game-medium)

Track farthest reachable index.
- If current index > farthest, fail.
- Update `farthest = max(farthest, i + nums[i])`
- If finish loop, success.

### Complexity
- Time: `O(n)`
- Space: `O(1)`

### Common mistakes
- Using greedy without proof (some problems require DP instead).

---

## 10) Heap / Priority Queue

### When to use
- Need repeated min/max extraction.
- Top-k problems.

### Why it works
Maintains partial order with efficient push/pop.

### Example: Top K frequent elements
**Practice:** [Top K Frequent Elements](problem-set.md#92-top-k-frequent-elements-medium)

- Count frequencies via map.
- Push into min-heap of size `k` by frequency.
- If size > k, pop smallest.

### Complexity
- Time: `O(n log k)` typical top-k approach
- Space: `O(n)` map + `O(k)` heap

### Common mistakes
- Using max-heap when min-heap of size `k` is more efficient.

---

## 11) Monotonic Stack

### When to use
- Next greater/smaller element.
- Histogram area, temperature span.

### Why it works
Stack preserves monotonic order so each element is pushed/popped at most once.

### Example: Daily Temperatures
**Practice:** [Daily Temperatures](problem-set.md#102-daily-temperatures-medium)

Maintain decreasing stack of indices.
When current temp is higher, pop until order restored and compute day differences.

### Complexity
- Time: `O(n)` amortized
- Space: `O(n)`

### Common mistakes
- Storing values instead of indices when distance needed.

---

## 12) Backtracking

### When to use
- Generate combinations/permutations/subsets.
- Constraint satisfaction.

### Why it works
Build partial candidate, recurse, and undo choice.

### Example: Combination Sum
**Practice:** [Combination Sum](problem-set.md#112-combination-sum-medium)

- Choose number, recurse with reduced target
- Backtrack and try next number
- Record paths when target reaches 0

### Complexity
- Exponential in worst case (depends on branching/depth)

### Common mistakes
- Forgetting to undo state (pop/unmark visited).

---

## 13) Union-Find (Disjoint Set)

### When to use
- Dynamic connectivity queries.
- Cycle detection in undirected graph.

### Why it works
Efficiently merges groups and checks representative parent.

### Example: Detect cycle
**Practice:** [Redundant Connection](data-structure-problem-set.md#102-redundant-connection-medium)

For each edge `(u,v)`:
- If `find(u)==find(v)` cycle exists
- Else `union(u,v)`

### Complexity
Nearly constant amortized (`α(n)`) with path compression + union by rank.

### Common mistakes
- Missing path compression/rank leading to slower trees.

---

## 14) Topological Sort

### When to use
- Dependency ordering in DAG.

### Why it works
Nodes with indegree 0 can be safely processed first.

### Example: Course schedule order
**Practice:** [Course Schedule II](problem-set.md#132-course-schedule-ii-medium)

- Build graph prerequisites.
- Queue indegree-0 courses.
- Pop, append to order, reduce neighbors indegree.
- If all nodes processed → valid order; else cycle.

### Complexity
- Time: `O(V+E)`
- Space: `O(V+E)`

### Common mistakes
- Not detecting cycle by comparing processed count with total nodes.

---

## 15) Bit Manipulation

### When to use
- Flags, subsets, parity, XOR tricks.

### Why it works
Bit operations are constant-time and memory efficient.

### Example: Single number
**Practice:** [Single Number](problem-set.md#141-single-number-easy)

All numbers appear twice except one.
`a XOR a = 0`, `a XOR 0 = a`
XOR all elements => unique number remains.

### Complexity
- Time: `O(n)`
- Space: `O(1)`

### Common mistakes
- Operator precedence errors (`&`, `|`, `^`, shifts).

---

## 16) Intervals

### When to use
- Inputs describe ranges, meetings, or start/end times.
- You need to merge overlaps, detect conflicts, or allocate resources.

### Why it works
Sorting by start point makes the next interval the only possible candidate to overlap the current merged interval.

### Example: Merge overlapping intervals
**Practice:** [Merge Intervals](problem-set.md#151-merge-intervals-medium)

Sort intervals by start; sweep left to right, extending the last output interval when the next start is at or before its end. Otherwise start a new output interval.

### Complexity
- Time: `O(n log n)` for sorting
- Space: `O(n)` for the output

### Common mistakes
- Assuming input is already sorted.
- Misinterpreting whether touching endpoints count as overlap.

---

## 17) Fast and Slow Pointers

### When to use
- Linked-list cycle detection or finding a midpoint.
- The sequence can be followed by repeatedly applying a next-step function.

### Why it works
Move one pointer by one step and another by two. If a cycle exists, the faster pointer eventually laps the slower one.

### Example: Detect a linked-list cycle
**Practice:** [Linked List Cycle](data-structure-problem-set.md#34-linked-list-cycle-easy)

Advance `slow` once and `fast` twice while both can move. If they meet, there is a cycle; if `fast` reaches the end, there is not.

### Complexity
- Time: `O(n)`
- Space: `O(1)`

### Common mistakes
- Comparing node values instead of node identity.
- Dereferencing `fast.next` without checking that `fast` is non-null.

---

## 18) Divide and Conquer

### When to use
- A problem can be split into smaller independent ranges, solved recursively, then combined.
- Sorting-based counting or balanced recursion is useful.

### Why it works
Each level processes the input in linear time while the number of levels is logarithmic when the split is balanced.

### Example: Count range sums with merge sort
**Practice:** [Count of Range Sum](problem-set.md#53-count-of-range-sum-hard)

Build prefix sums. Recursively count valid ranges in each half, then use sorted halves to count cross-boundary pairs before merging them.

### Complexity
- Time: `O(n log n)`
- Space: `O(n)`

### Common mistakes
- Counting only within each half and missing cross-boundary pairs.
- Failing to restore sorted order during the merge.

---

## 19) Sorting and Selection

### When to use
- Sorting simplifies ordering, grouping, or interval processing.
- You need only the `k`th element or top `k`, not a fully sorted result.

### Why it works
Sorting gives a predictable `O(n log n)` order. Selection partitions around a pivot and can find one rank without fully sorting.

### Example: Find the `k`th largest value
**Practice:** [Kth Largest Element in an Array](problem-set.md#94-kth-largest-element-in-an-array-medium)

Quickselect places a pivot in its final sorted position, then continues only in the partition containing the desired rank. A heap is a useful alternative when `k` is small.

### Complexity
- Sorting: `O(n log n)` time.
- Quickselect: average `O(n)`, worst-case `O(n^2)`; randomized pivots reduce the chance of worst-case behavior.
- Heap of size `k`: `O(n log k)` time.

### Common mistakes
- Forgetting duplicates count as separate ranks.
- Assuming Quickselect always has linear worst-case time.

---

## 20) Weighted Shortest Paths

### When to use
- **BFS:** all edges have the same cost (usually one).
- **Dijkstra:** weights are non-negative.
- **Bellman–Ford:** weights may be negative; you also need to detect a reachable negative-weight cycle.

### Core idea: relaxation
For an edge `u -> v` with weight `w`, a path to `v` through `u` has cost `dist[u] + w`. If that is smaller than the current `dist[v]`, replace `dist[v]` with it. This update is called **relaxing** the edge.

### Example: Dijkstra with non-negative weights
**Practice:** [Dijkstra's Shortest Paths](data-structure-problem-set.md#85-dijkstras-shortest-paths-medium)

Initialize `dist[source] = 0` and all other distances to infinity. Repeatedly remove the vertex with the smallest tentative distance from a min-heap and relax its outgoing edges. Ignore stale heap entries when a better distance has since been found.

Non-negative weights matter: once a vertex is removed with the smallest tentative distance, no later path can make it cheaper. Negative edges break that guarantee.

**Complexity:** `O((V + E) log V)` time and `O(V + E)` space with an adjacency list and binary heap.

### Example: Bellman–Ford with a negative edge
**Practice:** [Bellman-Ford Shortest Paths](data-structure-problem-set.md#86-bellman-ford-shortest-paths-medium)

Use the exercise graph with source `0`:

```text
0 -> 1 (4)
0 -> 2 (5)
1 -> 2 (-2)
2 -> 3 (3)
```

Start with distances `[0, ∞, ∞, ∞, ∞]`. On the first pass, relaxing the edges updates them to `[0, 4, 2, 5, ∞]`: the route to vertex `2` through vertex `1` costs `4 + (-2) = 2`, improving on the direct cost `5`. The next pass makes no changes, so the algorithm can stop early. Unreachable vertex `4` stays at infinity and is returned as `-1`.

Bellman–Ford relaxes every edge up to `V-1` times because a shortest simple path can contain at most `V-1` edges. If a reachable edge can still be relaxed after those passes, a reachable negative-weight cycle exists; distances on such paths have no finite minimum. Unreachable negative cycles do not affect distances from the source.

**Complexity:** `O(VE)` time and `O(V)` extra distance space, in addition to the graph.

### Choosing and implementing the algorithm
- Use BFS for equal-cost edges; it visits vertices in increasing distance layers.
- Use Dijkstra for non-negative weights; use a min-heap to process the next closest vertex.
- Use Bellman–Ford when negative edges are allowed or negative-cycle detection is required.
- A DAG with negative edges can also be solved in `O(V + E)` by relaxing vertices in topological order.

### Common mistakes
- Running Dijkstra on a graph with negative edges.
- Failing to skip relaxations from unreachable vertices; infinity plus a negative value must not create a fake path.
- Updating a distance from an unreachable vertex or using an unsafe finite sentinel that can overflow.
- Forgetting that Bellman–Ford's result depends only on reachable vertices when checking for negative cycles.
- Treating `-1` as infinity internally when `-1` could be a valid distance; use a separate unreachable state or a safely bounded sentinel.

---

## 21) Dynamic Range Queries

### When to use
- The array changes between range-sum or range-aggregate queries.
- A static prefix-sum array is no longer sufficient after updates.

### Why it works
Fenwick trees and segment trees store partial aggregates so updates and range queries touch only logarithmically many nodes.

### Example: Point update and range sum
**Practice:** [Range Sum Query — Mutable](problem-set.md#54-range-sum-query-mutable-medium)

Use a Fenwick tree for point updates and prefix sums; obtain an inclusive range sum by subtracting the prefix before its left endpoint from the prefix through its right endpoint.

### Complexity
- Build: `O(n)` with a linear Fenwick-tree build, or `O(n log n)` by repeated updates.
- Point update and range sum: `O(log n)`.
- Space: `O(n)`.

### Common mistakes
- Mixing inclusive and exclusive endpoints.
- Using a structure that does not support the required update/query operations.

---

## 22) Monotonic Queue

### When to use
- Find a maximum or minimum for every fixed-size sliding window.

### Why it works
Keep indices in a deque with values in decreasing (for maxima) or increasing (for minima) order. Values that cannot be a future answer are removed from the back; expired indices leave from the front.

### Example: Sliding-window maximum
**Practice:** [Sliding Window Maximum](problem-set.md#24-sliding-window-maximum-hard)

The front index is the maximum for the current window. Each index enters and leaves the deque at most once.

### Complexity
- Time: `O(n)`
- Space: `O(k)`

### Common mistakes
- Storing values instead of indices, so expired entries cannot be identified.
- Forgetting to remove indices that fall left of the current window.

---

## 23) String Matching (KMP)

### When to use
- Find a pattern in text when repeated prefixes can cause naive matching to re-check many characters.

### Why it works
The prefix-function (failure table) records the longest proper prefix that is also a suffix. On a mismatch, it lets the search reuse already matched information without moving the text index backward.

### Example: Find the first pattern occurrence
**Practice:** [Find the First Occurrence of a Pattern](problem-set.md#161-find-the-first-occurrence-of-a-pattern-medium)

Build the prefix table for the pattern, then scan the text once while following table links on mismatches.

### Complexity
- Time: `O(n + m)` for text length `n` and pattern length `m`.
- Space: `O(m)`.

### Common mistakes
- Including the full pattern when computing a proper prefix.
- Resetting the matched-prefix length to zero instead of using the failure table.

---

## Practical interview workflow

1. Clarify input/output and constraints.
2. Identify likely pattern from constraints.
3. Start with brute force briefly.
4. Optimize with suitable pattern.
5. Validate with edge cases.
6. State time/space clearly.
