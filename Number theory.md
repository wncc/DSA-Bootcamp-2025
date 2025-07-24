# **Number Theory**


### **1. Sieve of Eratosthenes**

The **Sieve of Eratosthenes** is a preprocessing algorithm that builds an array which allows us to efficiently determine whether a given number between 2 and *n* is prime. If the number is not prime, we can also find one of its prime factors using this array.

The algorithm constructs an array `sieve` where the indices 2, 3, ..., *n* are used. The value `sieve[k] = 0` means that *k* is prime. If `sieve[k] ≠ 0`, it means that *k* is not a prime number, and one of its prime factors is `sieve[k]`.

The algorithm iterates over the numbers from 2 to *n* one at a time. Whenever a new prime *x* is encountered, it marks all multiples of *x* (i.e., 2x, 3x, 4x, ...) as non-prime by setting their sieve values to *x*. This is because *x* divides those numbers.

For example, if *n* = 20, the array is built as follows:

|  2 |  3 |  4 |  5 |  6 |  7 |  8 |  9 | 10 | 11 | 12 | 13 | 14 | 15 | 16 | 17 | 18 | 19 | 20 |
|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|----|
|  0 |  0 |  2 |  0 |  3 |  0 |  2 |  3 |  5 |  0 |  3 |  0 |  7 |  5 |  2 |  0 |  3 |  0 |  5 |


### **Time Complexity Analysis:**

The inner loop of the algorithm is executed `n/x` times for each value of `x`. Therefore, the total running time is upper-bounded by the harmonic sum:

∑ (n / x) from x = 2 to n

= n/2 + n/3 + n/4 + ... + n/n

= O(n log log n)

The inner loop of the algorithm is executed n/x times for each value of x. Thus, an upper bound for the running time of the algorithm is the harmonic sum Xn x=2 n/x = n/2+ n/3+ n/4+··· + n/n = O(nlogn). In fact, the algorithm is more efficient, because the inner loop will be executed only if the number x is prime. It can be shown that the running time of the algorithm is only O(nlog logn), a complexity very near to O(n).

The following code implements the sieve of Eratosthenes. The code assumes that each element of sieve is initially zero


#### **C++ code:**


```
vector<bool> sieve(int n) {
    vector<bool> isPrime(n + 1, true);
    isPrime[0] = isPrime[1] = false;
    for (int i = 2; i * i <= n; ++i) {
        if (isPrime[i]) {
            for (int j = i * i; j <= n; j += i)
                isPrime[j] = false;
        }
    }
    return isPrime;
}
```



#### **Python Code:**


```
def sieve(n):
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n**0.5) + 1):
        if is_prime[i]:
            for j in range(i*i, n+1, i):
                is_prime[j] = False
    return is_prime
```



#### **JavaScript Code:**


```
function sieve(n) {
    const isPrime = Array(n + 1).fill(true);
    isPrime[0] = isPrime[1] = false;
    for (let i = 2; i * i <= n; i++) {
        if (isPrime[i]) {
            for (let j = i * i; j <= n; j += i) {
                isPrime[j] = false;
            }
        }
    }
    return isPrime;
}
```



### **2. Euclidean Algorithm (GCD)**

The **greatest common divisor** of numbers *a* and *b*, **gcd(a, b)**, is the greatest number that divides both *a* and *b*, and the **least common multiple** of *a* and *b*, **lcm(a, b)**, is the smallest number that is divisible by both *a* and *b*. For example, gcd(24, 36) = 12 and lcm(24, 36) = 72.

The greatest common divisor and the least common multiple are connected as follows:

lcm(a, b) = (a × b) / gcd(a, b)

**Euclid’s algorithm**¹ provides an efficient way to find the greatest common divisor of two numbers. The algorithm is based on the following formula:

gcd(a, b) =
    a                  if b = 0
    gcd(b, a mod b)    if b ≠ 0


For example, \
 gcd(24, 36) = gcd(36, 24) = gcd(24, 12) = gcd(12, 0) = 12.

The algorithm can be implemented as follows:

**C++:**


```
int gcd(int a, int b) {
    return b == 0 ? a : gcd(b, a % b);
}
```


**Python:**


```
def gcd(a, b):
    return a if b == 0 else gcd(b, a % b)
```


**JavaScript:**


```
function gcd(a, b) {
    return b === 0 ? a : gcd(b, a % b);
}
```


It can be shown that Euclid’s algorithm works in O(log n) time, where n = min(a, b). The worst case for the algorithm is the case when *a* and *b* are consecutive Fibonacci numbers. For example,

gcd(13, 8) = gcd(8, 5) = gcd(5, 3) = gcd(3, 2) = gcd(2, 1) = gcd(1, 0) = 1.


---


## **Modular exponentiation**

There is often need to efficiently calculate the value of `xⁿ mod m`. This can be done in **O(log n)** time using the following recursive routine:

xⁿ mod m =

    (x²)ⁿ/² mod m     if n is even

    x ⋅ xⁿ⁻¹ mod m    if n is odd

It is important that in the case of an even `n`, the value of `x²` is calculated only once. This guarantees that the time complexity of the algorithm is O(log n), because it always halves what is in the exponent.

The following function calculates the value of `xⁿ mod m`:

**C++:**


```
int modExpo(int a, int b, int mod) {
    int result = 1;
    a %= mod;
    while (b > 0) {
        if (b % 2 == 1) result = (1LL * result * a) % mod;
        a = (1LL * a * a) % mod;
        b /= 2;
    }
    return result;
}
```


**Python:**


```
def mod_exp(a, b, mod):
    result = 1
    a %= mod
    while b:
        if b % 2:
            result = (result * a) % mod
        a = (a * a) % mod
        b //= 2
    return result
```


**JavaScript:**


```
function modExp(a, b, mod) {
    let result = 1;
    a %= mod;
    while (b > 0) {
        if (b % 2 === 1) result = (result * a) % mod;
        a = (a * a) % mod;
        b = Math.floor(b / 2);
    }
    return result;
}
```



---


### **4. Modular Inverse**

The inverse of a modulo `m` is a number `x⁻¹` such that:


```
x ⋅ x⁻¹ mod m = 1
```


For example, if `x = 6` and `m = 17`, then `x⁻¹ = 3`, because `6 ⋅ 3 mod 17 = 1`.

Using modular inverse, we can divide numbers modulo `m`, because division by `x` corresponds to multiplication by `x⁻¹`. For example, to evaluate the value of `6^360 mod 17`

we can use the formula `2 ⋅ 3 mod 17`, because `36 mod 17 = 2` and `2 ⋅ 3 = 6`.

However, a modular inverse does not always exist. For example, if `x = 2` and `m = 4`, the equation:


```
2x mod 4 = 1
```


cannot be solved, because all multiples of 2 are even and the remainder can never be 1 when `m = 4`. It turns out that the value of `x⁻¹ mod m` can be calculated when `x` and `m` are coprime.

A modular inverse can be calculated using the formula:


```
x⁻¹ = x^(φ(m) − 1) mod m
```


If `m` is prime, the formula becomes:


```
x⁻¹ = x^(p − 2) mod p
```


For example:


```
6⁻¹ mod 17 = 6^15 mod 17 = 3
```


This formula allows us to efficiently calculate modular inverses using the same modular exponentiation routine. This formula can be derived using Euler’s theorem. First, the modular inverse should satisfy the following equation:


```
x ⋅ x⁻¹ mod m = 1
```


On the other hand, according to Euler’s theorem:


```
x^φ(m) mod m = 1
```


So the numbers `x⁻¹` and `x^(φ(m)−1)` are equal.

**C++:**


```
int modInverse(int a, int m) {
    return modExpo(a, m - 2, m); // Using Fermat's little theorem
}
```


**Python:**


```
def mod_inverse(a, m):
    return mod_exp(a, m - 2, m)
```


**JavaScript:**


```
function modInverse(a, m) {
    return modExp(a, m - 2, m);
}
```



---
# Questions to practice:  

##  Easy

1. [Find Greatest Common Divisor of Array – LeetCode](https://leetcode.com/problems/find-greatest-common-divisor-of-array/)
2. [LCM And GCD – GeeksforGeeks](https://www.geeksforgeeks.org/problems/lcm-and-gcd/1)
3. [Check if a Number is Prime – GeeksforGeeks](https://www.geeksforgeeks.org/problems/check-for-prime/1)
4. [Count Numbers Having Exactly 3 Divisors – LeetCode](https://leetcode.com/problems/three-divisors/)
5. [Print All Divisors of a Number – GeeksforGeeks](https://www.geeksforgeeks.org/problems/all-divisors-of-a-number/1)

---

##  Medium

1. [Sieve of Eratosthenes – GeeksforGeeks](https://www.geeksforgeeks.org/problems/sieve-of-eratosthenes5242/1)
2. [Sum of All Primes in a Range – GeeksforGeeks](https://www.geeksforgeeks.org/sum-of-all-primes-in-a-given-range-using-sieve-of-eratosthenes/)
3. [Modular Exponentiation – LeetCode](https://leetcode.com/problems/powx-n/)
4. [Modular Multiplicative Inverse – GeeksforGeeks](https://www.geeksforgeeks.org/multiplicative-inverse-under-modulo-m/)

---

##  Hard

1. [913A – Remainder by Division by Powers of Two – Codeforces](https://codeforces.com/problemset/problem/913/A)
2. [1285C – Fadi and LCM – Codeforces](https://codeforces.com/problemset/problem/1285/C)
3. [Totient Function – GeeksforGeeks](https://www.geeksforgeeks.org/problems/totient-function/1)

---
