# Data Structure Problem Set

Implement each function from its statement and constraints. The listed examples and test cases specify input/output behavior; values after an in-place operation are ignored unless explicitly requested.

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

### 1.1 Remove Duplicates from Sorted Array (Easy)

Given a non-decreasing integer array `nums`, remove duplicates in place so each distinct value occurs once. Return `k`, the number of distinct values; the first `k` positions must contain those values in their original sorted order. Values after position `k-1` are ignored.

**Function:** `removeDuplicates(nums: integer[]) -> integer`

**Examples:** `[1,1,2] -> k = 2, nums prefix = [1,2]`; `[0,0,1,1,1,2,2,3,3,4] -> k = 5, prefix = [0,1,2,3,4]`.

**Constraints:** `1 <= nums.length <= 3 * 10^4`; `-100 <= nums[i] <= 100`; `nums` is sorted.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[1]}` | `{"k":1,"prefix":[1]}` | One value |
| `{"nums":[1,1]}` | `{"k":1,"prefix":[1]}` | All equal |
| `{"nums":[1,1,2]}` | `{"k":2,"prefix":[1,2]}` | Example |
| `{"nums":[0,0,1,1,1,2,2,3,3,4]}` | `{"k":5,"prefix":[0,1,2,3,4]}` | Several duplicates |
| `{"nums":[-3,-3,-1,0,0,4]}` | `{"k":4,"prefix":[-3,-1,0,4]}` | Negative values |
| `{"nums":[-100,-100,100]}` | `{"k":2,"prefix":[-100,100]}` | Value boundaries |

### 1.2 Product of Array Except Self (Medium)

Given an integer array `nums`, return an array `answer` where `answer[i]` is the product of every element of `nums` except `nums[i]`. Do not use division. Products fit in a signed 32-bit integer.

**Function:** `productExceptSelf(nums: integer[]) -> integer[]`

**Examples:** `[1,2,3,4] -> [24,12,8,6]`; `[-1,1,0,-3,3] -> [0,0,9,0,0]`.

**Constraints:** `2 <= nums.length <= 10^5`; `-30 <= nums[i] <= 30`; products fit 32-bit signed integer.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[1,2]}` | `[2,1]` | Minimum length |
| `{"nums":[1,2,3,4]}` | `[24,12,8,6]` | Positive values |
| `{"nums":[-1,1,0,-3,3]}` | `[0,0,9,0,0]` | One zero and negatives |
| `{"nums":[0,0,2]}` | `[0,0,0]` | Multiple zeros |
| `{"nums":[-2,-3,-4]}` | `[12,8,6]` | Negative products |
| `{"nums":[1,1,1,1]}` | `[1,1,1,1]` | All ones |

### 1.3 First Missing Positive (Hard)

Given an unsorted integer array `nums`, return the smallest positive integer that does not occur in `nums`. Your solution must run in `O(n)` time and use `O(1)` additional space.

**Function:** `firstMissingPositive(nums: integer[]) -> integer`

**Examples:** `[1,2,0] -> 3`; `[3,4,-1,1] -> 2`.

**Constraints:** `1 <= nums.length <= 10^5`; `-2^31 <= nums[i] <= 2^31-1`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"nums":[1]}` | `2` | Smallest array |
| `{"nums":[1,2,0]}` | `3` | Consecutive positives |
| `{"nums":[3,4,-1,1]}` | `2` | Negative and misplaced values |
| `{"nums":[7,8,9,11,12]}` | `1` | Missing first positive |
| `{"nums":[1,1]}` | `2` | Duplicate |
| `{"nums":[2,2,2,2]}` | `1` | No one present |
| `{"nums":[-3,-2,0]}` | `1` | No positive values |

## 2) Strings

### 2.1 Valid Palindrome (Easy)

Return `true` if `s` is a palindrome after converting uppercase English letters to lowercase and removing every character that is not an English letter or digit. The empty normalized string is a palindrome.

**Function:** `isPalindrome(s: string) -> boolean`

**Examples:** `"A man, a plan, a canal: Panama" -> true`; `"race a car" -> false`.

**Constraints:** `1 <= s.length <= 2 * 10^5`; `s` contains printable ASCII characters.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"s":"A"}` | `true` | Single character |
| `{"s":"A man, a plan, a canal: Panama"}` | `true` | Ignore punctuation and case |
| `{"s":"race a car"}` | `false` | Not a palindrome |
| `{"s":" "}` | `true` | Normalizes to empty |
| `{"s":"0P"}` | `false` | Digits are retained |
| `{"s":".,!"}` | `true` | Only punctuation |

### 2.2 Longest Palindromic Substring (Medium)

Given a string `s`, return its longest contiguous substring that is a palindrome. If multiple answers have the same maximum length, return the leftmost one.

**Function:** `longestPalindrome(s: string) -> string`

**Examples:** `"babad" -> "bab"`; `"cbbd" -> "bb"`.

**Constraints:** `1 <= s.length <= 1000`; `s` consists of English letters and digits.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"s":"a"}` | `"a"` | Single character |
| `{"s":"babad"}` | `"bab"` | Equal-length ties, leftmost |
| `{"s":"cbbd"}` | `"bb"` | Even-length palindrome |
| `{"s":"aaaa"}` | `"aaaa"` | Entire string is a palindrome |
| `{"s":"abc"}` | `"a"` | No longer palindrome |
| `{"s":"forgeeksskeegfor"}` | `"geeksskeeg"` | Palindrome inside string |

### 2.3 Regular Expression Matching (Hard)

Implement full-string matching for text `s` and pattern `p`. `.` matches any single character. `*` matches zero or more occurrences of the immediately preceding pattern element. The pattern is valid and `*` never appears first or consecutively. Return whether the entire text matches.

**Function:** `isMatch(s: string, p: string) -> boolean`

**Examples:** `{"s":"aa","p":"a"} -> false`; `{"s":"aa","p":"a*"} -> true`; `{"s":"ab","p":".*"} -> true`.

**Constraints:** `0 <= s.length <= 20`; `1 <= p.length <= 30`; `s` contains lowercase letters; `p` contains lowercase letters, `.` and `*`; pattern is valid.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"s":"","p":"a*"}` | `true` | Star matches zero |
| `{"s":"aa","p":"a"}` | `false` | Pattern too short |
| `{"s":"aa","p":"a*"}` | `true` | Repeated preceding element |
| `{"s":"ab","p":".*"}` | `true` | Wildcard repeated |
| `{"s":"aab","p":"c*a*b"}` | `true` | Multiple zero/repeat choices |
| `{"s":"mississippi","p":"mis*is*p*."}` | `false` | Near match fails |
| `{"s":"ab","p":" .*"}` | `false` | Space is a literal character |

## 3) Linked Lists

For these problems, a linked list is represented by an integer array in head-to-tail order. Return the resulting list as an array.

### 3.1 Reverse Linked List (Easy)

Given the array representation of a singly linked list, reverse its node order.

**Function:** `reverseList(values: integer[]) -> integer[]`

**Examples:** `[1,2,3,4,5] -> [5,4,3,2,1]`; `[1,2] -> [2,1]`.

**Constraints:** `0 <= values.length <= 5000`; `-5000 <= values[i] <= 5000`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"values":[]}` | `[]` | Empty list |
| `{"values":[1]}` | `[1]` | Single node |
| `{"values":[1,2]}` | `[2,1]` | Two nodes |
| `{"values":[1,2,3,4,5]}` | `[5,4,3,2,1]` | Odd-length list |
| `{"values":[-3,0,3,9]}` | `[9,3,0,-3]` | Negative and zero |

### 3.2 Add Two Numbers (Medium)

Two non-empty linked lists represent non-negative integers. Digits are stored in reverse order, and each node contains one digit. Add the represented numbers and return the sum in the same reverse-order representation. Inputs have no leading zero except the number zero.

**Function:** `addTwoNumbers(a: integer[], b: integer[]) -> integer[]`

**Examples:** `[2,4,3] + [5,6,4] -> [7,0,8]`; `[0] + [0] -> [0]`.

**Constraints:** `1 <= a.length,b.length <= 100`; each digit is in `0..9`; each list represents a valid number without leading zero.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"a":[0],"b":[0]}` | `[0]` | Zero plus zero |
| `{"a":[2,4,3],"b":[5,6,4]}` | `[7,0,8]` | Carry across digits |
| `{"a":[9,9,9],"b":[1]}` | `[0,0,0,1]` | Final carry adds a node |
| `{"a":[1],"b":[9,9]}` | `[0,0,1]` | Different list lengths |
| `{"a":[5],"b":[5]}` | `[0,1]` | Single-digit carry |

### 3.3 Reverse Nodes in k-Group (Hard)

Given a linked list and positive integer `k`, reverse nodes in consecutive groups of size `k`. Any final group containing fewer than `k` nodes must remain in its original order.

**Function:** `reverseKGroup(values: integer[], k: integer) -> integer[]`

**Examples:** `[1,2,3,4,5], k=2 -> [2,1,4,3,5]`; `[1,2,3,4,5], k=3 -> [3,2,1,4,5]`.

**Constraints:** `0 <= values.length <= 5000`; `1 <= k <= 5000`; `-1000 <= values[i] <= 1000`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"values":[],"k":1}` | `[]` | Empty list |
| `{"values":[1],"k":1}` | `[1]` | Group size one |
| `{"values":[1,2],"k":2}` | `[2,1]` | Exact group |
| `{"values":[1,2,3,4,5],"k":2}` | `[2,1,4,3,5]` | Leftover node |
| `{"values":[1,2,3,4,5],"k":3}` | `[3,2,1,4,5]` | Incomplete group unchanged |
| `{"values":[1,2,3],"k":4}` | `[1,2,3]` | k exceeds length |

## 4) Stacks / Queues

### 4.1 Valid Parentheses (Easy)

Given a string containing only `(`, `)`, `{`, `}`, `[` and `]`, determine whether every opening bracket is closed by the same type of bracket and in the correct order.

**Function:** `isValid(s: string) -> boolean`

**Examples:** `"()" -> true`; `"()[]{}" -> true`; `"(]" -> false`.

**Constraints:** `1 <= s.length <= 10^4`; `s` contains only bracket characters.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"s":"()"}` | `true` | One pair |
| `{"s":"()[]{}"}` | `true` | Multiple types |
| `{"s":"(]"}` | `false` | Mismatched type |
| `{"s":"([])"}` | `true` | Proper nesting |
| `{"s":"([)]"}` | `false` | Wrong order |
| `{"s":"(((("}` | `false` | Unclosed brackets |
| `{"s":")("}` | `false` | Closing bracket first |

### 4.2 Implement Queue Using Stacks (Medium)

Implement a FIFO queue using only stack operations (`push`, `pop`, `top`, `empty`). Support `push(x)`, `pop()`, `peek()`, and `empty()`. Inputs guarantee `pop` and `peek` are called only when non-empty. Return one output for each operation: inserted values for `push`, removed/front values for `pop`/`peek`, and booleans for `empty`.

**Function:** `queueWithStacks(operations: string[], arguments: integer[][]) -> value[]`

**Examples:** operations `["push","push","peek","pop","empty"]`, arguments `[[1],[2],[],[],[]]` -> `[null,null,1,1,false]`.

**Constraints:** `1 <= operations.length <= 100`; values are in `[-100,100]`; operations are valid.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"operations":["push","pop"],"arguments":[[1],[]]}` | `[null,1]` | One item |
| `{"operations":["push","push","peek","pop","empty"],"arguments":[[1],[2],[],[],[]]}` | `[null,null,1,1,false]` | FIFO behavior |
| `{"operations":["empty"],"arguments":[[]]}` | `[true]` | Initially empty |
| `{"operations":["push","push","pop","push","pop","pop"],"arguments":[[1],[2],[],[3],[],[]]}` | `[null,null,1,null,2,3]` | Refill after pop |
| `{"operations":["push","push","push","pop","peek"],"arguments":[[-1],[0],[5],[],[]]}` | `[null,null,null,-1,0]` | Negative and zero |

### 4.3 Largest Rectangle in Histogram (Hard)

Given non-negative histogram bar heights, where each bar has width one, return the area of the largest rectangle contained in the histogram.

**Function:** `largestRectangleArea(heights: integer[]) -> integer`

**Examples:** `[2,1,5,6,2,3] -> 10`; `[2,4] -> 4`.

**Constraints:** `1 <= heights.length <= 10^5`; `0 <= heights[i] <= 10^4`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"heights":[0]}` | `0` | Zero height |
| `{"heights":[2,4]}` | `4` | Two bars |
| `{"heights":[2,1,5,6,2,3]}` | `10` | Maximum spans two bars |
| `{"heights":[2,2,2,2]}` | `8` | Equal bars |
| `{"heights":[1,2,3,4]}` | `6` | Increasing heights |
| `{"heights":[4,3,2,1]}` | `6` | Decreasing heights |

## 5) Hash Tables

### 5.1 Two Sum (Easy)

Given an integer array `nums` and an integer `target`, return the two distinct indices whose values sum to `target`. Exactly one solution exists. Return the indices in increasing order.

**Function:** `twoSum(nums: integer[], target: integer) -> integer[2]`

**Examples:** `[2,7,11,15], target=9 -> [0,1]`; `[3,2,4], target=6 -> [1,2]`.

**Constraints:** `2 <= nums.length <= 10^4`; `-10^9 <= nums[i], target <= 10^9`; exactly one solution; an element cannot be reused.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[2,7],"target":9}` | `[0,1]` | Minimum length |
| `{"nums":[2,7,11,15],"target":9}` | `[0,1]` | Example |
| `{"nums":[3,2,4],"target":6}` | `[1,2]` | Pair not adjacent |
| `{"nums":[3,3],"target":6}` | `[0,1]` | Duplicate values, separate indices |
| `{"nums":[-3,4,3,90],"target":0}` | `[0,2]` | Negative value |
| `{"nums":[-1000000000,1000000000],"target":0}` | `[0,1]` | Value boundaries |

### 5.2 LRU Cache (Medium)

Design a least-recently-used cache with positive capacity. `get(key)` returns its value or `-1` if absent. `put(key,value)` inserts or updates a key. When insertion makes the cache exceed capacity, evict the least recently used key. A successful `get` and every `put` make that key most recently used. Return `null` for each `put`, and the returned value for each `get`.

**Function:** `runLRU(capacity: integer, operations: string[], arguments: integer[][]) -> value[]`

**Example:** capacity `2`; operations `["put","put","get","put","get","put","get","get","get"]`; arguments `[[1,1],[2,2],[1],[3,3],[2],[4,4],[1],[3],[4]]` -> `[null,null,1,null,-1,null,-1,3,4]`.

**Constraints:** `1 <= capacity <= 3000`; up to `2 * 10^5` operations; keys and values are integers.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"capacity":1,"operations":["put","get"],"arguments":[[1,10],[1]]}` | `[null,10]` | Capacity one |
| `{"capacity":2,"operations":["put","put","get","put","get","put","get","get","get"],"arguments":[[1,1],[2,2],[1],[3,3],[2],[4,4],[1],[3],[4]]}` | `[null,null,1,null,-1,null,-1,3,4]` | Access changes eviction order |
| `{"capacity":2,"operations":["put","put","put","get"],"arguments":[[1,1],[1,2],[2,3],[1]]}` | `[null,null,null,2]` | Updating existing key |
| `{"capacity":2,"operations":["get","put","get"],"arguments":[[5],[5,7],[5]]}` | `[-1,null,7]` | Missing key then insert |
| `{"capacity":3,"operations":["put","put","put","get","put","get"],"arguments":[[-1,4],[0,5],[2,6],[-1],[3,7],[0]]}` | `[null,null,null,4,null,-1]` | Negative key and eviction |

### 5.3 All O'one Data Structure (Hard)

Design a structure supporting `inc(key)`, `dec(key)`, `getMaxKey()`, and `getMinKey()`. `inc` creates an absent key at count 1 or increments its count. `dec` decrements a present key and removes it at count 0. For `dec`, the key is guaranteed to exist. Return an empty string from a get operation when the structure is empty. Test cases query min/max only when the corresponding key is unique.

**Function:** `runAllOne(operations: string[], arguments: string[][]) -> string[]`

**Example:** operations `["inc","inc","inc","getMaxKey","getMinKey","dec","getMaxKey","getMinKey"]`, arguments `[["a"],["b"],["b"],[],[],["a"],[],[]]` -> `["","","","b","a","","b","b"]`; mutating operations return empty output strings.

**Constraints:** `1 <= operations.length <= 5 * 10^4`; keys are non-empty lowercase strings of length at most 10; `dec` targets an existing key.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"operations":["getMaxKey","getMinKey"],"arguments":[[],[]]}` | `["",""]` | Empty structure |
| `{"operations":["inc","getMaxKey","getMinKey"],"arguments":[["a"],[],[]]}` | `["","a","a"]` | One key |
| `{"operations":["inc","inc","inc","getMaxKey","getMinKey"],"arguments":[["a"],["b"],["b"],[],[]]}` | `["","","","b","a"]` | Unique max and min |
| `{"operations":["inc","inc","dec","getMaxKey","getMinKey"],"arguments":[["a"],["a"],["a"],[],[]]}` | `["","","","a","a"]` | Decrement count |
| `{"operations":["inc","dec","getMaxKey","getMinKey"],"arguments":[["x"],["x"],[],[]]}` | `["","","",""]` | Key removed, structure empty |

## 6) Trees

Trees are represented as level-order arrays, using `null` for missing children.

### 6.1 Symmetric Tree (Easy)

Given the root of a binary tree, return whether it is a mirror of itself around its center.

**Function:** `isSymmetric(levelOrder: value[]) -> boolean`

**Examples:** `[1,2,2,3,4,4,3] -> true`; `[1,2,2,null,3,null,3] -> false`.

**Constraints:** `1 <= node count <= 1000`; node values are in `[-100,100]`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"tree":[1]}` | `true` | One node |
| `{"tree":[1,2,2,3,4,4,3]}` | `true` | Symmetric tree |
| `{"tree":[1,2,2,null,3,null,3]}` | `false` | Shape mismatch |
| `{"tree":[1,2,3]}` | `false` | Different child values |
| `{"tree":[1,2,2,2,null,2]}` | `false` | Unequal depth |

### 6.2 Binary Tree Level Order Traversal (Medium)

Given a binary tree, return its node values grouped by level from top to bottom, and left to right within each level. An empty tree returns an empty list.

**Function:** `levelOrder(levelOrderInput: value[]) -> integer[][]`

**Examples:** `[3,9,20,null,null,15,7] -> [[3],[9,20],[15,7]]`; `[1] -> [[1]]`.

**Constraints:** `0 <= node count <= 2000`; node values are in `[-1000,1000]`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"tree":[]}` | `[]` | Empty tree |
| `{"tree":[1]}` | `[[1]]` | One level |
| `{"tree":[3,9,20,null,null,15,7]}` | `[[3],[9,20],[15,7]]` | Multiple levels |
| `{"tree":[1,2,3,4,5]}` | `[[1],[2,3],[4,5]]` | Incomplete final level |
| `{"tree":[-1,-2,-3]}` | `[[-1],[-2,-3]]` | Negative values |

### 6.3 Serialize and Deserialize Binary Tree (Hard)

Implement `serialize` to encode a binary tree and `deserialize` to reconstruct it. For each test, deserialize the serialization of the input tree and return the reconstructed level-order array, removing trailing `null` entries. Your serialized representation may use any format, but it must preserve tree shape and values.

**Function:** `roundTripTree(tree: value[]) -> value[]`

**Examples:** `[1,2,3,null,null,4,5] -> [1,2,3,null,null,4,5]`; `[] -> []`.

**Constraints:** `0 <= node count <= 10^4`; `-1000 <= node.val <= 1000`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"tree":[]}` | `[]` | Empty tree |
| `{"tree":[1]}` | `[1]` | Single node |
| `{"tree":[1,2,3,null,null,4,5]}` | `[1,2,3,null,null,4,5]` | Missing children |
| `{"tree":[-1,-2,3]}` | `[-1,-2,3]` | Negative values |
| `{"tree":[1,null,2,null,3]}` | `[1,null,2,null,3]` | Skewed tree |

## 7) Heaps

### 7.1 Last Stone Weight (Easy)

Given stone weights, repeatedly choose the two heaviest stones `x <= y`. If equal, both are destroyed. Otherwise replace them with a stone of weight `y-x`. Return the final stone weight, or zero if none remain.

**Function:** `lastStoneWeight(stones: integer[]) -> integer`

**Examples:** `[2,7,4,1,8,1] -> 1`; `[1] -> 1`.

**Constraints:** `1 <= stones.length <= 30`; `1 <= stones[i] <= 1000`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"stones":[1]}` | `1` | One stone |
| `{"stones":[2,7,4,1,8,1]}` | `1` | Multiple collisions |
| `{"stones":[2,2]}` | `0` | Equal pair disappears |
| `{"stones":[10,4]}` | `6` | Unequal pair |
| `{"stones":[3,3,3]}` | `3` | Odd number of equal stones |

### 7.2 K Closest Points to Origin (Medium)

Given points with integer coordinates and integer `k`, return the `k` points with the smallest squared Euclidean distance from the origin. Sort returned points by increasing squared distance, then increasing x-coordinate, then increasing y-coordinate.

**Function:** `kClosest(points: integer[][], k: integer) -> integer[][]`

**Examples:** `[[1,3],[-2,2]], k=1 -> [[-2,2]]`; `[[3,3],[5,-1],[-2,4]], k=2 -> [[3,3],[-2,4]]`.

**Constraints:** `1 <= k <= points.length <= 10^4`; `-10^4 <= x,y <= 10^4`; points are distinct.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"points":[[1,1]],"k":1}` | `[[1,1]]` | Single point |
| `{"points":[[1,3],[-2,2]],"k":1}` | `[[-2,2]]` | Nearest point |
| `{"points":[[3,3],[5,-1],[-2,4]],"k":2}` | `[[3,3],[-2,4]]` | Distance ordering |
| `{"points":[[1,0],[-1,0],[0,1]],"k":3}` | `[[-1,0],[0,1],[1,0]]` | Tie order by x then y |
| `{"points":[[0,0],[2,0],[0,2]],"k":1}` | `[[0,0]]` | Origin |
| `{"points":[[-10000,0],[0,9999]],"k":1}` | `[[0,9999]]` | Large coordinates |

### 7.3 Merge k Sorted Lists (Hard)

Given `k` sorted linked lists, each represented as an integer array, merge them into one sorted list and return it.

**Function:** `mergeKLists(lists: integer[][]) -> integer[]`

**Examples:** `[[1,4,5],[1,3,4],[2,6]] -> [1,1,2,3,4,4,5,6]`; `[] -> []`.

**Constraints:** `0 <= lists.length <= 10^4`; total number of nodes is `0..10^4`; values are in `[-10^4,10^4]`; each list is sorted non-decreasingly.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"lists":[]}` | `[]` | No lists |
| `{"lists":[[]]}` | `[]` | One empty list |
| `{"lists":[[1]]}` | `[1]` | One node |
| `{"lists":[[1,4,5],[1,3,4],[2,6]]}` | `[1,1,2,3,4,4,5,6]` | Standard merge |
| `{"lists":[[],[-2,0],[-2,3]]}` | `[-2,-2,0,3]` | Empty list and negatives |
| `{"lists":[[1,1],[1,1]]}` | `[1,1,1,1]` | Duplicate values |

## 8) Graphs

### 8.1 Find if Path Exists in Graph (Easy)

Given an undirected graph with vertices `0..n-1` and an edge list, determine whether a path exists between `source` and `destination`. A vertex is reachable from itself.

**Function:** `validPath(n: integer, edges: integer[][], source: integer, destination: integer) -> boolean`

**Examples:** `n=3, edges=[[0,1],[1,2]], source=0, destination=2 -> true`; `n=6, edges=[[0,1],[0,2],[3,5],[5,4],[4,3]], source=0, destination=5 -> false`.

**Constraints:** `1 <= n <= 2 * 10^5`; up to `2 * 10^5` unique undirected edges; endpoints are valid and distinct.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"n":1,"edges":[],"source":0,"destination":0}` | `true` | Same vertex |
| `{"n":2,"edges":[],"source":0,"destination":1}` | `false` | Disconnected |
| `{"n":3,"edges":[[0,1],[1,2]],"source":0,"destination":2}` | `true` | Multi-edge path |
| `{"n":6,"edges":[[0,1],[0,2],[3,5],[5,4],[4,3]],"source":0,"destination":5}` | `false` | Separate components |

### 8.2 Clone Graph (Medium)

Given a connected undirected graph represented by its adjacency list, return a deep copy. The clone must have the same node labels and neighbor relationships, and must not reuse any node object from the input. Return the clone's adjacency list in original node-label order; each neighbor list is sorted ascending. An empty graph returns an empty adjacency list.

**Function:** `cloneGraph(adjacency: integer[][]) -> integer[][]`

**Examples:** `[[2,4],[1,3],[2,4],[1,3]] -> [[2,4],[1,3],[2,4],[1,3]]`; `[] -> []`.

**Constraints:** `0 <= node count <= 100`; labels are consecutive integers `1..n`; graph is undirected, connected when non-empty, and has no self-edges.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"adjacency":[]}` | `[]` | Empty graph |
| `{"adjacency":[[]]}` | `[[]]` | Single isolated node |
| `{"adjacency":[[2],[1]]}` | `[[2],[1]]` | Two connected nodes |
| `{"adjacency":[[2,4],[1,3],[2,4],[1,3]]}` | `[[2,4],[1,3],[2,4],[1,3]]` | Cycle with shared neighbors |
| `{"adjacency":[[2,3],[1,3],[1,2]]}` | `[[2,3],[1,3],[1,2]]` | Triangle |

### 8.3 Critical Connections in a Network (Hard)

An undirected connected network has `n` vertices labeled `0..n-1`. A connection is critical if removing it disconnects the network. Return every critical connection as `[u,v]` with `u < v`, sorted lexicographically.

**Function:** `criticalConnections(n: integer, connections: integer[][]) -> integer[][]`

**Examples:** `n=4, connections=[[0,1],[1,2],[2,0],[1,3]] -> [[1,3]]`; `n=2, connections=[[0,1]] -> [[0,1]]`.

**Constraints:** `2 <= n <= 10^5`; `n-1 <= connections.length <= 2 * 10^5`; no duplicate edges; graph is connected.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"n":2,"connections":[[0,1]]}` | `[[0,1]]` | Single bridge |
| `{"n":4,"connections":[[0,1],[1,2],[2,0],[1,3]]}` | `[[1,3]]` | Bridge attached to cycle |
| `{"n":5,"connections":[[0,1],[1,2],[2,3],[3,4]]}` | `[[0,1],[1,2],[2,3],[3,4]]` | Every edge is critical |
| `{"n":4,"connections":[[0,1],[1,2],[2,0],[0,3],[3,2]]}` | `[]` | No bridge |
| `{"n":5,"connections":[[0,1],[1,2],[2,0],[1,3],[3,4]]}` | `[[1,3],[3,4]]` | Multiple bridges, sorted |

## 9) Tries

### 9.1 Implement Trie (Prefix Tree) (Easy)

Implement a trie with `insert(word)`, `search(word)`, and `startsWith(prefix)`. `search` is true only when the full word was inserted; `startsWith` is true when at least one inserted word begins with the prefix. Return `null` for inserts and booleans for queries.

**Function:** `runTrie(operations: string[], arguments: string[][]) -> value[]`

**Example:** operations `["insert","search","search","startsWith","insert","search"]`, arguments `[["apple"],["apple"],["app"],["app"],["app"],["app"]]` -> `[null,true,false,true,null,true]`.

**Constraints:** `1 <= operations.length <= 3 * 10^4`; words and prefixes contain lowercase English letters and have length at most 2000.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"operations":["insert","search"],"arguments":[["a"],["a"]]}` | `[null,true]` | Single-letter word |
| `{"operations":["insert","search","startsWith"],"arguments":[["apple"],["app"],["app"]]}` | `[null,false,true]` | Prefix is not full word |
| `{"operations":["insert","insert","search","startsWith"],"arguments":[["app"],["apple"],["app"],["appl"]]}` | `[null,null,true,true]` | Shared prefix |
| `{"operations":["search","startsWith"],"arguments":[["missing"],["m"]]}` | `[false,false]` | No inserted words |

### 9.2 Design Add and Search Words Data Structure (Medium)

Implement `addWord(word)` and `search(word)`. Search patterns contain lowercase letters and `.`; a dot matches any one lowercase letter. Matching must cover the entire stored word.

**Function:** `runWordDictionary(operations: string[], arguments: string[][]) -> value[]`

**Examples:** add `"bad"`, `"dad"`, `"mad"`; search `"pad" -> false`, `"bad" -> true`, `".ad" -> true`, `"b.." -> true`.

**Constraints:** `1 <= operations.length <= 10^4`; words and patterns have length `1..25`; lowercase letters and `.` only.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"operations":["addWord","search"],"arguments":[["a"],["a"]]}` | `[null,true]` | Exact one-character word |
| `{"operations":["addWord","search"],"arguments":[["bad"],["pad"]]}` | `[null,false]` | Word not present |
| `{"operations":["addWord","addWord","addWord","search","search","search","search"],"arguments":[["bad"],["dad"],["mad"],["pad"],["bad"],[".ad"],["b.."]]}` | `[null,null,null,false,true,true,true]` | Wildcard at different positions |
| `{"operations":["addWord","search","search"],"arguments":[["a"],["."],[".."]]}` | `[null,true,false]` | Wildcard matches exactly one char |

### 9.3 Word Search II (Hard)

Given an `m x n` board of lowercase letters and a list of distinct words, return every word that can be formed by a path of horizontally or vertically adjacent cells. A cell cannot be reused within one word. Return found words sorted lexicographically.

**Function:** `findWords(board: string[][], words: string[]) -> string[]`

**Examples:** board `[["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]]`, words `["oath","pea","eat","rain"]` -> `["eat","oath"]`; board `[["a","b"],["c","d"]]`, words `["abcb"]` -> `[]`.

**Constraints:** `1 <= m,n <= 12`; `1 <= words.length <= 3 * 10^4`; word length `1..10`; lowercase letters; words are distinct.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"board":[["a"]],"words":["a"]}` | `["a"]` | One-cell board |
| `{"board":[["a"]],"words":["b"]}` | `[]` | No match |
| `{"board":[["o","a","a","n"],["e","t","a","e"],["i","h","k","r"],["i","f","l","v"]],"words":["oath","pea","eat","rain"]}` | `["eat","oath"]` | Multiple words, sorted |
| `{"board":[["a","b"],["c","d"]],"words":["abcb"]}` | `[]` | Would require reusing a cell |
| `{"board":[["a","b"],["a","a"]],"words":["aa","aba","ba"]}` | `["aa","aba","ba"]` | Shared prefixes and paths |

## 10) Union-Find

### 10.1 Number of Provinces (Easy)

Given an `n x n` adjacency matrix `isConnected`, where `isConnected[i][j] == 1` means cities `i` and `j` are directly connected, return the number of connected groups of cities. The matrix is symmetric and every city is connected to itself.

**Function:** `findCircleNum(isConnected: integer[][]) -> integer`

**Examples:** `[[1,1,0],[1,1,0],[0,0,1]] -> 2`; `[[1,0,0],[0,1,0],[0,0,1]] -> 3`.

**Constraints:** `1 <= n <= 200`; matrix dimensions are `n x n`; entries are 0 or 1; matrix is symmetric.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"isConnected":[[1]]}` | `1` | One city |
| `{"isConnected":[[1,0],[0,1]]}` | `2` | No intercity connections |
| `{"isConnected":[[1,1],[1,1]]}` | `1` | Connected pair |
| `{"isConnected":[[1,1,0],[1,1,0],[0,0,1]]}` | `2` | One pair and one singleton |
| `{"isConnected":[[1,1,0],[1,1,1],[0,1,1]]}` | `1` | Transitive connectivity |

### 10.2 Redundant Connection (Medium)

A tree with vertices labeled `1..n` had one extra undirected edge added. Return the last edge in input order that can be removed to leave a tree. The input has exactly one cycle.

**Function:** `findRedundantConnection(edges: integer[][]) -> integer[2]`

**Examples:** `[[1,2],[1,3],[2,3]] -> [2,3]`; `[[1,2],[2,3],[3,4],[1,4],[1,5]] -> [1,4]`.

**Constraints:** `3 <= edges.length <= 1000`; labels are `1..n`; edges are unique; graph is connected after adding the extra edge.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"edges":[[1,2],[1,3],[2,3]]}` | `[2,3]` | Triangle |
| `{"edges":[[1,2],[2,3],[3,4],[1,4],[1,5]]}` | `[1,4]` | Cycle and attached node |
| `{"edges":[[1,2],[2,3],[3,1]]}` | `[3,1]` | Last input edge closes cycle |
| `{"edges":[[1,2],[2,3],[3,4],[4,5],[5,1]]}` | `[5,1]` | Larger cycle |

### 10.3 Number of Islands II (Hard)

An initially all-water `m x n` grid receives land additions in order. After each position `[r,c]`, return the number of islands, where islands are connected horizontally or vertically. Repeated additions of an already-land cell do not change the count.

**Function:** `numIslands2(m: integer, n: integer, positions: integer[][]) -> integer[]`

**Examples:** `m=3,n=3, positions=[[0,0],[0,1],[1,2],[2,1]] -> [1,1,2,3]`; `m=1,n=1, positions=[[0,0],[0,0]] -> [1,1]`.

**Constraints:** `1 <= m,n <= 10^4`; `1 <= m*n <= 10^5`; `1 <= positions.length <= 10^4`; positions are in bounds.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"m":1,"n":1,"positions":[[0,0]]}` | `[1]` | Single cell |
| `{"m":1,"n":2,"positions":[[0,0],[0,1]]}` | `[1,1]` | Adjacent additions merge |
| `{"m":3,"n":3,"positions":[[0,0],[0,1],[1,2],[2,1]]}` | `[1,1,2,3]` | Separate islands |
| `{"m":1,"n":1,"positions":[[0,0],[0,0]]}` | `[1,1]` | Duplicate position |
| `{"m":2,"n":2,"positions":[[0,0],[1,1],[0,1],[1,0]]}` | `[1,2,1,1]` | One cell connects components |
| `{"m":3,"n":3,"positions":[[1,1],[0,1],[1,0],[2,1],[1,2]]}` | `[1,1,1,1,1]` | Land expands around center |
