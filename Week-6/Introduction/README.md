# Dynamic Programming

## What is dynamic programming?
Dynamic Programming is a commonly used algorithmic technique used to optimize recursive solutions when same subproblems are called again.

* The core idea behind DP is to store solutions to subproblems so that each is solved only once.
* To solve DP problems, we first write a recursive solution in a way that there are overlapping subproblems in the recursion tree (the recursive function is called with the same parameters multiple times)
* To make sure that a recursive value is computed only once (to improve time taken by algorithm), we store results of the recursive calls.
* There are two ways to store the results, one is top down (or memoization) and other is bottom up (or tabulation).

## Why Dynamic Programming Matters

Dynamic Programming is not just another algorithm — it's a **critical mindset shift** in how we approach problems that seem exponential at first glance. Many problems in recursion are elegant in their logic but **explode in time complexity** due to repeated computations of the same subproblems.

Without optimization, these recursive calls form an exponential tree of repeated work.

Dynamic Programming teaches us to **recognize those repetitions**, **cache results**, and **turn exponential brute-force into polynomial efficiency**.


### From Recursion to Dynamic Programming

Most DP problems begin as simple recursion:

- Can you divide the problem into smaller parts?
- Do some of those smaller parts repeat?
- Can the result of one subproblem be reused in others?

When the answer is *yes* to these, **DP is the natural next step.**

We start with:
1. **Naive recursive solution** – clean but inefficient.
2. Then we add **memoization (top-down DP)** – store what we've already computed.
3. Finally, we transition to **tabulation (bottom-up DP)** – iteratively build up answers from base cases, often with space optimizations.

This transition is essential for writing code that’s not only correct but **efficient and scalable**.

Mastering DP helps you unlock solutions to a vast class of problems in:
- Optimization
- Counting paths or ways
- Partitioning and matching
- Decision-making under constraints

Whether it's solving real-world logistics problems or acing coding interviews, **DP is everywhere**.

This week, our goal is not to cover everything — but to build strong intuition using **4 foundational problems**, each showcasing a different core pattern. Once you're comfortable with these, you'll be ready to take on harder problems across trees, graphs, and strings.


## When to use DP?

Dynamic programming is used for solving problems that consists of the following characteristics:
1. **Optimal Substructure**
2. **Overlapping Subproblems**

### Optimal Substructure
This property means that we use the optimal results of subproblems to achieve the optimal result of the bigger problem.

### Overlapping Subproblems
The same subproblems are solved repeatedly in different parts of the problem refer to Overlapping Subproblems Property in Dynamic Programming.


## Approaches
Dynamic programming can be achieved using two approaches:
1. **Top-Down Approach**
2. **Bottom-Up Approach**

### Top-Down Approach
In the top-down approach, also known as memoization, we keep the solution recursive and add a memoization table to avoid repeated calls of same subproblems.

- Before making any recursive call, we first check if the memoization table already has solution for it.
- After the recursive call is over, we store the solution in the memoization table.

### Bottom-Up Approach
In the bottom-up approach, also known as tabulation, we start with the smallest subproblems and gradually build up to the final solution.

- We write an iterative solution (avoid recursion overhead) and build the solution in bottom-up manner.
- We use a dp table where we first fill the solution for base cases and then fill the remaining entries of the table using recursive formula.
- We only use recursive formula on table entries and do not make recursive calls.


## Common Dynamic Programming (DP) Algorithms

### 1. Sequence DP
Problems involving strings, arrays, or ordered elements.

- Fibonacci Sequence
- Climbing Stairs
- House Robber I/II
- Longest Increasing Subsequence (LIS)
- Longest Common Subsequence (LCS)
- Edit Distance (Levenshtein)
- Palindrome Partitioning
- Longest Palindromic Substring/Subsequence
- Maximum Subarray (Kadane’s Algorithm)

### 2. Knapsack Variants
Involving choices over weight/capacity constraints.

- 0/1 Knapsack
- Unbounded Knapsack
- Bounded Knapsack
- Subset Sum
- Partition Equal Subset Sum
- Target Sum
- Coin Change (min ways / total ways)

### 3. Grid DP / Path DP
2D DP problems involving paths, usually in matrices.

- Unique Paths
- Minimum Path Sum
- Maximum Path Sum
- Cherry Pickup
- Gold Mine Problem
- Robot with Obstacles
- Dungeon Game
- Probability of Reaching a Cell

### 4. Digit DP
Counting numbers with digit constraints.

- Count numbers with certain digits
- Sum of digits in a range
- Count numbers with k non-zero digits
- Avoid numbers with forbidden digits

### 5. Bitmask DP
Used when you want to track subsets or states using bits.

- Travelling Salesman Problem (TSP)
- Count ways to assign tasks
- Minimum Cost to Cover All Elements
- Hamiltonian Path

### 6. DP on Trees
Tree-structured DP where states are passed parent-child.

- Diameter of Tree
- Max Weight Independent Set in Tree
- Subtree DP
- Rerooting DP

### 7. DP on Graphs
General graphs with topological sorting or memoized DFS.

- Longest Path in DAG
- Number of Paths in DAG
- Minimum Jumps to Reach End
- Bellman-Ford Algorithm

### 8. Interval DP
Used when solving problems by splitting intervals.

- Matrix Chain Multiplication
- Burst Balloons
- Optimal BST
- Palindrome Partitioning
- Min Cost to Merge Stones

### 9. String Matching DP
Involving pattern matching or transformations.

- Regex Matching
- Wildcard Matching
- Minimum Insertions/Deletions to Convert Strings
- Word Break
- Interleaving Strings

### 10. Memoization Patterns

- Recursive Fibonacci
- Game Theory (Grundy Numbers)
- Minimax with DP (Nim Game, Stone Game)

### 11. State Compression

- Subset DP
- DP with rolling arrays

### 12. Advanced/Hybrid

- Slope Trick
- Li Chao Tree
- Convex Hull Trick
- Monotonic Queue Optimization
- Divide & Conquer Optimization
- Knuth Optimization
- Meet in the Middle + DP

### 13. Meta Patterns

- Top-down (Memoization)
- Bottom-up (Tabulation)
- Greedy + DP hybrids
- Bitmask + DP + Graph
- Tree DP + Rerooting


This week we shall only look at a few of these algorithms.
- [Climbing Stairs](../Climbing%20Stairs)
- [0/1 Knapsack](../0%5C1%20Knapsack)
- [Longest Common Subsequence (LCS)](../Longest%20Common%20Subsequence)
- [Matrix Chain Multiplication](../Matrix%20Chain%20Multiplication)

These four algorithms—Climbing Stairs, 0/1 Knapsack, Longest Common Subsequence (LCS), and Matrix Chain Multiplication—have been carefully chosen to cover the most fundamental dynamic programming patterns. Each represents a different class of DP problems: from simple recurrence and state transition (Climbing Stairs), to decision-based optimization (Knapsack), to two-dimensional comparisons (LCS), and finally, interval-based partitioning (Matrix Chain Multiplication). Mastering these equips you with the intuition and techniques needed to tackle the vast majority of DP problems you'll encounter in interviews, contests, or real-world optimization tasks.
