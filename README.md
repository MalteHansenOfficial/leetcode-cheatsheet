# 🚀 LeetCode Cheat Sheet

This is the reference I wish I had when I started grinding. It's built around the patterns that actually show up at Google, Meta, Amazon, Apple, and Microsoft — not just random problems, but the underlying logic interviewers are testing when they ask them.

One honest note upfront: this isn't a Python tutorial. If you're shaky on the basics of the language, sort that out first. This sheet assumes you can write a for loop and focuses entirely on the _patterns_ behind interview problems.

Feedback is welcome. Parts of the script are AI-generated (under supervision).

---

## 📚 Table of Contents

- [⏱️ Complexity Quick Reference](#%EF%B8%8F-complexity-quick-reference)
- [🧩 The Core Patterns](#-the-core-patterns)
- [🔧 Essential Data Structures](#-essential-data-structures)
- [🍳 Problem-Solving Recipes](#-problem-solving-recipes)
- [🐍 Python Snippets Worth Memorizing](#-python-snippets-worth-memorizing)
- [🗺️ What to Study and In What Order](#%EF%B8%8F-what-to-study-and-in-what-order)

---

## ⏱️ Complexity Quick Reference

| Complexity   | Name         | Typical Example           |
| ------------ | ------------ | ------------------------- |
| `O(1)`       | Constant     | HashMap lookup            |
| `O(log n)`   | Logarithmic  | Binary Search             |
| `O(n)`       | Linear       | Single pass through array |
| `O(n log n)` | Linearithmic | Merge Sort                |
| `O(n²)`      | Quadratic    | Nested loops              |
| `O(2ⁿ)`      | Exponential  | Backtracking over subsets |

⚠️ One rule that will save you mid-interview: **always nail the time complexity before touching space complexity.** Interviewers at FAANG companies care far more about whether you can reason about time. Space is almost always a follow-up.

---

## 🧩 The Core Patterns

Most FAANG problems aren't novel. They're one of ~15 patterns wearing a costume. Once you recognize the costume, the solution structure is largely predetermined.

---

### 🪟 Sliding Window

**The tell:** "Find a subarray or substring that satisfies some condition." If the problem involves a contiguous range and some constraint, sliding window is almost certainly the move.

- Fixed window size → advance both pointers in lockstep
- Variable window → stretch the right pointer until you break the constraint, then shrink from the left

**Problems to know cold:** Longest Substring Without Repeating Characters, Minimum Window Substring, Maximum Sum Subarray of Size K

---

### 👯 Two Pointers

**The tell:** Sorted array, finding pairs, or anything involving palindromes. The beauty here is you eliminate an entire nested loop by moving two pointers intelligently instead of checking every combination.

Start at opposite ends and move toward each other based on the comparison result. Sounds simple because it is — the hard part is recognizing when to apply it.

**Problems to know cold:** Two Sum II, 3Sum, Container With Most Water, Trapping Rain Water

---

### 🔍 Binary Search

**The tell:** Sorted input, OR a problem asking "what's the minimum/maximum X such that condition Y holds." That second case trips a lot of people — binary search isn't just for finding a value in a sorted array.

```python
lo, hi = 0, len(nums) - 1
while lo <= hi:
    mid = (lo + hi) // 2
    if nums[mid] == target:
        return mid
    elif nums[mid] < target:
        lo = mid + 1
    else:
        hi = mid - 1
```

Burn this template into memory. The off-by-one errors in binary search have ended more interviews than any other bug.

**Problems to know cold:** Search in Rotated Sorted Array, Find Minimum in Rotated Array, Koko Eating Bananas, Median of Two Sorted Arrays

---

### 🗃️ HashMap / HashSet

**The tell:** Frequency counting, deduplication, or any time you need O(1) lookup. This pattern underlies a huge portion of easy and medium problems.

The key insight is trading space for time — you store something you've already seen so you can answer "have I seen X before?" in constant time.

**Problems to know cold:** Two Sum, Group Anagrams, Top K Frequent Elements, Longest Consecutive Sequence

---

### 📚 Stack

**The tell:** Matching/nesting problems (brackets, HTML tags), or anything asking about the "next greater/smaller element." The stack lets you remember things in reverse order, which is exactly what those problems need.

**Problems to know cold:** Valid Parentheses, Daily Temperatures, Min Stack, Largest Rectangle in Histogram

---

### 🌲 Trees — DFS and BFS

**The tell:** Any tree problem, full stop. You'll use DFS for depth-related problems and path problems, BFS for level-by-level processing.

```python
# DFS — recursive, clean, and readable
def dfs(node):
    if not node:
        return
    dfs(node.left)
    dfs(node.right)

# BFS — use a deque, not a list (popleft is O(1) on deque, O(n) on list)
from collections import deque
queue = deque([root])
while queue:
    node = queue.popleft()
    if node.left: queue.append(node.left)
    if node.right: queue.append(node.right)
```

Trees come up constantly at FAANG. If you're weak here, prioritize it.

**Problems to know cold:** Invert Binary Tree, Max Depth, Level Order Traversal, Lowest Common Ancestor, Serialize and Deserialize Binary Tree

---

### 🕸️ Graphs — DFS and BFS

**The tell:** Connected components, shortest paths, cycle detection, or any problem where nodes have relationships. Graphs are just generalized trees — once you're comfortable with tree DFS/BFS, graphs are the same idea with a `visited` set added.

```python
visited = set()

def dfs(node):
    if node in visited:
        return
    visited.add(node)
    for neighbor in graph[node]:
        dfs(neighbor)
```

Build the `visited` set habit early. Forgetting it causes infinite loops and costs you the problem.

**Problems to know cold:** Number of Islands, Clone Graph, Course Schedule (I and II), Pacific Atlantic Water Flow, Word Ladder

---

### 🧠 Dynamic Programming

**The tell:** Overlapping subproblems. When a recursive solution repeatedly solves the same subproblem, DP caches those results. Ask yourself: _"Is the answer to this problem built from answers to smaller versions of the same problem?"_

- **1D DP:** `dp[i]` depends on one or two earlier values
- **2D DP:** `dp[i][j]` for problems involving two strings or a grid

Start by writing the recursive solution first. Get it correct. Then add memoization. Then convert to tabulation if needed. Don't start with the table — you'll lose track of what you're even computing.

**Problems to know cold:** Climbing Stairs, House Robber, Coin Change, Longest Common Subsequence, Edit Distance, 0/1 Knapsack

---

### 🔙 Backtracking

**The tell:** "Find ALL solutions," "generate all combinations/permutations/subsets." If the problem wants every valid arrangement rather than just one answer, backtracking is the pattern.

```python
def backtrack(start, current):
    results.append(current[:])  # snapshot — don't append current directly
    for i in range(start, len(nums)):
        current.append(nums[i])
        backtrack(i + 1, current)
        current.pop()  # undo the choice
```

The `current.pop()` at the end is the whole point — you undo your last choice so you can try the next one. If you forget it, you corrupt the state for future iterations.

**Problems to know cold:** Subsets, Permutations, Combination Sum, Letter Combinations of a Phone Number, Word Search

---

### ⛰️ Heap / Priority Queue

**The tell:** "Top K," "K closest," "Kth largest," or anything streaming where you need to efficiently track extremes.

```python
import heapq

min_heap = []
heapq.heappush(min_heap, val)
heapq.heappop(min_heap)   # pops the smallest

# Python only has min heaps — for max heap, push negative values
heapq.heappush(max_heap, -val)
max_val = -heapq.heappop(max_heap)
```

**Problems to know cold:** Kth Largest Element in a Stream, Top K Frequent Elements, Merge K Sorted Lists, Task Scheduler, Find Median from Data Stream

---

## 🔧 Essential Data Structures

| Structure    | Python                   | When to Reach for It                                |
| ------------ | ------------------------ | --------------------------------------------------- |
| Array        | `list`                   | Index access, sliding window, in-place manipulation |
| HashMap      | `dict` / `defaultdict`   | Frequency counting, O(1) lookup                     |
| HashSet      | `set`                    | Deduplication, membership checks                    |
| Stack        | `list` with `append/pop` | LIFO order, bracket matching, monotonic problems    |
| Queue        | `deque`                  | BFS, FIFO, sliding window maximum                   |
| Min/Max Heap | `heapq`                  | Top K, greedy scheduling, streaming                 |
| Prefix Sum   | `list`                   | Range sum queries without recomputing               |

---

## 🍳 Problem-Solving Recipes

### ➕ Prefix Sum

Build `prefix[i] = prefix[i-1] + nums[i]` once, then answer range sum queries in O(1). The classic setup for problems asking "how many subarrays sum to K."

### 📉 Monotonic Stack

For "next greater element" or "next smaller element" problems: maintain a stack where elements are always in increasing (or decreasing) order. When a new element breaks that property, you've found your answer for the elements you pop.

### 🔗 Interval Merging

Sort intervals by start time first. Then walk through and greedily extend the current interval if it overlaps the next one, or start a new interval if it doesn't.

### 📋 Topological Sort (BFS / Kahn's Algorithm)

For dependency problems — prerequisites, build orders, task scheduling. Track in-degrees for each node. Enqueue nodes with in-degree 0. Process them, decrement neighbors' in-degrees, enqueue any that hit 0. If you process all nodes, no cycle exists.

### 🤝 Union-Find (Disjoint Set Union)

For connected components and cycle detection in undirected graphs. Faster to implement than DFS for certain problems, and the code is reusable across almost all problems that use it.

```python
parent = list(range(n))

def find(x):
    if parent[x] != x:
        parent[x] = find(parent[x])  # path compression
    return parent[x]

def union(x, y):
    parent[find(x)] = find(y)
```

---

## 🐍 Python Snippets Worth Memorizing

```python
from collections import defaultdict, Counter, deque
import heapq

# Count frequencies in one line
freq = Counter(nums)

# Graph as adjacency list
graph = defaultdict(list)

# Sort by a specific field
pairs.sort(key=lambda x: x[1])

# Infinity (useful for DP initialization)
float('inf'), float('-inf')

# Group anagrams — sort each word to get a stable key
key = tuple(sorted(word))

# Initialize DP arrays
dp = [0] * (n + 1)
dp = [[0] * cols for _ in range(rows)]

# Heap with tuples — Python compares element by element
heapq.heappush(heap, (priority, value))
```

---

## 🗺️ What to Study and In What Order

This ordering is deliberate. Each topic builds on the previous one, and earlier topics have the highest density of interview questions.

**🗂️ Arrays & Hashing** — Everything else builds on this. Don't skip or rush it.

**🪟 Two Pointers & Sliding Window** — These two patterns together cover a massive slice of real interview questions. Get comfortable with both before moving on.

**📚 Stack** — High return on investment. The problems are learnable in a few days and stack questions appear constantly.

**🔍 Binary Search** — Master the template. The off-by-one errors are the only hard part.

**🔗 Linked Lists** — Pointer manipulation. Slow/fast pointer technique is the main thing to internalize here.

**🌲 Trees** — DFS and BFS in tree form. Recursive thinking becomes essential here and carries forward into graphs and DP.

**🕸️ Graphs** — The natural extension of trees. Islands, paths, cycle detection, topological sort.

**⛰️ Heap / Priority Queue** — K-th element problems are evergreen at FAANG. Know how to use a heap without thinking about it.

**🔙 Backtracking** — Permutations, combinations, constraint satisfaction. Takes time to internalize the "undo" pattern.

**🧠 1D Dynamic Programming** — Start simple. Climbing Stairs → House Robber → Coin Change. Feel the pattern before going 2D.

**🧩 2D Dynamic Programming** — String and grid problems. Edit Distance and LCS are the canonical examples.

**📅 Greedy & Intervals** — Scheduling problems, meeting rooms, jump game. Greedy is often the optimal solution when you can prove a local choice is globally optimal.

**⚡ Bit Manipulation** — Niche but quick to learn. Comes up occasionally and the tricks are memorable.

**📐 Math & Geometry** — Least common, but good to have in your back pocket.

---

The thing that separates people who do well in FAANG interviews from people who don't usually isn't how many problems they've solved — it's whether they've internalized _why_ a pattern applies. When you see a problem, you should be asking "what does this problem need?" not "which problem does this look like?"

Two hundred problems done shallowly will lose to fifty done deeply every time.
