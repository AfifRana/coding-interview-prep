# Problem Solving Ideas (Algorithms)

This document explains *how to think* when solving algorithmic interview questions.
For each pattern, you get:

1. **When to use it**
2. **Why it works**
3. **Step-by-step example**
4. **Complexity notes**
5. **Common mistakes**

---

## 1) Two Pointers

### When to use
- Input is sorted (array/string), or
- You need pairs/subarrays with a target condition, or
- You need in-place partitioning/reversal.

### Why it works
Two indices move with rules that reduce search space from `O(n^2)` to `O(n)` in many pair problems.

### Example: Pair sum in sorted array
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

### Example: Word ladder distance
Each valid word transformation is an edge.
- Start word in queue level 0
- Expand all one-letter transformations
- First hit end word => shortest number of transforms

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
`dp[i] = dp[i-1] + dp[i-2]`
- `dp[1]=1, dp[2]=2`
- `dp[3]=3, dp[4]=5, dp[5]=8`

### Complexity
- Time: often `O(n)` to `O(n^2)`
- Space: optimized from table to rolling vars when possible

### Common mistakes
- Wrong state definition.
- Incorrect base cases.

---

## 9) Greedy

### When to use
- Local best choice can be proven globally optimal.

### Why it works
Certain structures (interval scheduling, Huffman-like choices, jump reachability) support safe local decisions.

### Example: Jump Game
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
All numbers appear twice except one.
`a XOR a = 0`, `a XOR 0 = a`
XOR all elements => unique number remains.

### Complexity
- Time: `O(n)`
- Space: `O(1)`

### Common mistakes
- Operator precedence errors (`&`, `|`, `^`, shifts).

---

## Practical interview workflow

1. Clarify input/output and constraints.
2. Identify likely pattern from constraints.
3. Start with brute force briefly.
4. Optimize with suitable pattern.
5. Validate with edge cases.
6. State time/space clearly.
