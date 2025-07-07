# Longest Common Subsequence

Given two strings, s1 and s2, the task is to find the length of the Longest Common Subsequence. If there is no common subsequence, return 0. A subsequence is a string generated from the original string by deleting 0 or more characters, without changing the relative order of the remaining characters.

For example, subsequences of "ABC" are "", "A", "B", "C", "AB", "AC", "BC" and "ABC". In general, a string of length n has 2n subsequences.

## Naive approach using recursion
**Time complexity:** O(2^min(m, n))  
**Space complexity:** O(min(m, n))  
The idea is to compare the last characters of s1 and s2. While comparing the strings s1 and s2 two cases arise:

1. Match : Make the recursion call for the remaining strings (strings of lengths m-1 and n-1) and add 1 to result.
2. Do not Match : Make two recursive calls. First for lengths m-1 and n, and second for m and n-1. Take the maximum of two results. 
Base case : If any of the strings become empty, we return 0.

### C++
```cpp
// A Naive recursive implementation of LCS problem
#include <bits/stdc++.h>
using namespace std;

// Returns length of LCS for s1[0..m-1], s2[0..n-1]
int lcsRec(string &s1, string &s2,int m,int n) {
    
    // Base case: If either string is empty, the length of LCS is 0
    if (m == 0 || n == 0)
        return 0;

    // If the last characters of both substrings match
    if (s1[m - 1] == s2[n - 1])
      
        // Include this character in LCS and recur for remaining substrings
        return 1 + lcsRec(s1, s2, m - 1, n - 1);

    else
        // If the last characters do not match
        // Recur for two cases:
        // 1. Exclude the last character of s1 
        // 2. Exclude the last character of s2 
        // Take the maximum of these two recursive calls
        return max(lcsRec(s1, s2, m, n - 1), lcsRec(s1, s2, m - 1, n));
}
int lcs(string &s1,string &s2){
    
    int m = s1.size(), n = s2.size();
    return lcsRec(s1,s2,m,n);
}

int main() {
    string s1 = "AGGTAB";
    string s2 = "GXTXAYB";
    int m = s1.size();
    int n = s2.size();

    cout << lcs(s1, s2) << endl;

    return 0;
}
```

### Python
```python
# A Naive recursive implementation of LCS problem

# Returns length of LCS for s1[0..m-1], s2[0..n-1]
def lcsRec(s1, s2, m, n):
  
    # Base case: If either string is empty, the length of LCS is 0
    if m == 0 or n == 0:
        return 0

    # If the last characters of both substrings match
    if s1[m - 1] == s2[n - 1]:

        # Include this character in LCS and recur for remaining substrings
        return 1 + lcsRec(s1, s2, m - 1, n - 1)

    else:
        # If the last characters do not match
        # Recur for two cases:
        # 1. Exclude the last character of S1 
        # 2. Exclude the last character of S2 
        # Take the maximum of these two recursive calls
        return max(lcsRec(s1, s2, m, n - 1), lcsRec(s1, s2, m - 1, n))

def lcs(s1,s2):
    m = len(s1)
    n = len(s2)
    return lcsRec(s1,s2,m,n)

if __name__ == "__main__":
    s1 = "AGGTAB"
    s2 = "GXTXAYB"
    print(lcs(s1, s2))
```

### Javascript
```javascript
// A Naive recursive implementation of LCS problem

// Returns length of LCS for s1[0..m-1], s2[0..n-1]
function lcsRec(s1, s2, m, n) {
  
    // Base case: If either string is empty, the length of LCS is 0
    if (m === 0 || n === 0)
        return 0;

    // If the last characters of both substrings match
    if (s1[m - 1] === s2[n - 1])

        // Include this character in LCS and recur for remaining substrings
        return 1 + lcsRec(s1, s2, m - 1, n - 1);

    else
        return Math.max(lcsRec(s1, s2, m, n - 1), lcsRec(s1, s2, m - 1, n));
}
function lcs(s1,s2){
    
    let m = s1.length;
    let n = s2.length;
    return lcsRec(s1,s2,m,n);
}

// driver code
let s1 = "AGGTAB";
let s2 = "GXTXAYB";
let m = s1.length;
let n = s2.length;

console.log(lcs(s1, s2, m, n));
```

---

## Using memoization
**Time complexity:** O(m*n)  
**Space complexity:** O(m*n)  
To optimize the recursive solution, we use a 2D memoization table of size (m+1)×(n+1)(m+1) \times (n+1)(m+1)×(n+1), initialized to −1-1−1 to track computed values. Before making recursive calls, we check this table to avoid redundant computations of overlapping subproblems. This prevents repeated calculations, improving efficiency through memoization or tabulation.

![img](./src/lcs_memo.jpg)

### C++
```cpp
// C++ implementation of Top-Down DP
// of LCS problem
#include <bits/stdc++.h>
using namespace std;
// Returns length of LCS for s1[0..m-1], s2[0..n-1]
int lcsRec(string &s1, string &s2, int m, int n, vector<vector<int>> &memo) {

    // Base Case
    if (m == 0 || n == 0)
        return 0;

    // Already exists in the memo table
    if (memo[m][n] != -1)
        return memo[m][n];

    // Match
    if (s1[m - 1] == s2[n - 1])
        return memo[m][n] = 1 + lcsRec(s1, s2, m - 1, n - 1, memo);

    // Do not match
    return memo[m][n] = max(lcsRec(s1, s2, m, n - 1, memo), lcsRec(s1, s2, m - 1, n, memo));
}
int lcs(string &s1,string &s2){
    int m = s1.length();
    int n = s2.length();
    vector<vector<int>> memo(m + 1, vector<int>(n + 1, -1));
    return lcsRec(s1, s2, m, n, memo);
}

int main() {
    string s1 = "AGGTAB";
    string s2 = "GXTXAYB";
    cout << lcs(s1, s2) << endl;
    return 0;
}
```

### Python
```python
def lcsRec(s1, s2, m, n, memo):
    # Base Case
    if m == 0 or n == 0:
        return 0

    # Already exists in the memo table
    if memo[m][n] != -1:
        return memo[m][n]

    # Match
    if s1[m - 1] == s2[n - 1]:
        memo[m][n] = 1 + lcsRec(s1, s2, m - 1, n - 1, memo)
        return memo[m][n]

    # Do not match
    memo[m][n] = max(lcsRec(s1, s2, m, n - 1, memo),
                     lcsRec(s1, s2, m - 1, n, memo))
    return memo[m][n]


def lcs(s1, s2):
    m = len(s1)
    n = len(s2)
    memo = [[-1 for _ in range(n + 1)] for _ in range(m + 1)]
    return lcsRec(s1,s2,m,n,memo)
    
if __name__ == "__main__":
    s1 = "AGGTAB"
    s2 = "GXTXAYB"
    print(lcs(s1, s2))
```

### Javascript
```javascript
// A Top-Down DP implementation of LCS problem

// Returns length of LCS for s1[0..m-1], s2[0..n-1]
function lcsRec(s1, s2, m, n, memo)
{
    // Base Case
    if (m === 0 || n === 0)
        return 0;

    // Already exists in the memo table
    if (memo[m][n] !== -1)
        return memo[m][n];

    // Match
    if (s1[m - 1] === s2[n - 1]) {
        memo[m][n] = 1 + lcsRec(s1, s2, m - 1, n - 1, memo);
        return memo[m][n];
    }

    // Do not match
    memo[m][n] = Math.max(lcsRec(s1, s2, m, n - 1, memo),
                          lcsRec(s1, s2, m - 1, n, memo));
    return memo[m][n];
}

function lcs(s1, s2)
{

    const m = s1.length;
    const n = s2.length;
    const memo = Array.from({length : m + 1},
                            () => Array(n + 1).fill(-1));

    return lcsRec(s1, s2, m, n, memo);
}
// driver code
const s1 = "AGGTAB";
const s2 = "GS1TS1AS2B";

console.log(lcs(s1, s2));
```

---

## Using Tabulation
**Time complexity:** O(m*n)  
**Space complexity:** O(m*n)  
There are two parameters that change in the recursive solution and these parameters go from 0 to m and 0 to n. So we create a 2D dp array of size (m+1) x (n+1).  

- We first fill the known entries when m is 0 or n is 0.
- Then we fill the remaining entries using the recursive formula.

### C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

// Returns length of LCS for s1[0..m-1], s2[0..n-1]
int lcs(string &s1, string &s2) {
    int m = s1.size();
    int n = s2.size();

    // Initializing a matrix of size (m+1)*(n+1)
    vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));

    // Building dp[m+1][n+1] in bottom-up fashion
    for (int i = 1; i <= m; ++i) {
        for (int j = 1; j <= n; ++j) {
            if (s1[i - 1] == s2[j - 1])
                dp[i][j] = dp[i - 1][j - 1] + 1;
            else
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1]);
        }
    }

    // dp[m][n] contains length of LCS for s1[0..m-1]
    // and s2[0..n-1]
    return dp[m][n];
}

int main() {
    string s1 = "AGGTAB";
    string s2 = "GXTXAYB";
    cout << lcs(s1, s2) << endl;

    return 0;
}
```

### Python
```python
def lcs(S1, S2):
    m = len(S1)
    n = len(S2)

    # Initializing a matrix of size (m+1)*(n+1)
    dp = [[0] * (n + 1) for x in range(m + 1)]

    # Building dp[m+1][n+1] in bottom-up fashion
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if S1[i - 1] == S2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j],
                               dp[i][j - 1])

    # dp[m][n] contains length of LCS for S1[0..m-1]
    # and S2[0..n-1]
    return dp[m][n]


if __name__ == "__main__":
    S1 = "AGGTAB"
    S2 = "GXTXAYB"
    print(lcs(S1, S2))
```

### Javascript
```javascript
function lcs(S1, S2) {
    const m = S1.length;
    const n = S2.length;

    // Initializing a matrix of size (m+1)*(n+1)
    const dp = Array.from({length : m + 1},
                          () => Array(n + 1).fill(0));

    // Building dp[m+1][n+1] in bottom-up fashion
    for (let i = 1; i <= m; i++) {
        for (let j = 1; j <= n; j++) {
            if (S1[i - 1] === S2[j - 1]) {
                dp[i][j] = dp[i - 1][j - 1] + 1;
            }
            else {
                dp[i][j]
                    = Math.max(dp[i - 1][j], dp[i][j - 1]);
            }
        }
    }

    // dp[m][n] contains length of LCS for
    // S1[0..m-1] and S2[0..n-1]
    return dp[m][n];
}

const S1 = "AGGTAB";
const S2 = "GXTXAYB";
console.log(lcs(S1, S2));
```

---

## Using space optimization DP
**Time complexity:** O(m*n)  
**Space complexity:** O(n)  
In this approach, the space complexity is further optimized by using a single DP array, where: 
dp[j] represents the value of dp[i-1][j] (previous row's value) before updating. During the computation, dp[j] is updated to represent the current row value dp[i][j]
Now the recurrance relations become:

- If the characters s1[i-1] and s2[j-1] match, dp[j] = 1+ prev. Here, prev is a temporary variable storing the diagonal value (dp[i-1][j-1]).
- If the characters don't match, dp[j] = max(dp[j-1], dp[j]). Here dp[j] represents the value of dp[i-1][j] before updating and dp[j-1] represents the value of dp[i-1][j]. 

### C++
```cpp
// C++ program to find the longest common subsequence of two strings
// using space optimization
 
#include <iostream>
#include <vector>
using namespace std;

int lcs(string &s1, string &s2) {
  
    int m = s1.length(), n = s2.length();

    // dp vector is initialized to all zeros
    // This vector stores the LCS values for the current row.
    // dp[j] represents LCS of s1[0..i] and s2[0..j]
    vector<int> dp(n + 1, 0);

    // i and j represent the lengths of s1 and s2 respectively
    for (int i = 1; i <= m; ++i) {

        // prev stores the value from the previous
        // row and previous column (i-1), (j -1)
        // Used to keep track of LCS[i-1][j-1] while updating dp[j]
        int prev = dp[0];

        for (int j = 1; j <= n; ++j) {

            // temp temporarily stores the current
            // dp[j] before it gets updated
            int temp = dp[j];

            // If characters match, add 1 to the value
            // from the previous row and previous column
            // dp[j] = 1 + LCS[i-1][j-1]
            if (s1[i - 1] == s2[j - 1])
                dp[j] = 1 + prev;
            else
                // Otherwise, take the maximum of the
                // left (dp[j-1]) and top (dp[j]) values
                dp[j] = max(dp[j - 1], dp[j]);

            // Update prev for the next iteration
            // This keeps the value of the previous
          // row (i-1) for future comparisons
            prev = temp;
        }
    }

    // The last element of the vector contains the length of the LCS
    // dp[n] stores the length of LCS of s1[0..m] and s2[0..n]
    return dp[n];
}

int main() {
    string s1 = "AGGTAB", s2 = "GXTXAYB";
    cout << lcs(s1, s2);
    return 0;
}
```

### Python
```python
# Python program to find the longest common subsequence of two strings
# using space optimization
 
def lcs(s1, s2):
    m = len(s1)
    n = len(s2)
    
    # dp array is initialized to all zeros
    dp = [0] * (n + 1)

    # i and j represent the lengths of s1 
    # and s2 respectively
    for i in range(1, m + 1):
      
        # prev stores the value from the previous 
        # row and previous column (i-1), (j -1)
        prev = dp[0]
      
        for j in range(1, n + 1):
          
            # temp temporarily stores the current 
            # dp[j] before it gets updated
            temp = dp[j]
            if s1[i - 1] == s2[j - 1]:
              
                # If characters match, add 1 to the value 
                # from the previous row and previous column
                dp[j] = 1 + prev
            else:
              
                # Otherwise, take the maximum of the 
                # left and top values
                dp[j] = max(dp[j - 1], dp[j])
                
            # Update prev for the next iteration
            prev = temp
      
    # The last element of the list contains
    # the length of the LCS
    return dp[n]
 
if __name__ == "__main__":
    s1 = "AGGTAB"
    s2 = "GXTXAYB"
    res = lcs(s1, s2)
    print(res)
```

### Javascript
```javascript
// JavaScript program to find the longest common subsequence of two strings
// using space optimization
  
function lcs(s1, s2) {
    const m = s1.length;
    const n = s2.length;

    // dp array is initialized to all zeros
    const dp = Array(n + 1).fill(0);

    // i and j represent the lengths of s1 and s2
    // respectively
    for (let i = 1; i <= m; ++i) {

        // prev stores the value from the previous
        // row and previous column (i-1), (j -1)
        let prev = dp[0];

        for (let j = 1; j <= n; ++j) {

            // temp temporarily stores the current
            // dp[j] before it gets updated
            const temp = dp[j];
            if (s1[i - 1] === s2[j - 1]) {

                // If characters match, add 1 to the value
                // from the previous row and previous column
                dp[j] = 1 + prev;
            }
            else {

                // Otherwise, take the maximum of the
                // left and top values
                dp[j] = Math.max(dp[j - 1], dp[j]);
            }

            // Update prev for the next iteration
            prev = temp;
        }
    }

    // The last element of the array 
    // contains the length of the LCS
    return dp[n];
}

const s1 = "AGGTAB";
const s2 = "GXTXAYB";
const res = lcs(s1, s2);
console.log(res);
```
