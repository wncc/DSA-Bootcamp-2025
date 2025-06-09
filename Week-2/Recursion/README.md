
<!-----



Conversion time: 1.204 seconds.


Using this Markdown file:

1. Paste this output into your source file.
2. See the notes and action items below regarding this conversion run.
3. Check the rendered output (headings, lists, code blocks, tables) for proper
   formatting and use a linkchecker before you publish this page.

Conversion notes:

* Docs to Markdown version 1.0β44
* Wed Jun 04 2025 02:37:32 GMT-0700 (PDT)
* Source doc: Recursion
* Tables are currently converted to HTML tables.

WARNING:
You have 9 H1 headings. You may want to use the "H1 -> H2" option to demote all headings by one level.

----->
<!--

<p style="color: red; font-weight: bold">>>>>>  gd2md-html alert:  ERRORs: 0; WARNINGs: 1; ALERTS: 0.</p>
<ul style="color: red; font-weight: bold"><li>See top comment block for details on ERRORs and WARNINGs. <li>In the converted Markdown or HTML, search for inline alerts that start with >>>>>  gd2md-html alert:  for specific instances that need correction.</ul>

<p style="color: red; font-weight: bold">Links to alert messages:</p>
<p style="color: red; font-weight: bold">>>>>> PLEASE check and correct alert issues and delete this message and the inline alerts.<hr></p>
-->


# Recursion


# Introduction

The process in which a function calls itself directly or indirectly is called recursion and the corresponding function is called a recursive function.



* A recursive algorithm takes one step toward solution and then recursively calls itself to further move. The algorithm stops once we reach the solution.
* Since the called function may further call itself, this process might continue forever. So it is essential to provide a base case to terminate this recursion process.

---

# Comparison of Recursive and Iterative Approaches


<table>
  <tr>
   <td>Approach
   </td>
   <td>Complexity 
   </td>
   <td>Memory Usage
   </td>
  </tr>
  <tr>
   <td>Iterative
   </td>
   <td>O(n)
   </td>
   <td>O(1)
   </td>
  </tr>
  <tr>
   <td>Recursive
   </td>
   <td>O(n)
   </td>
   <td>O(n)
   </td>
  </tr>
</table>

---

# Need of Recursion?



* Recursion helps in logic building. Recursive thinking helps in solving complex problems by breaking them into smaller subproblems.
* Recursive solutions work as a basis for Dynamic Programming and Divide and Conquer algorithms.
* Certain problems can be solved quite easily using recursion like Towers of Hanoi (TOH), Inorder/Preorder/Postorder Tree Traversals, DFS of Graph, etc.

---

# Base condition

A recursive program stops at a base condition. There can be more than one base condition in a recursion. If the base case is not reached or not defined, then the stack overflow problem may arise. 

---

# Direct and indirect recursion

A function is called direct recursive if it calls itself directly during its execution. In other words, the function makes a recursive call to itself within its own body.

An indirect recursive function is one that calls another function, and that other function, in turn, calls the original function either directly or through other functions. This creates a chain of recursive calls involving multiple functions, as opposed to direct recursion, where a function calls itself.

---

# Example

Here we will better understand recursion by finding the Fibonacci series.


## C++


```
#include <bits/stdc++.h>
using namespace std;

// Function for fibonacci

int fib(int n)
{
    // Stop condition
    if (n == 0)
        return 0;

    // Stop condition
    if (n == 1 || n == 2)
        return 1;

    // Recursion function
    else
        return (fib(n - 1) + fib(n - 2));
}

// Driver Code
int main()
{
    // Initialize variable n.
    int n = 5;
    cout<<"Fibonacci series of 5 numbers is: ";

    // for loop to print the fibonacci series.
    for (int i = 0; i < n; i++) 
    {
        cout<<fib(i)<<" ";
    }
    return 0;
}
```



## JavaScript


```
// Function for fibonacci
function fib(n) {
    // Stop condition
    if (n === 0) return 0;

    // Stop condition
    if (n === 1 || n === 2) return 1;

    // Recursion function
    return fib(n - 1) + fib(n - 2);
}

// Driver Code
let n = 5;
console.log("Fibonacci series of 5 numbers is:");

// for loop to print the fibonacci series.
for (let i = 0; i < n; i++) {
    console.log(fib(i) + " ");
}
```



## Python


```
# Function for fibonacci
def fib(n):

    # Stop condition
    if (n == 0):
        return 0

    # Stop condition
    if (n == 1 or n == 2):
        return 1

    # Recursion function
    else:
        return (fib(n - 1) + fib(n - 2))


# Driver Code

# Initialize variable n.
n = 5;
print("Fibonacci series of 5 numbers is :",end=" ")

# for loop to print the fibonacci series.
for i in range(0,n): 
    print(fib(i),end=" ")
```

---

# Memoization

Memoization is a specific form of caching that is used in dynamic programming. The purpose of caching is to improve the performance of our programs and keep data accessible that can be used later. It basically stores the previously calculated result of the subproblem and reuses the stored result for the same subproblem. This removes the extra effort to calculate again and again for the same problem.  


## Example

The Fibonacci sequence is a classic example of how memoization can optimize recursive algorithms by eliminating redundant computations.

The Fibonacci sequence is defined as:

**Base Case:** F(0) = 0 and F(1) = 1

**Recursive Cases:** F(n) = F(n-1) + F(n-2)

Using recursion, solving F(n) involves repeatedly breaking the problem into smaller subproblems. However, many of these subproblems are recalculated multiple times, leading to inefficiency. For instance, computing F(5) will independently calculate F(3) and F(2) multiple times. By using memoization, we store the results of already computed subproblems in a cache, allowing us to reuse them whenever the same subproblem arises again. This eliminates redundant calculations and significantly improves efficiency. 

Without memoization, the time complexity of finding the nth Fibonacci number using recursion is O(2^n), as the function repeatedly solves overlapping subproblems, creating an exponential number of recursive calls. For instance, F(3) and F(2) are recalculated multiple times when computing F(5), leading to inefficiency. With memoization, the time complexity reduces to O(n) because each Fibonacci number is computed only once and stored for reuse. This eliminates redundant computations and ensures a linear traversal from F(0) and F(n), significantly improving performance.

---

# Resources

Here are some resources to help you understand and implement recursion:

[Introduction to Recursion | GeeksforGeeks](https://www.geeksforgeeks.org/introduction-to-recursion-2/)

[Recursive Algorithms | GeeksforGeeks](https://www.geeksforgeeks.org/recursion-algorithms/)

---

# Practice Problems


## Easy



1. [Power of Four - LeetCode](https://leetcode.com/problems/power-of-four/description/) | [Solution](https://www.geeksforgeeks.org/find-whether-a-given-number-is-a-power-of-4-or-not/)

     

2. [Generate Parentheses - LeetCode](http://leetcode.com/problems/generate-parentheses/description/) | [Solution](https://www.geeksforgeeks.org/print-all-combinations-of-balanced-parentheses/) 
3. [Subsets - LeetCode](https://leetcode.com/problems/subsets/description/) | [Solution](https://www.geeksforgeeks.org/backtracking-to-find-all-subsets/) 
4. [Letter Combinations of a Phone Number - LeetCode](https://leetcode.com/problems/letter-combinations-of-a-phone-number/description/) | [Solution](https://www.geeksforgeeks.org/find-possible-words-phone-digits/) 
5. [String to Integer (atoi) - LeetCode](https://leetcode.com/problems/string-to-integer-atoi/description/) | [Solution](https://www.geeksforgeeks.org/recursive-implementation-of-atoi/) 
6. [Pow(x, n) - LeetCode](https://leetcode.com/problems/powx-n/description/) | [Solution](https://www.geeksforgeeks.org/write-a-c-program-to-calculate-powxn/#naive-approach-2-using-recursion-on-time-and-on-space) 
7. [Unique 3-Digit Even Numbers - LeetCode](https://leetcode.com/problems/unique-3-digit-even-numbers/description/) | [Solution](https://www.geeksforgeeks.org/find-all-distinct-three-digit-numbers-from-given-array-of-digits/) 


## Medium



1. [Combination Sum - LeetCode](https://leetcode.com/problems/combination-sum/description/) | [Solution](https://www.geeksforgeeks.org/combinational-sum/) 
2. [Combination Sum II - LeetCode](https://leetcode.com/problems/combination-sum-ii/description/) | [Solution](https://www.youtube.com/watch?v=mcn27eoRfZ0&t=1s) 
3. [Subsets II - LeetCode](https://leetcode.com/problems/subsets-ii/description/) | [Solution](https://www.geeksforgeeks.org/find-all-unique-subsets-of-a-given-set/) 
4. [Combination Sum III - LeetCode](https://leetcode.com/problems/combination-sum-iii/description/) | [Solution](https://www.geeksforgeeks.org/generate-all-possible-combinations-of-k-numbers-that-sums-to-n/) 
5. [Palindrome Partitioning - LeetCode](https://leetcode.com/problems/palindrome-partitioning/description/) | [Solution](https://www.geeksforgeeks.org/palindrome-partitioning-dp-17/) 


## Hard



1. [Word Break - LeetCode](https://leetcode.com/problems/word-break/description/) | [Solution](https://www.geeksforgeeks.org/word-break-problem-dp-32/) 
2. [Expression Add Operators - LeetCode](https://leetcode.com/problems/expression-add-operators/description/) | [Solution](https://www.geeksforgeeks.org/print-all-possible-expressions-that-evaluate-to-a-target/) 
3. [Sudoku Solver - LeetCode](https://leetcode.com/problems/sudoku-solver/description/) | [Solution](https://www.geeksforgeeks.org/sudoku-backtracking-7/) 
