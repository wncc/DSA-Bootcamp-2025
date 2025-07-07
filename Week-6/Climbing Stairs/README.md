# Climbing Stairs Problem

There are n stairs, and a person standing at the bottom wants to climb stairs to reach the top. The person can climb either 1 stair or 2 stairs at a time, the task is to count the number of ways that a person can reach at the top.

There are multiple ways to solve this problem in varying levels of complexity. 

---

## Using Memoization
**Time complexity:** O(n)  
**Space complexity:** O(n)  
Number of ways to reach the nth stair, i.e., countWays(n), depends on the optimal solutions of the subproblems countWays(n-1) and countWays(n-2). By combining these optimal substructures, we can efficiently calculate the total number of ways to reach the nth stair. There is only one parameter that changes in the recursive solution and it can go from 0 to n. So we create a 1D array of size n+1 for memoization.

### C++
```cpp
// C++ program to count number of ways to reach 
// nth stair using memoization

#include <iostream>
#include <vector>
using namespace std;

int countWaysRec(int n, vector<int>& memo) {
  
  	// Base cases
    if (n == 0 || n == 1)
        return 1;

  	// if the result for this subproblem is 
  	// already computed then return it
    if (memo[n] != -1) 
        return memo[n];
    
    return memo[n] = countWaysRec(n - 1, memo) +
      				 	countWaysRec(n - 2, memo);
}

int countWays(int n) {
  
  	// Memoization array to store the results
  	vector<int> memo(n + 1, -1);
  	return countWaysRec(n, memo);
}

int main() {
    int n = 4;
    cout << countWays(n);
    return 0;
}
```

### Python
```python
# Python program to count number of ways to reach nth stair 
# using memoization

def countWaysRec(n, memo):
  
    # Base cases
    if n == 0 or n == 1:
        return 1

    # if the result for this subproblem is 
    # already computed then return it
    if memo[n] != -1:
        return memo[n]

    memo[n] = countWaysRec(n - 1, memo) + countWaysRec(n - 2, memo)
    return memo[n]

def countWays(n):
  
    # Memoization array to store the results
    memo = [-1] * (n + 1)
    return countWaysRec(n, memo)

if __name__ == "__main__":
    n = 4
    print(countWays(n))
```

### Javascript
```javascript
// JavaScript program to count number of ways to reach nth stair
// using memoization

function countWaysRec(n, memo) {

    // Base cases
    if (n === 0 || n === 1)
        return 1;

    // if the result for this subproblem is 
    // already computed then return it
    if (memo[n] !== -1) 
        return memo[n];

    memo[n] = countWaysRec(n - 1, memo) + countWaysRec(n - 2, memo); 
    return memo[n];
}

function countWays(n) {

    // Memoization array to store the results
    let memo = Array(n + 1).fill(-1);
    return countWaysRec(n, memo);
}

const n = 4;
console.log(countWays(n));
```

---


## Using Tabulation
**Time complexity:** O(n)  
**Space complexity:** O(n)  
The approach is similar to the previous one; just instead of breaking down the problem recursively, we iteratively build up the solution by calculating in bottom-up manner. Maintain a dp[] table such that dp[i] stores the number of ways to reach ith stair. 

- Base Case: For i = 0 and i = 1, dp[i] = 1
- Recursive Case: For i > 1, dp[i] = dp[i - 1] + dp[i - 2]

### C++
```cpp
// C++ program to count number of ways 
// to reach nth stair using Tabulation

#include <iostream>
#include <vector>
using namespace std;

int countWays(int n) {
    vector<int> dp(n + 1, 0);
  
    // Base cases
    dp[0] = 1;
    dp[1] = 1;
	
    for (int i = 2; i <= n; i++)
        dp[i] = dp[i - 1] + dp[i - 2]; 
  
    return dp[n];
}

int main() {
    int n = 4;
    cout << countWays(n);
    return 0;
}
```

### Python
```python
# Python program to count number of ways 
# to reach nth stair using Tabulation

def countWays(n):
    dp = [0] * (n + 1)
  
    # Base cases
    dp[0] = 1
    dp[1] = 1

    for i in range(2, n + 1):
        dp[i] = dp[i - 1] + dp[i - 2]; 
  
    return dp[n]

n = 4
print(countWays(n))
```

### Javascript
```javascript
// JavaScript program to count number of ways 
// to reach nth stair using tabulation

function countWays(n) {
    const dp = new Array(n + 1).fill(0);
  
    // Base cases
    dp[0] = 1;
    dp[1] = 1;

    for (let i = 2; i <= n; i++)
        dp[i] = dp[i - 1] + dp[i - 2]; 
  
    return dp[n];
}

const n = 4;
console.log(countWays(n));
```

---

## Using Space Optimized DP
**Time complexity:** O(n)  
**Space complexity:** O(1)  
In the above approach, we observe that each subproblem only depends on the results of previous two subproblems, that is dp[i] depends on dp[i - 1] and dp[i - 2] only. So, we can optimize the space complexity by using just two variables to store these last two values. 

### C++
```cpp
// C++ program to count number of ways to
// reach nth stair using Space Optimized DP

#include <iostream>
#include <vector>
using namespace std;

int countWays(int n) {
  
    // variable prev1, prev2 to store the
  	// values of last and second last states 
    int prev1 = 1;
    int prev2 = 1;
  
  	for (int i = 2; i <= n; i++) {
        int curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
  
  	// In last iteration final value
  	// of curr is stored in prev.
    return prev1;
}

int main() {
    int n = 4;
    cout << countWays(n);
    return 0;
}
```

### Python
```python
# Python program to count number of ways to
# reach nth stair using Space Optimized DP

def countWays(n):
  
    # variable prev1, prev2 - to store the
    # values of last and second last states 
    prev1 = 1
    prev2 = 1
  
    for i in range(2, n + 1):
        curr = prev1 + prev2
        prev2 = prev1
        prev1 = curr
  
    # In last iteration final value
    # of curr is stored in prev.
    return prev1

n = 4
print(countWays(n))
```

### Javascript
```javascript
// JavaScript program to count number of ways to
// reach nth stair using Space Optimized DP

function countWays(n) {
  
    // variable prev1, prev2 - to store the
    // values of last and second last states 
    let prev1 = 1;
    let prev2 = 1;
  
    for (let i = 2; i <= n; i++) {
        let curr = prev1 + prev2;
        prev2 = prev1;
        prev1 = curr;
    }
  
    // In last iteration final value
    // of curr is stored in prev.
    return prev1;
}

const n = 4;
console.log(countWays(n));
```

---

## Using Matrix Exponentiation
**Time complexity:** O(logn)  
**Space complexity:** O(1)  
This problem is quite similar to the Fibonacci approach, with only one difference in the base case (the Fibonacci sequence value for 0 is 0, whereas in this problem it is 1). 

### C++
```cpp
// C++ Program to count no. of ways to reach
// nth stairs using matrix Exponentiation

#include <iostream>
#include <vector>
using namespace std;

// function to multiply two 2x2 Matrices
void multiply(vector<vector<int>>& a, vector<vector<int>>& b) {
  
    // Matrix to store the result
    vector<vector<int>> res(2, vector<int>(2));

    // Matrix Multiplication
    res[0][0] = a[0][0] * b[0][0] + a[0][1] * b[1][0];
    res[0][1] = a[0][0] * b[0][1] + a[0][1] * b[1][1];
    res[1][0] = a[1][0] * b[0][0] + a[1][1] * b[1][0];
    res[1][1] = a[1][0] * b[0][1] + a[1][1] * b[1][1];

    // Copy the result back to the first matrix
    a[0][0] = res[0][0];
    a[0][1] = res[0][1];
    a[1][0] = res[1][0];
    a[1][1] = res[1][1];
}

// Function to find (m[][] ^ expo)
vector<vector<int>> power(vector<vector<int>>& m, int expo) {
  
    // Initialize result with identity matrix
    vector<vector<int>> res = { { 1, 0 }, { 0, 1 } };

    while (expo) {
        if (expo & 1)
            multiply(res, m);
        multiply(m, m);
        expo >>= 1;
    }

    return res;
}

int countWays(int n) {
  
    // base case
    if (n == 0 || n == 1)
        return 1;

    vector<vector<int>> m = { { 1, 1 }, { 1, 0 } };
  
    // Matrix initialMatrix = {{f(1), 0}, {f(0), 0}}, where f(0)
    // and f(1) are first two terms of sequence
    vector<vector<int>> initialMatrix = { { 1, 0 }, { 1, 0 } };

    // Multiply matrix m (n - 1) times
    vector<vector<int>> res = power(m, n - 1);

    multiply(res, initialMatrix);

    return res[0][0];
}

int main() {
    int n = 4;
    cout << countWays(n);
    return 0;
}
```

### Python
```python
# Python Program to count no. of ways to reach
# nth stairs using matrix Exponentiation

def multiply(a, b):
    
    # Matrix to store the result
    res = [[0, 0], [0, 0]]

    # Matrix Multiplication
    res[0][0] = a[0][0] * b[0][0] + a[0][1] * b[1][0]
    res[0][1] = a[0][0] * b[0][1] + a[0][1] * b[1][1]
    res[1][0] = a[1][0] * b[0][0] + a[1][1] * b[1][0]
    res[1][1] = a[1][0] * b[0][1] + a[1][1] * b[1][1]

    # Copy the result back to the first matrix
    a[0][0] = res[0][0]
    a[0][1] = res[0][1]
    a[1][0] = res[1][0]
    a[1][1] = res[1][1]

def power(m, expo):
    
    # Initialize result with identity matrix
    res = [[1, 0], [0, 1]]

    while expo > 0:
        if expo & 1:
            multiply(res, m)
        multiply(m, m)
        expo >>= 1

    return res

def countWays(n):
    
    # base case
    if n == 0 or n == 1:
        return 1

    m = [[1, 1], [1, 0]]
    
    # Matrix initialMatrix = {{f(1), 0}, {f(0), 0}}, where f(0)
    # and f(1) are first two terms of sequence
    initialMatrix = [[1, 0], [1, 0]]

    # Multiply matrix m (n - 1) times
    res = power(m, n - 1)

    multiply(res, initialMatrix)

    # Return the final result as an integer
    return res[0][0]

n = 4
print(countWays(n))
```

### Javascript
```javascript
// JavaScript Program to count no. of ways to reach
// nth stairs using matrix Exponentiation

function multiply(a, b) {
    
    // Matrix to store the result
    const res = [
        [0, 0],
        [0, 0]
    ];

    // Matrix Multiplication
    res[0][0] = a[0][0] * b[0][0] + a[0][1] * b[1][0];
    res[0][1] = a[0][0] * b[0][1] + a[0][1] * b[1][1];
    res[1][0] = a[1][0] * b[0][0] + a[1][1] * b[1][0];
    res[1][1] = a[1][0] * b[0][1] + a[1][1] * b[1][1];

    // Copy the result back to the first matrix
    a[0][0] = res[0][0];
    a[0][1] = res[0][1];
    a[1][0] = res[1][0];
    a[1][1] = res[1][1];
}

function power(m, expo) {
    
    // Initialize result with identity matrix
    const res = [
        [1, 0],
        [0, 1]
    ];

    while (expo > 0) {
        if (expo & 1) 
            multiply(res, m);
        multiply(m, m);
        expo >>= 1;
    }

    return res;
}

function countWays(n) {
    
    // base case
    if (n === 0 || n === 1)
        return 1;

    const m = [
        [1, 1],
        [1, 0]
    ];
    
    // Matrix initialMatrix = {{f(1), 0}, {f(0), 0}}, where f(0)
    // and f(1) are first two terms of sequence
    const initialMatrix = [
        [1, 0],
        [1, 0]
    ];

    // Multiply matrix m (n - 1) times
    const res = power(m, n - 1);

    multiply(res, initialMatrix);

    return res[0][0];
}

const n = 4;
console.log(countWays(n));
```
