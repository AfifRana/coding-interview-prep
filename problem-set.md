# Algorithm Problem Set

For each prompt, implement the named function. The examples and test tables specify expected behavior; your solution should satisfy the full statement and constraints. Outputs with multiple valid answers use the canonical ordering stated in the problem.

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

### 1.1 Two Sum II — Input Array Is Sorted (Easy)

Given a 1-indexed array of integers `numbers`, sorted in non-decreasing order, and an integer `target`, return the indices of two distinct elements whose values sum to `target`. Exactly one valid pair exists. Return the smaller index first.

**Function:** `twoSum(numbers: integer[], target: integer) -> integer[2]`

**Example 1**

- **Input:** `{"numbers":[2,7,11,15],"target":9}`
- **Output:** `[1,2]`

**Example 2**

- **Input:** `{"numbers":[2,3,4],"target":6}`
- **Output:** `[1,3]`

**Constraints:** `2 <= numbers.length <= 30000`; `-1000 <= numbers[i] <= 1000`; `numbers` is sorted non-decreasingly; `-1000 <= target <= 1000`; exactly one solution exists; the same element cannot be used twice.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"numbers":[1,2],"target":3}` | `[1,2]` | Minimum length |
| `{"numbers":[2,7,11,15],"target":9}` | `[1,2]` | First pair |
| `{"numbers":[2,3,4],"target":6}` | `[1,3]` | Pair skips middle |
| `{"numbers":[-5,-2,0,3,8],"target":1}` | `[2,4]` | Negative values |
| `{"numbers":[1,1,3,5],"target":2}` | `[1,2]` | Duplicate values, distinct positions |
| `{"numbers":[-10,-3,4,9],"target":6}` | `[2,4]` | Mixed signs |
| `{"numbers":[0,0,1,2],"target":0}` | `[1,2]` | Duplicate zeroes |
| `{"numbers":[-1000,0,1000],"target":0}` | `[1,3]` | Value boundaries |

### 1.2 3Sum (Medium)

Given an integer array `nums`, return every unique triplet `[a,b,c]` of distinct array elements whose sum is zero. Each triplet must be sorted in non-decreasing order; the result must contain no duplicate triplets and be sorted lexicographically. Return an empty array when no triplet exists.

**Function:** `threeSum(nums: integer[]) -> integer[][]`

**Example 1**

- **Input:** `{"nums":[-1,0,1,2,-1,-4]}`
- **Output:** `[[-1,-1,2],[-1,0,1]]`

**Example 2**

- **Input:** `{"nums":[0,1,1]}`
- **Output:** `[]`

**Constraints:** `3 <= nums.length <= 3000`; `-100000 <= nums[i] <= 100000`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[0,0,0]}` | `[[0,0,0]]` | Minimum size, repeated values |
| `{"nums":[0,1,1]}` | `[]` | No solution |
| `{"nums":[-1,0,1,2,-1,-4]}` | `[[-1,-1,2],[-1,0,1]]` | Multiple unique triplets |
| `{"nums":[-2,0,1,1,2]}` | `[[-2,0,2],[-2,1,1]]` | Duplicate candidates |
| `{"nums":[-4,-2,-1,0,1,2,3,5]}` | `[[-4,-1,5],[-4,1,3],[-2,-1,3],[-2,0,2],[-1,0,1]]` | Lexicographic result order |
| `{"nums":[-3,1,2]}` | `[[-3,1,2]]` | Single solution |
| `{"nums":[-2,-2,2,2]}` | `[]` | Insufficient balancing values |
| `{"nums":[-1,-1,-1,0,1,1,1]}` | `[[-1,0,1]]` | Many duplicate combinations |

### 1.3 Trapping Rain Water (Hard)

Given `n` non-negative integers `height`, where each value is the height of a vertical bar of width one, return the total number of units of water that can be trapped after raining. Water above a bar is bounded by the tallest bars to its left and right; bars at the ends cannot hold water.

**Function:** `trap(height: integer[]) -> integer`

**Example 1**

- **Input:** `{"height":[0,1,0,2,1,0,1,3,2,1,2,1]}`
- **Output:** `6`

**Example 2**

- **Input:** `{"height":[4,2,0,3,2,5]}`
- **Output:** `9`

**Constraints:** `1 <= height.length <= 20000`; `0 <= height[i] <= 100000`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"height":[0]}` | `0` | Minimum length |
| `{"height":[0,1]}` | `0` | No interior bar |
| `{"height":[1,1,1]}` | `0` | Flat boundary |
| `{"height":[3,0,3]}` | `3` | Single basin |
| `{"height":[0,1,0,2,1,0,1,3,2,1,2,1]}` | `6` | Multiple basins |
| `{"height":[4,2,0,3,2,5]}` | `9` | Uneven basin |
| `{"height":[5,4,1,2]}` | `1` | Descending boundary |
| `{"height":[2,0,2,0,2]}` | `4` | Repeated basins |

## 2) Sliding Window

### 2.1 Maximum Average Subarray I (Easy)

Given an integer array `nums` and an integer `k`, find a contiguous subarray of length exactly `k` with the maximum average value. Return that maximum average as a number. The answer is accepted if its absolute error is at most `10^-5`.

**Function:** `findMaxAverage(nums: integer[], k: integer) -> number`

**Example 1**

- **Input:** `{"nums":[1,12,-5,-6,50,3],"k":4}`
- **Output:** `12.75`

**Example 2**

- **Input:** `{"nums":[5],"k":1}`
- **Output:** `5.0`

**Constraints:** `1 <= k <= nums.length <= 100000`; `-10000 <= nums[i] <= 10000`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[5],"k":1}` | `5.0` | Minimum length |
| `{"nums":[1,2],"k":2}` | `1.5` | Window is entire array |
| `{"nums":[1,12,-5,-6,50,3],"k":4}` | `12.75` | Mixed values |
| `{"nums":[-1,-2,-3,-4],"k":2}` | `-1.5` | All negative |
| `{"nums":[-5,0,5],"k":1}` | `5.0` | Negative and positive |
| `{"nums":[4,4,4,4],"k":3}` | `4.0` | Equal window averages |
| `{"nums":[-10000,10000,-10000],"k":2}` | `0.0` | Extreme signs |
| `{"nums":[3,-1,2,7,-4,8],"k":3}` | `3.6666666666666665` | Fractional maximum average |

### 2.2 Longest Substring Without Repeating Characters (Medium)

Given a string `s`, return the length of its longest contiguous substring in which every character occurs at most once. Treat characters as the input's individual characters; the empty string has answer zero.

**Function:** `lengthOfLongestSubstring(s: string) -> integer`

**Example 1**

- **Input:** `{"s":"abcabcbb"}`
- **Output:** `3`

**Example 2**

- **Input:** `{"s":"bbbbb"}`
- **Output:** `1`

**Constraints:** `0 <= s.length <= 50000`; `s` consists of English letters, digits, symbols, and spaces.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"s":""}` | `0` | Empty string |
| `{"s":"a"}` | `1` | Single character |
| `{"s":"abcdef"}` | `6` | All distinct |
| `{"s":"bbbbb"}` | `1` | Repeated character |
| `{"s":"abcabcbb"}` | `3` | Repetition after distinct prefix |
| `{"s":"pwwkew"}` | `3` | Best substring is not a subsequence |
| `{"s":"abba"}` | `2` | Duplicate within current window |
| `{"s":"a b!a"}` | `4` | Spaces and symbols |

### 2.3 Minimum Window Substring (Hard)

Given strings `s` and `t`, return the shortest substring of `s` that contains every character in `t` with at least the multiplicity it has in `t`. If several shortest windows exist, return the leftmost. Return the empty string if no such window exists. When `t` is empty, return the empty string.

**Function:** `minWindow(s: string, t: string) -> string`

**Example 1**

- **Input:** `{"s":"ADOBECODEBANC","t":"ABC"}`
- **Output:** `"BANC"`

**Example 2**

- **Input:** `{"s":"a","t":"a"}`
- **Output:** `"a"`

**Constraints:** `0 <= s.length,t.length <= 100000`; `s` and `t` contain uppercase/lowercase English letters.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"s":"a","t":"a"}` | `"a"` | Minimum matching window |
| `{"s":"a","t":"aa"}` | `""` | Multiplicity makes solution impossible |
| `{"s":"ADOBECODEBANC","t":"ABC"}` | `"BANC"` | Shortest window after irrelevant characters |
| `{"s":"aa","t":"aa"}` | `"aa"` | Required duplicate characters |
| `{"s":"abxxac","t":"ac"}` | `"ac"` | Leftmost shortest match |
| `{"s":"xyz","t":""}` | `""` | Empty target |
| `{"s":"","t":"a"}` | `""` | Empty source |
| `{"s":"bbaa","t":"ab"}` | `"ba"` | Required chars in either order |

## 3) Hash Map

### 3.1 Contains Duplicate (Easy)

Given an integer array `nums`, return `true` if any value appears at least twice, and `false` if every value is distinct.

**Function:** `containsDuplicate(nums: integer[]) -> boolean`

**Example 1**

- **Input:** `{"nums":[1,2,3,1]}`
- **Output:** `true`

**Example 2**

- **Input:** `{"nums":[1,2,3,4]}`
- **Output:** `false`

**Constraints:** `1 <= nums.length <= 100000`; `-10^9 <= nums[i] <= 10^9`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[1]}` | `false` | Minimum size |
| `{"nums":[1,2,3,4]}` | `false` | All distinct |
| `{"nums":[1,2,3,1]}` | `true` | Repeated value |
| `{"nums":[-1,-1]}` | `true` | Repeated negative |
| `{"nums":[0,1,0]}` | `true` | Repeated zero |
| `{"nums":[-3,-2,-1]}` | `false` | Distinct negative values |
| `{"nums":[1000000000,-1000000000]}` | `false` | Value boundaries |
| `{"nums":[5,4,3,2,1,5]}` | `true` | Duplicate at distant positions |

### 3.2 Group Anagrams (Medium)

Given an array of lowercase English strings `strs`, group strings that are anagrams of one another. Each input string must appear in exactly one group. For canonical output here, preserve the original order of strings within each group and order groups by the first occurrence of any member in the input; this ordering does not alter group membership.

**Function:** `groupAnagrams(strs: string[]) -> string[][]`

**Example 1**

- **Input:** `{"strs":["eat","tea","tan","ate","nat","bat"]}`
- **Output:** `[["eat","tea","ate"],["tan","nat"],["bat"]]`

**Example 2**

- **Input:** `{"strs":[""]}`
- **Output:** `[[""]]`

**Constraints:** `1 <= strs.length <= 10000`; `0 <= strs[i].length <= 100`; `strs[i]` contains lowercase English letters only.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"strs":[""]}` | `[[""]]` | Empty string |
| `{"strs":["a"]}` | `[["a"]]` | Single item |
| `{"strs":["eat","tea","tan","ate","nat","bat"]}` | `[["eat","tea","ate"],["tan","nat"],["bat"]]` | Multiple groups, stable canonical order |
| `{"strs":["ab","ba","ab"]}` | `[["ab","ba","ab"]]` | Repeated identical anagram |
| `{"strs":["a","b","c"]}` | `[["a"],["b"],["c"]]` | No anagram pairs |
| `{"strs":["","","a"]}` | `[["",""],["a"]]` | Repeated empty strings |
| `{"strs":["abc","bca","cab","acb"]}` | `[["abc","bca","cab","acb"]]` | Same letters, varied order |
| `{"strs":["listen","silent","enlist","google","gogole"]}` | `[["listen","silent","enlist"],["google","gogole"]]` | Distinct signatures |

### 3.3 Substring with Concatenation of All Words (Hard)

Given a string `s` and an array `words` of equal-length strings, find every starting index in `s` where a contiguous substring is a concatenation of each word in `words` exactly once, in any order, with no gaps. Return indices in strictly increasing order. Words may repeat, in which case each occurrence is required.

**Function:** `findSubstring(s: string, words: string[]) -> integer[]`

**Example 1**

- **Input:** `{"s":"barfoothefoobarman","words":["foo","bar"]}`
- **Output:** `[0,9]`

**Example 2**

- **Input:** `{"s":"wordgoodgoodgoodbestword","words":["word","good","best","word"]}`
- **Output:** `[]`

**Constraints:** `1 <= s.length <= 10000`; `1 <= words.length <= 5000`; `1 <= words[i].length <= 30`; all words have equal length; `s` and words contain lowercase English letters.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"s":"a","words":["a"]}` | `[0]` | Minimum match |
| `{"s":"abc","words":["x"]}` | `[]` | No matching word |
| `{"s":"barfoothefoobarman","words":["foo","bar"]}` | `[0,9]` | Two matches in index order |
| `{"s":"wordgoodgoodgoodbestword","words":["word","good","best","word"]}` | `[]` | Repeated required word unavailable |
| `{"s":"barfoofoobarthefoobarman","words":["bar","foo","the"]}` | `[6,9,12]` | Overlapping valid starts |
| `{"s":"aaaaaa","words":["aa","aa"]}` | `[0,1,2]` | Repeated words and overlap |
| `{"s":"catdogcat","words":["cat","dog"]}` | `[0,3]` | Both word orders |
| `{"s":"foobarbaz","words":["foo","bar"]}` | `[0]` | Match at beginning, trailing text |

## 4) Binary Search

### 4.1 Binary Search (Easy)

Given an array of distinct integers `nums` sorted in ascending order and an integer `target`, return the index of `target`, or `-1` if it is not present.

**Function:** `search(nums: integer[], target: integer) -> integer`

**Example 1**

- **Input:** `{"nums":[-1,0,3,5,9,12],"target":9}`
- **Output:** `4`

**Example 2**

- **Input:** `{"nums":[-1,0,3,5,9,12],"target":2}`
- **Output:** `-1`

**Constraints:** `1 <= nums.length <= 10000`; `-10000 <= nums[i], target <= 10000`; all elements are distinct; `nums` is ascending.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[5],"target":5}` | `0` | Minimum size, present |
| `{"nums":[5],"target":2}` | `-1` | Minimum size, absent |
| `{"nums":[-1,0,3,5,9,12],"target":9}` | `4` | Interior match |
| `{"nums":[-1,0,3,5,9,12],"target":2}` | `-1` | Missing interior value |
| `{"nums":[1,3,5,7],"target":1}` | `0` | First element |
| `{"nums":[1,3,5,7],"target":7}` | `3` | Last element |
| `{"nums":[-9,-4,-1,2,8],"target":-4}` | `1` | Negative match |
| `{"nums":[-9,-4,-1,2,8],"target":9}` | `-1` | Above range |

### 4.2 Search in Rotated Sorted Array (Medium)

An array of distinct integers was sorted in ascending order and then rotated at an unknown pivot. Given that array and `target`, return its index, or `-1` if it is absent. The array contains no duplicates.

**Function:** `searchRotated(nums: integer[], target: integer) -> integer`

**Example 1**

- **Input:** `{"nums":[4,5,6,7,0,1,2],"target":0}`
- **Output:** `4`

**Example 2**

- **Input:** `{"nums":[4,5,6,7,0,1,2],"target":3}`
- **Output:** `-1`

**Constraints:** `1 <= nums.length <= 5000`; `-10000 <= nums[i], target <= 10000`; values are distinct; the input is a rotation of a strictly ascending array.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[1],"target":1}` | `0` | Minimum size, present |
| `{"nums":[1],"target":0}` | `-1` | Minimum size, absent |
| `{"nums":[4,5,6,7,0,1,2],"target":0}` | `4` | Match after pivot |
| `{"nums":[4,5,6,7,0,1,2],"target":3}` | `-1` | Missing target |
| `{"nums":[1,2,3,4,5],"target":4}` | `3` | Unrotated array |
| `{"nums":[5,1,3],"target":5}` | `0` | Match at left boundary |
| `{"nums":[6,7,1,2,3,4,5],"target":5}` | `6` | Match near right boundary |
| `{"nums":[-4,-1,0,3,8],"target":-1}` | `1` | Negative values, no rotation |

### 4.3 Median of Two Sorted Arrays (Hard)

Given two arrays `nums1` and `nums2`, each sorted in non-decreasing order, return the median of all elements in both arrays. If the combined length is even, return the arithmetic mean of the two middle values. The combined input is non-empty.

**Function:** `findMedianSortedArrays(nums1: integer[], nums2: integer[]) -> number`

**Example 1**

- **Input:** `{"nums1":[1,3],"nums2":[2]}`
- **Output:** `2.0`

**Example 2**

- **Input:** `{"nums1":[1,2],"nums2":[3,4]}`
- **Output:** `2.5`

**Constraints:** `0 <= nums1.length, nums2.length <= 1000`; `1 <= nums1.length + nums2.length <= 2000`; `-10^6 <= nums1[i], nums2[i] <= 10^6`; both arrays are sorted.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums1":[],"nums2":[1]}` | `1.0` | One combined element |
| `{"nums1":[1],"nums2":[]}` | `1.0` | Empty second array |
| `{"nums1":[1,3],"nums2":[2]}` | `2.0` | Odd total length |
| `{"nums1":[1,2],"nums2":[3,4]}` | `2.5` | Even total length |
| `{"nums1":[0,0],"nums2":[0,0]}` | `0.0` | Duplicate values |
| `{"nums1":[-5,-3],"nums2":[-4,-2,-1]}` | `-3.0` | Negative values |
| `{"nums1":[1,2,3,4,5],"nums2":[6,7,8,9]}` | `5.0` | Unequal lengths |
| `{"nums1":[1,100],"nums2":[2,3,4,5,6]}` | `4.0` | Skewed distributions |

## 5) Prefix Sum

### 5.1 Range Sum Query — Immutable (Easy)

Given an integer array `nums`, answer multiple inclusive range-sum queries. For each query `[left,right]`, return the sum of `nums[left]` through `nums[right]`.

**Function:** `rangeSumQuery(nums: integer[], queries: integer[][]) -> integer[]`

**Example 1**

- **Input:** `{"nums":[-2,0,3,-5,2,-1],"queries":[[0,2],[2,5],[0,5]]}`
- **Output:** `[1,-1,-3]`

**Example 2**

- **Input:** `{"nums":[5],"queries":[[0,0]]}`
- **Output:** `[5]`

**Constraints:** `1 <= nums.length <= 10000`; `-100000 <= nums[i] <= 100000`; `1 <= queries.length <= 10000`; `0 <= left <= right < nums.length`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[5],"queries":[[0,0]]}` | `[5]` | Minimum array and singleton range |
| `{"nums":[1,2,3],"queries":[[0,2]]}` | `[6]` | Entire array |
| `{"nums":[-2,0,3,-5,2,-1],"queries":[[0,2],[2,5],[0,5]]}` | `[1,-1,-3]` | Multiple overlapping ranges |
| `{"nums":[1,2,3,4],"queries":[[1,1],[2,3]]}` | `[2,7]` | Singleton and partial ranges |
| `{"nums":[-1,-2,-3],"queries":[[0,1],[1,2]]}` | `[-3,-5]` | Negative sums |
| `{"nums":[0,0,0],"queries":[[0,2],[1,1]]}` | `[0,0]` | Zero values |
| `{"nums":[100000,-100000],"queries":[[0,1],[1,1]]}` | `[0,-100000]` | Opposite signs |
| `{"nums":[4,-1,7,2],"queries":[[3,3],[0,2],[1,3]]}` | `[2,10,8]` | Boundary indices |

### 5.2 Subarray Sum Equals K (Medium)

Given an integer array `nums` and integer `k`, return the number of non-empty contiguous subarrays whose elements sum to exactly `k`. Negative values and zeroes are allowed.

**Function:** `subarraySum(nums: integer[], k: integer) -> integer`

**Example 1**

- **Input:** `{"nums":[1,1,1],"k":2}`
- **Output:** `2`

**Example 2**

- **Input:** `{"nums":[1,2,3],"k":3}`
- **Output:** `2`

**Constraints:** `1 <= nums.length <= 20000`; `-1000 <= nums[i] <= 1000`; `-10^7 <= k <= 10^7`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[1],"k":1}` | `1` | Minimum size, match |
| `{"nums":[1],"k":0}` | `0` | No solution |
| `{"nums":[1,1,1],"k":2}` | `2` | Overlapping matches |
| `{"nums":[1,2,3],"k":3}` | `2` | Singleton and longer match |
| `{"nums":[1,-1,0],"k":0}` | `3` | Zero-sum subarrays |
| `{"nums":[-1,-1,1],"k":-1}` | `3` | Negative target |
| `{"nums":[0,0,0],"k":0}` | `6` | All zeroes, all ranges |
| `{"nums":[2,-2,2,-2],"k":2}` | `3` | Repeated prefix sums |

### 5.3 Count of Range Sum (Hard)

Given an integer array `nums` and integers `lower` and `upper`, return the number of non-empty contiguous subarrays whose sum lies inclusively between `lower` and `upper`.

**Function:** `countRangeSum(nums: integer[], lower: integer, upper: integer) -> integer`

**Example 1**

- **Input:** `{"nums":[-2,5,-1],"lower":-2,"upper":2}`
- **Output:** `3`

**Example 2**

- **Input:** `{"nums":[0],"lower":0,"upper":0}`
- **Output:** `1`

**Constraints:** `1 <= nums.length <= 1000`; `-2^31 <= nums[i] <= 2^31-1`; `-10^5 <= lower <= upper <= 10^5`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[0],"lower":0,"upper":0}` | `1` | Minimum size, inclusive equal bounds |
| `{"nums":[1],"lower":2,"upper":3}` | `0` | No valid range |
| `{"nums":[-2,5,-1],"lower":-2,"upper":2}` | `3` | Canonical mixed-sign example |
| `{"nums":[1,2,3],"lower":3,"upper":3}` | `2` | Exact-boundary sums |
| `{"nums":[-1,-1],"lower":-2,"upper":-1}` | `3` | Negative ranges |
| `{"nums":[0,0,0],"lower":0,"upper":0}` | `6` | All subarrays qualify |
| `{"nums":[2,-2,2],"lower":-1,"upper":1}` | `2` | Prefix cancellation |
| `{"nums":[-3,4,-2,1],"lower":-3,"upper":0}` | `5` | Inclusive bounds and mixed signs |

## 6) DFS/BFS

### 6.1 Maximum Depth of Binary Tree (Easy)

Given the root of a binary tree, return its maximum depth: the number of nodes on the longest path from the root down to a leaf. An empty tree has depth zero. Tree inputs below use level-order arrays, with `null` marking missing children and trailing null entries omitted.

**Function:** `maxDepth(root: TreeNode | null) -> integer`

**Example 1**

- **Input:** `{"root":[3,9,20,null,null,15,7]}`
- **Output:** `3`

**Example 2**

- **Input:** `{"root":[]}`
- **Output:** `0`

**Constraints:** The number of nodes is in `[0, 10^4]`; `-100 <= Node.val <= 100`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"root":[]}` | `0` | Empty tree |
| `{"root":[1]}` | `1` | Single node |
| `{"root":[1,2]}` | `2` | One-sided tree |
| `{"root":[1,null,2]}` | `2` | Right-only child |
| `{"root":[3,9,20,null,null,15,7]}` | `3` | Balanced root with deeper leaves |
| `{"root":[1,2,null,3]}` | `3` | Left-skewed tree |
| `{"root":[1,null,2,null,3,null,4]}` | `4` | Right-skewed tree |
| `{"root":[0,-1,1,-2,null,null,2]}` | `3` | Negative and duplicate-permitted values |

### 6.2 Number of Islands (Medium)

Given an `m` by `n` grid of characters `'1'` (land) and `'0'` (water), return the number of islands. An island is a maximal group of land cells connected horizontally or vertically; diagonal contact alone does not connect cells. The grid is surrounded by water.

**Function:** `numIslands(grid: character[][]) -> integer`

**Example 1**

- **Input:** `{"grid":[["1","1","1","1","0"],["1","1","0","1","0"],["1","1","0","0","0"],["0","0","0","0","0"]]}`
- **Output:** `1`

**Example 2**

- **Input:** `{"grid":[["1","1","0","0","0"],["1","1","0","0","0"],["0","0","1","0","0"],["0","0","0","1","1"]]}`
- **Output:** `3`

**Constraints:** `1 <= m,n <= 300`; `grid[i].length == n`; every cell is `'0'` or `'1'`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"grid":[["1"]]}` | `1` | Minimum grid, land |
| `{"grid":[["0"]]}` | `0` | Minimum grid, water |
| `{"grid":[["1","1","1"]]}` | `1` | One-row connected land |
| `{"grid":[["1"],["0"],["1"]]}` | `2` | One-column separated land |
| `{"grid":[["1","0"],["0","1"]]}` | `2` | Diagonal contact is disconnected |
| `{"grid":[["1","1","0"],["0","1","0"],["1","0","1"]]}` | `3` | Several components |
| `{"grid":[["1","1","1"],["1","1","1"]]}` | `1` | Entire grid is land |
| `{"grid":[["0","0"],["0","0"]]}` | `0` | Entire grid is water |

### 6.3 Word Ladder II (Hard)

Given `beginWord`, `endWord`, and a dictionary `wordList`, return every shortest transformation sequence from `beginWord` to `endWord`. A transformation changes exactly one letter, and each transformed word must be in `wordList`. The begin word need not be in the list; the end word must be in it. If no sequence exists, return an empty list. For canonical output, sort words within each path as generated by the transformation sequence (without rearranging the path), then sort the list of paths lexicographically.

**Function:** `findLadders(beginWord: string, endWord: string, wordList: string[]) -> string[][]`

**Example 1**

- **Input:** `{"beginWord":"hit","endWord":"cog","wordList":["hot","dot","dog","lot","log","cog"]}`
- **Output:** `[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]`

**Example 2**

- **Input:** `{"beginWord":"hit","endWord":"cog","wordList":["hot","dot","dog","lot","log"]}`
- **Output:** `[]`

**Constraints:** `1 <= beginWord.length <= 5`; `endWord.length == beginWord.length`; `1 <= wordList.length <= 500`; all words have equal length and contain lowercase English letters; `beginWord != endWord`; all dictionary words are unique.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"beginWord":"a","endWord":"c","wordList":["c"]}` | `[["a","c"]]` | Direct one-step path |
| `{"beginWord":"a","endWord":"c","wordList":["b"]}` | `[]` | End word absent |
| `{"beginWord":"hit","endWord":"cog","wordList":["hot","dot","dog","lot","log","cog"]}` | `[["hit","hot","dot","dog","cog"],["hit","hot","lot","log","cog"]]` | Two shortest paths, lexical order |
| `{"beginWord":"hit","endWord":"cog","wordList":["hot","dot","dog","lot","log"]}` | `[]` | No valid end word |
| `{"beginWord":"red","endWord":"tax","wordList":["ted","tex","red","tax","tad","den","rex","pee"]}` | `[["red","rex","tex","tax"],["red","ted","tad","tax"],["red","ted","tex","tax"]]` | All shortest paths, lexical ordering |
| `{"beginWord":"same","endWord":"came","wordList":["came"]}` | `[["same","came"]]` | One-character difference |
| `{"beginWord":"a","endWord":"b","wordList":["b","c"]}` | `[["a","b"]]` | Irrelevant dictionary word |
| `{"beginWord":"ab","endWord":"cd","wordList":["cb","cd","ad"]}` | `[["ab","ad","cd"],["ab","cb","cd"]]` | Two shortest chains, lexical ordering |

## 7) Dynamic Programming

### 7.1 Climbing Stairs (Easy)

You are climbing a staircase with `n` steps. Each move climbs either one or two steps. Return the number of distinct ways to reach exactly the top. The order of moves matters.

**Function:** `climbStairs(n: integer) -> integer`

**Example 1**

- **Input:** `{"n":2}`
- **Output:** `2`

**Example 2**

- **Input:** `{"n":3}`
- **Output:** `3`

**Constraints:** `1 <= n <= 45`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"n":1}` | `1` | Minimum size |
| `{"n":2}` | `2` | Two single steps or one double |
| `{"n":3}` | `3` | Canonical small case |
| `{"n":4}` | `5` | Multiple move sequences |
| `{"n":5}` | `8` | Larger recurrence case |
| `{"n":10}` | `89` | Mid-range input |
| `{"n":20}` | `10946` | Larger input |
| `{"n":45}` | `1836311903` | Maximum constraint |

### 7.2 Coin Change (Medium)

Given an array of distinct positive coin denominations `coins` and a non-negative integer `amount`, return the fewest number of coins needed to make exactly `amount`. Each denomination may be used any number of times. Return `-1` if the amount cannot be formed; amount zero requires zero coins.

**Function:** `coinChange(coins: integer[], amount: integer) -> integer`

**Example 1**

- **Input:** `{"coins":[1,2,5],"amount":11}`
- **Output:** `3`

**Example 2**

- **Input:** `{"coins":[2],"amount":3}`
- **Output:** `-1`

**Constraints:** `1 <= coins.length <= 12`; `1 <= coins[i] <= 2^31-1`; coin values are distinct; `0 <= amount <= 10^4`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"coins":[1],"amount":0}` | `0` | Zero amount |
| `{"coins":[1],"amount":1}` | `1` | Single coin |
| `{"coins":[2],"amount":3}` | `-1` | Impossible amount |
| `{"coins":[1,2,5],"amount":11}` | `3` | Minimum uses mixed denominations |
| `{"coins":[2,5,10,1],"amount":27}` | `4` | Non-greedy choice: 10+10+5+2 |
| `{"coins":[186,419,83,408],"amount":6249}` | `20` | Larger values and amount |
| `{"coins":[3,7],"amount":14}` | `2` | Reuse one denomination |
| `{"coins":[4,6],"amount":7}` | `-1` | No exact combination |

### 7.3 Edit Distance (Hard)

Given strings `word1` and `word2`, return the minimum number of operations required to convert `word1` into `word2`. One operation inserts one character, deletes one character, or replaces one character. Characters are lowercase English letters.

**Function:** `minDistance(word1: string, word2: string) -> integer`

**Example 1**

- **Input:** `{"word1":"horse","word2":"ros"}`
- **Output:** `3`

**Example 2**

- **Input:** `{"word1":"intention","word2":"execution"}`
- **Output:** `5`

**Constraints:** `0 <= word1.length, word2.length <= 500`; each string contains lowercase English letters only.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"word1":"","word2":""}` | `0` | Both strings empty |
| `{"word1":"","word2":"abc"}` | `3` | Insert all characters |
| `{"word1":"abc","word2":""}` | `3` | Delete all characters |
| `{"word1":"a","word2":"a"}` | `0` | Identical single character |
| `{"word1":"a","word2":"b"}` | `1` | One replacement |
| `{"word1":"horse","word2":"ros"}` | `3` | Canonical mixed operations |
| `{"word1":"intention","word2":"execution"}` | `5` | Multiple optimal edits |
| `{"word1":"kitten","word2":"sitting"}` | `3` | Insertions and replacements |
## 8) Greedy

### 8.1 Assign Cookies (Easy)

There are `g.length` children and `s.length` cookies. Child `i` is satisfied by a cookie of size at least `g[i]`. Each child and cookie can be used at most once. Return the greatest number of children that can be satisfied.

**Function:** `findContentChildren(g: integer[], s: integer[]) -> integer`

**Example 1**

- **Input:** `{"g":[1,2,3],"s":[1,1]}`
- **Output:** `1`

**Example 2**

- **Input:** `{"g":[1,2],"s":[1,2,3]}`
- **Output:** `2`

**Constraints:** `1 <= g.length, s.length <= 30000`; `1 <= g[i], s[i] <= 10^9`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"g":[1],"s":[1]}` | `1` | Minimum inputs |
| `{"g":[2],"s":[1]}` | `0` | Cookie too small |
| `{"g":[1,2,3],"s":[1,1]}` | `1` | Fewer cookies than children |
| `{"g":[1,2],"s":[1,2,3]}` | `2` | Extra cookies |
| `{"g":[2,2,2],"s":[2,2]}` | `2` | Duplicate requirements |
| `{"g":[1,3,5],"s":[2,4,6]}` | `3` | Every child can be satisfied |
| `{"g":[5,6],"s":[1,2,3,4]}` | `0` | No child can be satisfied |
| `{"g":[1000000000],"s":[1000000000]}` | `1` | Maximum value |

### 8.2 Jump Game (Medium)

Given a non-empty array of non-negative integers `nums`, you start at index `0`. At index `i`, you may jump forward by any integer from `0` through `nums[i]`. Return whether you can reach the last index.

**Function:** `canJump(nums: integer[]) -> boolean`

**Example 1**

- **Input:** `{"nums":[2,3,1,1,4]}`
- **Output:** `true`

**Example 2**

- **Input:** `{"nums":[3,2,1,0,4]}`
- **Output:** `false`

**Constraints:** `1 <= nums.length <= 10000`; `0 <= nums[i] <= 10^5`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"nums":[0]}` | `true` | Already at destination |
| `{"nums":[1,0]}` | `true` | Exact final jump |
| `{"nums":[2,3,1,1,4]}` | `true` | Reachable with choices |
| `{"nums":[3,2,1,0,4]}` | `false` | Zero creates a barrier |
| `{"nums":[2,0,0]}` | `true` | Jump over zeros |
| `{"nums":[0,1]}` | `false` | Cannot leave first index |
| `{"nums":[1,1,1,1,1]}` | `true` | Repeated unit jumps |
| `{"nums":[5,0,0,0,0,0]}` | `true` | Jump directly to end |

### 8.3 Candy (Hard)

There are `n` children standing in a line. Each child must receive at least one candy. A child with a higher rating than an adjacent child must receive more candies than that neighbor. Return the minimum number of candies required.

**Function:** `candy(ratings: integer[]) -> integer`

**Example 1**

- **Input:** `{"ratings":[1,0,2]}`
- **Output:** `5`

**Example 2**

- **Input:** `{"ratings":[1,2,2]}`
- **Output:** `4`

**Constraints:** `1 <= ratings.length <= 20000`; `0 <= ratings[i] <= 20000`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"ratings":[1]}` | `1` | Single child |
| `{"ratings":[1,1]}` | `2` | Equal ratings |
| `{"ratings":[1,2,3]}` | `6` | Increasing ratings |
| `{"ratings":[3,2,1]}` | `6` | Decreasing ratings |
| `{"ratings":[1,0,2]}` | `5` | Local minimum |
| `{"ratings":[1,2,2]}` | `4` | Equal adjacent ratings |
| `{"ratings":[1,3,4,5,2]}` | `11` | Peak followed by a drop |
| `{"ratings":[5,5,5,5,5]}` | `5` | Flat sequence |

## 9) Heap / Priority Queue

### 9.1 Kth Largest Element in a Stream (Easy)

Design a stream object initialized with integer `k` and an initial integer array `nums`. For each `add(val)` operation, insert `val` and return the `k`th largest value among all values seen so far. The initial input and every operation guarantee that at least `k` values have been seen when a result is requested.

**Function:** `kthLargestStream(k: integer, nums: integer[], operations: integer[]) -> integer[]`

**Example 1**

- **Input:** `{"k":3,"nums":[4,5,8,2],"operations":[3,5,10,9,4]}`
- **Output:** `[4,5,5,8,8]`

**Example 2**

- **Input:** `{"k":1,"nums":[-2],"operations":[-3,0,2]}`
- **Output:** `[-2,0,2]`

**Constraints:** `1 <= k <= 10^4`; `0 <= nums.length <= 10^4`; `k <= nums.length + operations.length`; values are in `[-10^4,10^4]`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"k":1,"nums":[],"operations":[5]}` | `[5]` | Smallest valid stream |
| `{"k":1,"nums":[-2],"operations":[-3,0,2]}` | `[-2,0,2]` | Largest-so-far behavior |
| `{"k":2,"nums":[1,2],"operations":[3,4]}` | `[2,3]` | Increasing stream |
| `{"k":2,"nums":[5,5],"operations":[5,1]}` | `[5,5]` | Duplicate values |
| `{"k":3,"nums":[4,5,8,2],"operations":[3,5,10,9,4]}` | `[4,5,5,8,8]` | Standard stream |
| `{"k":2,"nums":[-5,-2],"operations":[-10,0]}` | `[-5,-2]` | Negative values |
| `{"k":3,"nums":[10,20,30],"operations":[5]}` | `[10]` | New value below kth |
| `{"k":2,"nums":[10000,-10000],"operations":[10000]}` | `[10000]` | Value boundaries |

### 9.2 Top K Frequent Elements (Medium)

Given an integer array `nums` and an integer `k`, return the `k` most frequent distinct values. Order the result by decreasing frequency, breaking ties by increasing numeric value.

**Function:** `topKFrequent(nums: integer[], k: integer) -> integer[]`

**Example 1**

- **Input:** `{"nums":[1,1,1,2,2,3],"k":2}`
- **Output:** `[1,2]`

**Example 2**

- **Input:** `{"nums":[4,4,1,1,2,2],"k":2}`
- **Output:** `[1,2]`

**Constraints:** `1 <= nums.length <= 10^5`; `-10^4 <= nums[i] <= 10^4`; `1 <= k <= number of distinct values`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[1],"k":1}` | `[1]` | Single value |
| `{"nums":[1,1,2],"k":1}` | `[1]` | One most frequent |
| `{"nums":[1,1,1,2,2,3],"k":2}` | `[1,2]` | Different frequencies |
| `{"nums":[4,4,1,1,2,2],"k":2}` | `[1,2]` | Frequency tie, numeric order |
| `{"nums":[-1,-1,2,2,3],"k":3}` | `[-1,2,3]` | Negative values and tie |
| `{"nums":[5,4,3,2,1],"k":5}` | `[1,2,3,4,5]` | All frequencies tied |
| `{"nums":[0,0,0,1,1,2,2,2],"k":2}` | `[0,2]` | Tied maximum frequency |
| `{"nums":[10000,10000,-10000],"k":1}` | `[10000]` | Value boundaries |

### 9.3 Find Median from Data Stream (Hard)

Design a data structure that accepts integers one at a time and reports the median of all values seen so far. For an odd number of values, the median is the middle value after sorting. For an even number, it is the arithmetic mean of the two middle values. Process operations in order; `addNum` produces no output and each `findMedian` produces a number.

**Function:** `medianStream(operations: string[], arguments: integer[][]) -> number[]`

**Example 1**

- **Input:** `{"operations":["addNum","addNum","findMedian","addNum","findMedian"],"arguments":[[1],[2],[],[3],[]]}`
- **Output:** `[1.5,2]`

**Example 2**

- **Input:** `{"operations":["addNum","findMedian","addNum","findMedian"],"arguments":[[-1],[],[-2],[]]}`
- **Output:** `[-1,-1.5]`

**Constraints:** `1 <= operations.length <= 5 * 10^4`; each `addNum` value is in `[-10^5,10^5]`; `findMedian` is called only after at least one insertion.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"operations":["addNum","findMedian"],"arguments":[[7],[]]}` | `[7]` | One value |
| `{"operations":["addNum","addNum","findMedian"],"arguments":[[1],[2],[]]}` | `[1.5]` | Even count |
| `{"operations":["addNum","addNum","addNum","findMedian"],"arguments":[[1],[2],[3],[]]}` | `[2]` | Odd count |
| `{"operations":["addNum","addNum","findMedian"],"arguments":[[-1],[-2],[]]}` | `[-1.5]` | Negative median |
| `{"operations":["addNum","addNum","addNum","addNum","findMedian"],"arguments":[[5],[5],[5],[5],[]]}` | `[5]` | Repeated values |
| `{"operations":["addNum","addNum","addNum","findMedian"],"arguments":[[100000],[-100000],[0],[]]}` | `[0]` | Wide values |
| `{"operations":["addNum","findMedian","addNum","findMedian","addNum","findMedian"],"arguments":[[2],[],[1],[],[3],[]]}` | `[2,1.5,2]` | Repeated queries as stream grows |
| `{"operations":["addNum","addNum","addNum","addNum","findMedian"],"arguments":[[1],[10],[2],[9],[]]}` | `[5.5]` | Median across unsorted arrivals |

## 10) Monotonic Stack

### 10.1 Next Greater Element I (Easy)

`nums1` is a subset of distinct values from distinct-value array `nums2`. For each value in `nums1`, find the first greater value to its right in `nums2`; return `-1` if none exists. Return answers in `nums1` order.

**Function:** `nextGreaterElement(nums1: integer[], nums2: integer[]) -> integer[]`

**Example 1**

- **Input:** `{"nums1":[4,1,2],"nums2":[1,3,4,2]}`
- **Output:** `[-1,3,-1]`

**Example 2**

- **Input:** `{"nums1":[2,4],"nums2":[1,2,3,4]}`
- **Output:** `[3,-1]`

**Constraints:** `1 <= nums1.length <= nums2.length <= 1000`; `-10^4 <= nums[i] <= 10^4`; values within each array are distinct; `nums1` is a subset of `nums2`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums1":[1],"nums2":[1,2]}` | `[2]` | Next item is greater |
| `{"nums1":[2],"nums2":[2,1]}` | `[-1]` | No greater value |
| `{"nums1":[4,1,2],"nums2":[1,3,4,2]}` | `[-1,3,-1]` | Mixed answers |
| `{"nums1":[2,4],"nums2":[1,2,3,4]}` | `[3,-1]` | Increasing sequence |
| `{"nums1":[3,1],"nums2":[3,2,1]}` | `[-1,-1]` | Decreasing sequence |
| `{"nums1":[-2,-1],"nums2":[-3,-2,-1]}` | `[-1,-1]` | Negative values |
| `{"nums1":[5,1,4],"nums2":[1,5,2,4,3]}` | `[-1,5,-1]` | Preserve query order |
| `{"nums1":[10],"nums2":[-10,0,10]}` | `[-1]` | Largest value at end |

### 10.2 Daily Temperatures (Medium)

Given daily temperatures, return an array where each position is the number of days until a strictly warmer temperature. If no future day is warmer, the answer for that day is `0`.

**Function:** `dailyTemperatures(temperatures: integer[]) -> integer[]`

**Example 1**

- **Input:** `{"temperatures":[73,74,75,71,69,72,76,73]}`
- **Output:** `[1,1,4,2,1,1,0,0]`

**Example 2**

- **Input:** `{"temperatures":[30,40,50,60]}`
- **Output:** `[1,1,1,0]`

**Constraints:** `1 <= temperatures.length <= 10^5`; `30 <= temperatures[i] <= 100`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"temperatures":[30]}` | `[0]` | Single day |
| `{"temperatures":[30,40]}` | `[1,0]` | Next day warmer |
| `{"temperatures":[40,30]}` | `[0,0]` | No later warmer day |
| `{"temperatures":[30,40,50,60]}` | `[1,1,1,0]` | Strictly increasing |
| `{"temperatures":[60,50,40,30]}` | `[0,0,0,0]` | Strictly decreasing |
| `{"temperatures":[73,74,75,71,69,72,76,73]}` | `[1,1,4,2,1,1,0,0]` | Warmer day several positions away |
| `{"temperatures":[50,50,51]}` | `[2,1,0]` | Equal is not warmer |
| `{"temperatures":[30,100,30,99]}` | `[1,0,1,0]` | Near-maximum temperatures |

### 10.3 Largest Rectangle in Histogram (Hard)

Given an array of non-negative bar heights, where each bar has width one, return the largest rectangular area that can be formed using contiguous bars.

**Function:** `largestRectangleArea(heights: integer[]) -> integer`

**Example 1**

- **Input:** `{"heights":[2,1,5,6,2,3]}`
- **Output:** `10`

**Example 2**

- **Input:** `{"heights":[2,4]}`
- **Output:** `4`

**Constraints:** `1 <= heights.length <= 10^5`; `0 <= heights[i] <= 10^4`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"heights":[0]}` | `0` | Minimum height |
| `{"heights":[5]}` | `5` | Single bar |
| `{"heights":[2,4]}` | `4` | Two bars |
| `{"heights":[2,1,5,6,2,3]}` | `10` | Standard mixed heights |
| `{"heights":[2,2,2,2]}` | `8` | Equal heights |
| `{"heights":[1,2,3,4]}` | `6` | Increasing heights |
| `{"heights":[4,3,2,1]}` | `6` | Decreasing heights |
| `{"heights":[5,0,5]}` | `5` | Zero separates bars |

## 11) Backtracking

### 11.1 Binary Watch (Easy)

A binary watch has four LEDs for hours (`0` through `11`) and six LEDs for minutes (`0` through `59`). Given `turnedOn`, return every valid time with exactly that many illuminated LEDs. Format times as `H:MM` and sort them by actual time (hour first, then minute).

**Function:** `readBinaryWatch(turnedOn: integer) -> string[]`

**Example 1**

- **Input:** `{"turnedOn":1}`
- **Output:** `["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]`

**Example 2**

- **Input:** `{"turnedOn":9}`
- **Output:** `[]`

**Constraints:** `0 <= turnedOn <= 10`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"turnedOn":0}` | `["0:00"]` | No LEDs |
| `{"turnedOn":1}` | `["0:01","0:02","0:04","0:08","0:16","0:32","1:00","2:00","4:00","8:00"]` | One LED, sorted output |
| `{"turnedOn":2}` | `["0:03","0:05","0:06","0:09","0:10","0:12","0:17","0:18","0:20","0:24","0:33","0:34","0:36","0:40","0:48","1:01","1:02","1:04","1:08","1:16","1:32","2:01","2:02","2:04","2:08","2:16","2:32","3:00","4:01","4:02","4:04","4:08","4:16","4:32","5:00","6:00","8:01","8:02","8:04","8:08","8:16","8:32","9:00","10:00"]` | Two LEDs, exact complete result |
| `{"turnedOn":8}` | `["7:31","7:47","7:55","7:59","11:31","11:47","11:55","11:59"]` | Maximum feasible LED count |
| `{"turnedOn":9}` | `[]` | Too many LEDs for a valid display |
| `{"turnedOn":10}` | `[]` | Upper constraint |

### 11.2 Combination Sum (Medium)

Given distinct positive integers `candidates` and a positive integer `target`, return all unique combinations whose values sum to `target`. A candidate may be used any number of times. Sort numbers within each combination and sort the list of combinations lexicographically.

**Function:** `combinationSum(candidates: integer[], target: integer) -> integer[][]`

**Example 1**

- **Input:** `{"candidates":[2,3,6,7],"target":7}`
- **Output:** `[[2,2,3],[7]]`

**Example 2**

- **Input:** `{"candidates":[2,3,5],"target":8}`
- **Output:** `[[2,2,2,2],[2,3,3],[3,5]]`

**Constraints:** `1 <= candidates.length <= 30`; candidates are distinct positive integers; `1 <= target <= 500`; the number of valid combinations is less than 150.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"candidates":[2],"target":1}` | `[]` | No combination |
| `{"candidates":[1],"target":1}` | `[[1]]` | Single candidate |
| `{"candidates":[2,3,6,7],"target":7}` | `[[2,2,3],[7]]` | Reuse and single-item solution |
| `{"candidates":[2,3,5],"target":8}` | `[[2,2,2,2],[2,3,3],[3,5]]` | Multiple combinations, canonical ordering |
| `{"candidates":[1,2],"target":4}` | `[[1,1,1,1],[1,1,2],[2,2]]` | Repeated values in combination |
| `{"candidates":[3,5],"target":10}` | `[[5,5]]` | Only one valid combination |
| `{"candidates":[4,6],"target":3}` | `[]` | Target below all candidates |
| `{"candidates":[1,2,3],"target":3}` | `[[1,1,1],[1,2],[3]]` | Distinct combinations in lexicographic order |

### 11.3 N-Queens (Hard)

Place `n` queens on an `n x n` chessboard so that no two queens share a row, column, or diagonal. Return all distinct solutions. Represent each board as `n` strings, using `Q` for a queen and `.` for an empty square. Sort solutions lexicographically by their row-string arrays.

**Function:** `solveNQueens(n: integer) -> string[][]`

**Example 1**

- **Input:** `{"n":1}`
- **Output:** `[["Q"]]`

**Example 2**

- **Input:** `{"n":4}`
- **Output:** `[["..Q.","Q...","...Q",".Q.."],[".Q..","...Q","Q...","..Q."]]`

**Constraints:** `1 <= n <= 9`.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"n":1}` | `[["Q"]]` | Smallest board |
| `{"n":2}` | `[]` | No solution |
| `{"n":3}` | `[]` | No solution |
| `{"n":4}` | `[["..Q.","Q...","...Q",".Q.."],[".Q..","...Q","Q...","..Q."]]` | Two solutions, canonical order |
| `{"n":5}` | `[["....Q","..Q..","Q....","...Q.",".Q..."],["....Q",".Q...","...Q.","Q....","..Q.."],["...Q.",".Q...","....Q","..Q..","Q...."],["...Q.","Q....","..Q..","....Q",".Q..."],["..Q..","....Q",".Q...","...Q.","Q...."],["..Q..","Q....","...Q.",".Q...","....Q"],[".Q...","....Q","..Q..","Q....","...Q."],[".Q...","...Q.","Q....","..Q..","....Q"],["Q....","...Q.",".Q...","....Q","..Q.."],["Q....","..Q..","....Q",".Q...","...Q."]]` | Ten solutions, canonical order |

## 12) Union-Find

### 12.1 Find if Path Exists in Graph (Easy)

There are `n` vertices labeled `0` through `n-1` and an undirected edge list. Return whether a path exists between `source` and `destination`. A vertex has a path to itself.

**Function:** `validPath(n: integer, edges: integer[][], source: integer, destination: integer) -> boolean`

**Example 1**

- **Input:** `{"n":3,"edges":[[0,1],[1,2],[2,0]],"source":0,"destination":2}`
- **Output:** `true`

**Example 2**

- **Input:** `{"n":6,"edges":[[0,1],[0,2],[3,5],[5,4],[4,3]],"source":0,"destination":5}`
- **Output:** `false`

**Constraints:** `1 <= n <= 2 * 10^5`; `0 <= edges.length <= 2 * 10^5`; edges contain distinct valid endpoints; no duplicate undirected edges.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"n":1,"edges":[],"source":0,"destination":0}` | `true` | Same vertex |
| `{"n":2,"edges":[],"source":0,"destination":1}` | `false` | No edges |
| `{"n":2,"edges":[[0,1]],"source":0,"destination":1}` | `true` | Direct edge |
| `{"n":4,"edges":[[0,1],[1,2],[2,3]],"source":0,"destination":3}` | `true` | Multi-edge path |
| `{"n":6,"edges":[[0,1],[0,2],[3,5],[5,4],[4,3]],"source":0,"destination":5}` | `false` | Separate components |
| `{"n":5,"edges":[[0,1],[1,2],[2,0],[3,4]],"source":2,"destination":0}` | `true` | Cycle in component |
| `{"n":5,"edges":[[0,1],[2,3]],"source":1,"destination":4}` | `false` | Isolated destination |
| `{"n":4,"edges":[[0,3],[1,2]],"source":0,"destination":3}` | `true` | Endpoint edge |

### 12.2 Number of Connected Components in an Undirected Graph (Medium)

Given `n` vertices labeled `0` through `n-1` and an undirected edge list, return the number of connected components.

**Function:** `countComponents(n: integer, edges: integer[][]) -> integer`

**Example 1**

- **Input:** `{"n":5,"edges":[[0,1],[1,2],[3,4]]}`
- **Output:** `2`

**Example 2**

- **Input:** `{"n":5,"edges":[[0,1],[1,2],[2,3],[3,4]]}`
- **Output:** `1`

**Constraints:** `1 <= n <= 2 * 10^5`; `0 <= edges.length <= min(2 * 10^5, n*(n-1)/2)`; edges contain distinct valid endpoints with no duplicates.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"n":1,"edges":[]}` | `1` | One isolated vertex |
| `{"n":4,"edges":[]}` | `4` | All isolated |
| `{"n":2,"edges":[[0,1]]}` | `1` | Connected pair |
| `{"n":5,"edges":[[0,1],[1,2],[3,4]]}` | `2` | Multiple components |
| `{"n":5,"edges":[[0,1],[1,2],[2,3],[3,4]]}` | `1` | One connected component |
| `{"n":6,"edges":[[0,1],[1,2],[2,0],[3,4]]}` | `3` | Cycle and isolated vertex |
| `{"n":7,"edges":[[0,6],[1,2],[2,3],[4,5]]}` | `3` | Components not contiguous by labels |
| `{"n":5,"edges":[[0,4],[1,4],[2,4],[3,4]]}` | `1` | Star graph |

### 12.3 Redundant Connection II (Hard)

A directed graph began as a rooted tree with nodes labeled `1` through `n`, where every node except the root has exactly one parent and all nodes are reachable from the root. One additional directed edge was added. Return the last edge in input order whose removal leaves a rooted tree. The answer is guaranteed to exist and be unique under this rule.

**Function:** `findRedundantDirectedConnection(edges: integer[][]) -> integer[2]`

**Example 1**

- **Input:** `{"edges":[[1,2],[1,3],[2,3]]}`
- **Output:** `[2,3]`

**Example 2**

- **Input:** `{"edges":[[1,2],[2,3],[3,4],[4,1],[1,5]]}`
- **Output:** `[4,1]`

**Constraints:** `3 <= edges.length <= 1000`; `edges[i].length == 2`; labels are in `1..n`, where `n = edges.length`; edges are distinct.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"edges":[[1,2],[1,3],[2,3]]}` | `[2,3]` | A node has two parents |
| `{"edges":[[1,2],[2,3],[3,4],[4,1],[1,5]]}` | `[4,1]` | Directed cycle |
| `{"edges":[[2,1],[3,1],[4,2],[1,4]]}` | `[2,1]` | Two parents and cycle |
| `{"edges":[[1,2],[2,3],[3,1]]}` | `[3,1]` | Cycle closes on last edge |
| `{"edges":[[1,2],[2,3],[3,4],[4,5],[5,3]]}` | `[5,3]` | Cycle among non-root nodes |
| `{"edges":[[4,1],[1,2],[2,3],[3,1]]}` | `[3,1]` | Root edge plus cycle |

## 13) Topological Sort

### 13.1 Course Schedule (Easy)

There are `numCourses` courses labeled `0` through `numCourses-1`. Each prerequisite pair `[course, prerequisite]` means the prerequisite must be completed first. Return `true` if it is possible to complete all courses.

**Function:** `canFinish(numCourses: integer, prerequisites: integer[][]) -> boolean`

**Example 1**

- **Input:** `{"numCourses":2,"prerequisites":[[1,0]]}`
- **Output:** `true`

**Example 2**

- **Input:** `{"numCourses":2,"prerequisites":[[1,0],[0,1]]}`
- **Output:** `false`

**Constraints:** `1 <= numCourses <= 2000`; `0 <= prerequisites.length <= 5000`; no duplicate prerequisite pairs.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"numCourses":1,"prerequisites":[]}` | `true` | Single course |
| `{"numCourses":2,"prerequisites":[]}` | `true` | No dependencies |
| `{"numCourses":2,"prerequisites":[[1,0]]}` | `true` | One dependency |
| `{"numCourses":2,"prerequisites":[[1,0],[0,1]]}` | `false` | Two-course cycle |
| `{"numCourses":4,"prerequisites":[[1,0],[2,1],[3,2]]}` | `true` | Dependency chain |
| `{"numCourses":4,"prerequisites":[[1,0],[2,1],[0,2]]}` | `false` | Cycle with extra course |
| `{"numCourses":4,"prerequisites":[[1,0],[2,0],[3,1],[3,2]]}` | `true` | Dependency branches |
| `{"numCourses":3,"prerequisites":[[0,1],[1,2],[2,0]]}` | `false` | Three-course cycle |

### 13.2 Course Schedule II (Medium)

Given `numCourses` and prerequisite pairs `[course, prerequisite]`, return an order that completes every course while respecting all prerequisites. If several orders are valid, return the lexicographically smallest sequence. Return an empty array if completing all courses is impossible.

**Function:** `findOrder(numCourses: integer, prerequisites: integer[][]) -> integer[]`

**Example 1**

- **Input:** `{"numCourses":2,"prerequisites":[[1,0]]}`
- **Output:** `[0,1]`

**Example 2**

- **Input:** `{"numCourses":4,"prerequisites":[[1,0],[2,0],[3,1],[3,2]]}`
- **Output:** `[0,1,2,3]`

**Constraints:** `1 <= numCourses <= 2000`; `0 <= prerequisites.length <= 5000`; no duplicate prerequisite pairs.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"numCourses":1,"prerequisites":[]}` | `[0]` | One course |
| `{"numCourses":3,"prerequisites":[]}` | `[0,1,2]` | Lexicographic order with no edges |
| `{"numCourses":2,"prerequisites":[[1,0]]}` | `[0,1]` | Single dependency |
| `{"numCourses":2,"prerequisites":[[1,0],[0,1]]}` | `[]` | Cycle |
| `{"numCourses":4,"prerequisites":[[1,0],[2,0],[3,1],[3,2]]}` | `[0,1,2,3]` | Branches with unique smallest order |
| `{"numCourses":4,"prerequisites":[[2,0],[3,1]]}` | `[0,1,2,3]` | Multiple available choices |
| `{"numCourses":5,"prerequisites":[[1,0],[2,1],[3,2],[4,3]]}` | `[0,1,2,3,4]` | Long chain |
| `{"numCourses":3,"prerequisites":[[0,1],[1,2],[2,0]]}` | `[]` | Cycle across all vertices |

### 13.3 Alien Dictionary (Hard)

Words are sorted according to an unknown alphabet. Each word contains lowercase English letters. Return a valid ordering of the distinct letters that is consistent with the given word order. If multiple orders are valid, return the lexicographically smallest. Return the empty string if no valid ordering exists, including when a longer word precedes its own prefix.

**Function:** `alienOrder(words: string[]) -> string`

**Example 1**

- **Input:** `{"words":["wrt","wrf","er","ett","rftt"]}`
- **Output:** `"wertf"`

**Example 2**

- **Input:** `{"words":["z","x"]}`
- **Output:** `"zx"`

**Constraints:** `1 <= words.length <= 100`; `1 <= words[i].length <= 100`; words contain lowercase English letters.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"words":["z"]}` | `"z"` | One distinct character |
| `{"words":["z","x"]}` | `"zx"` | One ordering constraint |
| `{"words":["wrt","wrf","er","ett","rftt"]}` | `"wertf"` | Multiple constraints |
| `{"words":["abc","ab"]}` | `""` | Invalid prefix ordering |
| `{"words":["z","x","z"]}` | `""` | Cycle |
| `{"words":["abc","abc"]}` | `"abc"` | Identical words |
| `{"words":["za","zb","ca","cb"]}` | `"abzc"` | Independent character constraints, lexicographic result |
| `{"words":["a","b","ca"]}` | "abc" | Unconstrained character included |

## 14) Bit Manipulation

### 14.1 Single Number (Easy)

Every integer in `nums` appears exactly twice except for one integer, which appears once. Return that single integer.

**Function:** `singleNumber(nums: integer[]) -> integer`

**Example 1**

- **Input:** `{"nums":[2,2,1]}`
- **Output:** `1`

**Example 2**

- **Input:** `{"nums":[4,1,2,1,2]}`
- **Output:** `4`

**Constraints:** `1 <= nums.length <= 3 * 10^4`; length is odd; `-3 * 10^4 <= nums[i] <= 3 * 10^4`; exactly one value appears once and all others appear twice.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"nums":[7]}` | `7` | Single element |
| `{"nums":[2,2,1]}` | `1` | Single number at end |
| `{"nums":[4,1,2,1,2]}` | `4` | Single number at start |
| `{"nums":[-1,3,3]}` | `-1` | Negative answer |
| `{"nums":[0,1,0]}` | `1` | Zero duplicate |
| `{"nums":[-30000,5,5]}` | `-30000` | Minimum allowed value |
| `{"nums":[100,100,-30000]}` | `-30000` | Mixed values |
| `{"nums":[9,8,7,8,7]}` | `9` | Pair order is arbitrary |

### 14.2 Single Number III (Medium)

Every integer in `nums` appears exactly twice except for two distinct integers, which appear once each. Return those two single values in increasing numeric order.

**Function:** `singleNumberIII(nums: integer[]) -> integer[2]`

**Example 1**

- **Input:** `{"nums":[1,2,1,3,2,5]}`
- **Output:** `[3,5]`

**Example 2**

- **Input:** `{"nums":[-1,0]}`
- **Output:** `[-1,0]`

**Constraints:** `2 <= nums.length <= 3 * 10^4`; length is even; `-2^31 <= nums[i] < 2^31`; exactly two distinct values occur once and all others occur twice.

| Input | Expected Output | Coverage note |
|---|---|---|
| `{"nums":[-1,0]}` | `[-1,0]` | Minimum length |
| `{"nums":[1,2,1,3,2,5]}` | `[3,5]` | Standard case |
| `{"nums":[0,1]}` | `[0,1]` | Includes zero |
| `{"nums":[-3,-3,-1,2,2,5]}` | `[-1,5]` | Negative and positive singles |
| `{"nums":[7,7,8,9]}` | `[8,9]` | Singles at end |
| `{"nums":[9,8,9,7]}` | `[7,8]` | Canonical sorted output |
| `{"nums":[100000,1,1,-100000]}` | `[-100000,100000]` | Large magnitudes |
| `{"nums":[5,4,3,4,3,2]}` | `[2,5]` | Interleaved pairs |

### 14.3 Maximum XOR of Two Numbers in an Array (Hard)

Given a non-empty array of non-negative integers, choose two different indices and return the maximum bitwise XOR of their values.

**Function:** `findMaximumXOR(nums: integer[]) -> integer`

**Example 1**

- **Input:** `{"nums":[3,10,5,25,2,8]}`
- **Output:** `28`

**Example 2**

- **Input:** `{"nums":[0]}`
- **Output:** `0`

**Constraints:** `1 <= nums.length <= 2 * 10^5`; `0 <= nums[i] <= 2^31 - 1`. If fewer than two values are present, return `0`.

| Input | Expected Output | Coverage note |
|---|---:|---|
| `{"nums":[0]}` | `0` | Single input |
| `{"nums":[0,0]}` | `0` | Equal values |
| `{"nums":[1,2]}` | `3` | Minimum pair |
| `{"nums":[3,10,5,25,2,8]}` | `28` | Standard maximum |
| `{"nums":[8,1,2,12]}` | `14` | Pair crosses high bits |
| `{"nums":[5,5,5,5]}` | `0` | All values equal |
| `{"nums":[0,1,2,3,4]}` | `7` | Small consecutive values |
| `{"nums":[2147483647,0]}` | `2147483647` | Maximum allowed value |
