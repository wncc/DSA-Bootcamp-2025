# **String Algorithms**


## **1. Introduction to String Algorithms**

Many string problems can be easily solved in O(n 2 ) time, but the challenge is to find algorithms that work in O(n) or O(nlogn) time. For example, a fundamental string processing problem is the pattern matching problem: given a string of length n and a pattern of length m, our task is to find the occurrences of the pattern in the string. For example, the pattern ABC occurs two times in the string ABABCBABC. The pattern matching problem can be easily solved in O(nm) time by a brute force algorithm that tests all positions where the pattern may occur in the string. However, in this chapter, we will see that there are more efficient algorithms that require only O(n+ m) time. 


---


## **2. Basic String Operations**

Before diving into advanced algorithms, ensure familiarity with:



* Comparing strings 

* Reversing strings 

* Substring extraction 

* Prefix and suffix handling 


These can be done using built-in functions in C++, Python, or JavaScript.


---

# **KMP Algorithm (Knuth–Morris–Pratt)**

The KMP algorithm efficiently finds all occurrences of a pattern in a string in **O(n + m)** time, where `n` is the length of the text and `m` is the length of the pattern. It avoids unnecessary rechecking of characters after a mismatch by using preprocessed information from the pattern.

What we want to do here is that given a text string `T` of length `n` and a pattern string `P` of length `m`, we have to find all positions where `P` occurs as a substring in `T`.

The idea is that naive pattern matching algorithm may take `O(nm)` time in the worst case. The KMP algorithm improves this by preprocessing the pattern to create an array called the **longest prefix which is also suffix** (LPS) array, which helps skip characters without checking them again.


---


### **The LPS Array**

The **LPS array** is the foundation of the **KMP algorithm**. It helps us determine how much of the pattern we can reuse when a mismatch happens.


---


### 
    **💡 Definition**

For a given index `i`, `lps[i]` is defined as the **length of the longest proper prefix** of the substring `pattern[0..i]` that is also a **suffix** of this substring.



* A **proper prefix** is a prefix that is **not equal to the entire string**. \

* A **suffix** is a substring that occurs at the end.

So, for `pattern = "ABABAC"`:

| Index `i` | Substring  | Proper Prefixes         | Proper Suffixes         | LPS Value |
|-----------|------------|--------------------------|--------------------------|-----------|
| 0         | A          | —                        | —                        | 0         |
| 1         | AB         | A                        | B                        | 0         |
| 2         | ABA        | A, AB                    | A, BA                    | 1         |
| 3         | ABAB       | A, AB, ABA               | B, AB, BAB               | 2         |
| 4         | ABABA      | A, AB, ABA, ABAB         | A, BA, ABA, BABA         | 3         |
| 5         | ABABAC     | A, AB, ABA, ABAB, ABABA  | C, AC, BAC, ABAC, BABAC  | 0         |

**Final LPS Array**: `[0, 0, 1, 2, 3, 0]`


Final LPS: `[0, 0, 1, 2, 3, 0]`


### How to Compute the LPS Array:


### **C++:**

Start with:

- lps[0] = 0

- len = 0 (length of current longest prefix-suffix)

- i = 1 (we start checking from second character)

Loop while i &lt; pattern.length:

    If pattern[i] == pattern[len]:

        len += 1

        lps[i] = len

        i += 1

    Else if len != 0:

        len = lps[len - 1]   // Backtrack to previous best match

    Else:

        lps[i] = 0

        i += 1

This avoids unnecessary comparisons and computes LPS in `O(m)` time. \



---


### **Algorithm Steps**



1. **Build the LPS array** for the pattern in `O(m)` time. \

2. Use two pointers, `i` for text and `j` for pattern. \

3. If characters match, increment both `i` and `j`. \

4. If `j` reaches the end of the pattern, we have a match. Store `i - j` and update `j = lps[j - 1]`. \

5. If a mismatch occurs: \

    * If `j > 0`, set `j = lps[j - 1] \
`
    * Otherwise, increment `i` only. \



---


### Implementation:

Cpp:


```
vector<int> computeLPS(string pattern) {
    int m = pattern.size();
    vector<int> lps(m, 0);
    int len = 0;
    for (int i = 1; i < m;) {
        if (pattern[i] == pattern[len]) {
            lps[i++] = ++len;
        } else if (len != 0) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return lps;
}

vector<int> kmpSearch(string text, string pattern) {
    vector<int> lps = computeLPS(pattern);
    vector<int> result;
    int i = 0, j = 0;
    while (i < text.size()) {
        if (text[i] == pattern[j]) {
            i++; j++;
        }
        if (j == pattern.size()) {
            result.push_back(i - j);
            j = lps[j - 1];
        } else if (i < text.size() && text[i] != pattern[j]) {
            j = (j != 0) ? lps[j - 1] : 0;
            if (j == 0) i++;
        }
    }
    return result;
}
```


Python:


```
def compute_lps(pattern):
    lps = [0] * len(pattern)
    length = 0
    i = 1
    while i < len(pattern):
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        elif length != 0:
            length = lps[length - 1]
        else:
            lps[i] = 0
            i += 1
    return lps

def kmp_search(text, pattern):
    lps = compute_lps(pattern)
    i = j = 0
    result = []
    while i < len(text):
        if text[i] == pattern[j]:
```


JavaScript:


```
function computeLPS(pattern) {
    const lps = Array(pattern.length).fill(0);
    let len = 0, i = 1;
    while (i < pattern.length) {
        if (pattern[i] === pattern[len]) {
            len++;
            lps[i++] = len;
        } else if (len !== 0) {
            len = lps[len - 1];
        } else {
            lps[i++] = 0;
        }
    }
    return lps;
}

function kmpSearch(text, pattern) {
    const lps = computeLPS(pattern);
    const result = [];
    let i = 0, j = 0;

    while (i < text.length) {
        if (text[i] === pattern[j]) {
            i++; j++;
        }
        if (j === pattern.length) {
            result.push(i - j);
            j = lps[j - 1];
        } else if (i < text.length && text[i] !== pattern[j]) {
            j = j !== 0 ? lps[j - 1] : 0;
            if (j === 0) i++;
        }
    }
    return
```



### **Time Complexity**



* LPS Array Construction: `O(m) \
`
* Pattern Search: `O(n) \
`
* Total: `O(n + m)`
## **5. Z Algorithm**

The Z-array z of a string s of length n contains for each k = 0,1,...,n − 1 the length of the longest substring of s that begins at position k and is a prefix of 247 s. Thus, z[k] = p tells us that s[0... p − 1] equals s[k...k + p − 1]. Many string processing problems can be efficiently solved using the Z-array. For example, the Z-array of ACBACDACBACBACDA is as follows:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |


In this case, for example, z[6] = 5, because the substring ACBAC of length 5 is a prefix of s, but the substring ACBACB of length 6 is not a prefix of s.

Algorithm description Next we describe an algorithm, called the Z-algorithm2 , that efficiently constructs the Z-array in O(n) time. The algorithm calculates the Z-array values from left to right by both using information already stored in the Z-array and comparing substrings character by character. To efficiently calculate the Z-array values, the algorithm maintains a range [x, y] such that s[x... y] is a prefix of s and y is as large as possible. Since we know that s[0... y− x] and s[x... y] are equal, we can use this information when calculating Z-values for positions x+1, x+2,..., y. At each position k, we first check the value of z[k − x]. If k +z[k − x] &lt; y, we know that z[k] = z[k − x]. However, if k +z[k − x] ≥ y, s[0... y− k] equals s[k... y], and to determine the value of z[k] we need to compare the substrings character by character. Still, the algorithm works in O(n) time, because we start comparing at positions y− k +1 and y+1. For example, let us construct the following Z-array: 

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 |   |   |   |   |   |   |   |   |   |
| [x, y]    |                       | 6 → 10                         |


After calculating the value z[6] = 5, the current [x, y] range is [6,10]:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 |   |   |   |   |   |   |   |


Now we can calculate subsequent Z-array values efficiently, because we know that s[0...4] and s[6...10] are equal. First, since z[1] = z[2] = 0, we immediately know that also z[7] = z[8] = 0: 

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 |   |   |   |   |   |   |


Then, since z[3] = 2, we know that z[9] ≥ 2:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 |   |   |   |   |   |   |
|           |                             ↑             ↑                   |
| matched   |       A C B A C   =   A C B A C                               |


However, we have no information about the string after position 10, so we need to compare the substrings character by character: 

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 | 7 |   |   |   |   |   |
| [x, y]    |                                   | 9 → 15                   |


It turns out that z[9] = 7, so the new [x, y] range is [9,15]:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 | 7 |   |   |   |   |   |


After this, all the remaining Z-array values can be determined by using the information already stored in the Z-array:

| i         | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 |10 |11 |12 |13 |14 |15 |
|-----------|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|---|
| s[i]      | A | C | B | A | C | D | A | C | B | A | C | B | A | C | D | A |
| z[i]      |   |   |   |   |   |   | 5 | 0 | 0 | 2 | 7 | 0 | 0 | 0 | 0 | 0 |


Using the Z-array It is often a matter of taste whether to use string hashing or the Z-algorithm. Unlike hashing, the Z-algorithm always works and there is no risk for collisions. On the other hand, the Z-algorithm is more difficult to implement and some problems can only be solved using hashing. As an example, consider again the pattern matching problem, where our task is to find the occurrences of a pattern p in a string s. We already solved this problem efficiently using string hashing, but the Z-algorithm provides another way to solve the problem. A usual idea in string processing is to construct a string that consists of multiple strings separated by special characters. In this problem, we can construct a string p#s, where p and s are separated by a special character # that does not occur in the strings. The Z-array of p#s tells us the positions where p occurs in s, because such positions contain the length of p. For example, if s =HATTIVATTI and p =ATT, the Z-array is as follows: A T T # H A T T I V A T T I – 0 0 0 0 3 0 0 0 0 3 0 0 0 0 1 2 3 4 5 6 7 8 9 10 11 12 13 The positions 5 and 10 contain the value 3, which means that the pattern ATT occurs in the corresponding positions of HATTIVATTI. The time complexity of the resulting algorithm is linear, because it suffices to construct the Z-array and go through its values. 


### **Implementation**

 Here is a short implementation of the Z-algorithm that returns a vector that corresponds to the Z-array.

**C++:**


```
vector<int> z_array(const string& s) {
    int n = s.size();
    vector<int> z(n);
    int x = 0, y = 0;

    for (int i = 1; i < n; i++) {
        if (i <= y)
            z[i] = min(z[i - x], y - i + 1);
        while (i + z[i] < n && s[z[i]] == s[i + z[i]]) {
            x = i;
            y = i + z[i];
            z[i]++;
        }
    }

    return z;
}
```


**Python:**


```
def z_array(s):
    n = len(s)
    z = [0] * n
    x = y = 0
    for i in range(1, n):
        if i <= y:
            z[i] = min(z[i - x], y - i + 1)
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1
            x = i
            y = i + z[i] - 1
    return z
```


**Javascript;**


```
function zArray(s) {
    const n = s.length;
    const z = Array(n).fill(0);
    let x = 0, y = 0;

    for (let i = 1; i < n; i++) {
        if (i <= y) {
            z[i] = Math.min(z[i - x], y - i + 1);
        }
        while (i + z[i] < n && s[z[i]] === s[i + z[i]]) {
            z[i]++;
            x = i;
            y = i + z[i] - 1;
        }
    }
    return z;
}
```



---


## **6. Trie (Prefix Tree)**

A trie is a rooted tree that maintains a set of strings. Each string in the set is stored as a chain of characters that starts at the root. If two strings have a common prefix, they also have a common chain in the tree. For example, consider the following trie: 

Pic

This trie corresponds to the set {CANAL,CANDY,THE,THERE}. The character * in a node means that a string in the set ends at the node. Such a character is needed, because a string may be a prefix of another string. For example, in the above trie, THE is a prefix of THERE. We can check in O(n) time whether a trie contains a string of length n, because we can follow the chain that starts at the root node. We can also add a string of length n to the trie in O(n) time by first following the chain and then adding new nodes to the trie if necessary. Using a trie, we can find the longest prefix of a given string such that the prefix belongs to the set. Moreover, by storing additional information in each node, we can calculate the number of strings that belong to the set and have a given string as a prefix. A trie can be stored in an array `int trie[N][A];`

where N is the maximum number of nodes (the maximum total length of the strings in the set) and A is the size of the alphabet. The nodes of a trie are numbered 0,1,2,... so that the number of the root is 0, and trie[s][c] is the next node in the chain when we move from node s using character c.

**C++ implementation:**


```
#include <iostream>
#include <unordered_map>
using namespace std;

struct TrieNode {
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord = false;
};

class Trie {
public:
    TrieNode* root;

    Trie() {
        root = new TrieNode();
    }

    void insert(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch))
                node->children[ch] = new TrieNode();
            node = node->children[ch];
        }
        node->isEndOfWord = true;
    }

    bool search(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children.count(ch))
                return false;
            node = node->children[ch];
        }
        return node->isEndOfWord;
    }
};
```


**Python implementation:**


```
class TrieNode:
    def __init__(self):
        self.children = {}
        self.is_end_of_word = False

class Trie:
    def __init__(self):
        self.root = TrieNode()

    def insert(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                node.children[ch] = TrieNode()
            node = node.children[ch]
        node.is_end_of_word = True

    def search(self, word):
        node = self.root
        for ch in word:
            if ch not in node.children:
                return False
            node = node.children[ch]
        return node.is_end_of_word
```


**JavaScript Implementation:**


```
class TrieNode {
    constructor() {
        this.children = {};
        this.isEndOfWord = false;
    }
}

class Trie {
    constructor() {
        this.root = new TrieNode();
    }

    insert(word) {
        let node = this.root;
        for (let ch of word) {
            if (!node.children[ch])
                node.children[ch] = new TrieNode();
            node = node.children[ch];
        }
        node.isEndOfWord = true;
    }

    search(word) {
        let node = this.root;
        for (let ch of word) {
            if (!node.children[ch])
                return false;
            node = node.children[ch];
        }
        return node.isEndOfWord;
    }
}

```
# Practice Problems

## Easy
1. [strStr (First Occurrence)](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/)
2. [String Matching in an Array](https://leetcode.com/problems/string-matching-in-an-array/)
3. [Naive Pattern Searching (GfG)](https://www.geeksforgeeks.org/naive-algorithm-for-pattern-searching/)
4. [Repeated Substring Pattern](https://leetcode.com/problems/repeated-substring-pattern/)
5. [Remove All Occurrences of a Substring](https://leetcode.com/problems/remove-all-occurrences-of-a-substring/)

## Medium
1. [Search Pattern (KMP) – GfG](https://www.geeksforgeeks.org/problems/search-pattern0205/1)
2. [Z‑Algorithm Pattern Search – GfG](https://www.geeksforgeeks.org/dsa/z-algorithm-linear-time-pattern-searching-algorithm/)
3. [Longest Happy Prefix](https://leetcode.com/problems/longest-happy-prefix/)
4. [Implement Trie (Prefix Tree)](https://leetcode.com/problem-list/trie/)

## Hard
1. [Shortest Palindrome](https://leetcode.com/problems/shortest-palindrome/)
2. [Stamping the Sequence](https://leetcode.com/problems/stamping-the-sequence/)
3. [Repeated String Match](https://leetcode.com/problems/repeated-string-match/)
