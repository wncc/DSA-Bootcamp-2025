# Heaps 

A Heap is a complete binary tree data structure that satisfies the heap property: for every node, the value of its children is greater than or equal to its own value. Heaps are usually used to implement priority queues, where the smallest (or largest) element is always at the root of the tree.

A Binary Heap is a complete binary tree that stores data efficiently, allowing quick access to the maximum or minimum element, depending on the type of heap. It can either be a Min Heap or a Max Heap. In a Min Heap, the key at the root must be the smallest among all the keys in the heap, and this property must hold true recursively for all nodes. Similarly, a Max Heap follows the same principle, but with the largest key at the root.

### Priority Queue

A priority queue is a type of data structure where each element has a priority assigned to it. Elements are processed based on their priority, with higher priority elements being processed before lower priority ones. This is different from a regular queue where elements are processed in the order they arrive (first-in, first-out).

### Example
```cpp
#include <iostream>
#include <queue>
using namespace std;

int main() {
    
    // Creating a priority queue of integers
    priority_queue<int> pq;
    pq.push(9);
    pq.push(10);
    pq.push(6);
    
    cout << pq.top() << " ";
    return 0;
}
```
### Output
```
10
```
In the above program, we created a priority queue of integers that contains the elements 9, 10 and 6. The top element, which is the largest value in the queue, is then printed.

Refer to this site to understand it better:
https://www.geeksforgeeks.org/priority-queue-set-1-introduction/


## Properties
A heap has two fundamental properties:

1. **Complete Binary Tree**:  
A heap is always a complete binary tree. This means that all levels of the tree are fully filled, except possibly the last level, which is filled from left to right. This structure guarantees that the heap is balanced, ensuring logarithmic time complexity for insertion and deletion operations.

2. **Heap Order Property**:  
The heap must satisfy the heap order property, which depends on whether it is a max-heap or a min-heap.

- **Max-Heap**: In a max-heap, every parent node is greater than or equal to its children. This ensures that the largest element is always located at the root of the heap. For example, in a max-heap like `[50, 30, 40, 10, 20]`, the root (50) is greater than both its children (30 and 40).

- **Min-Heap**: In a min-heap, every parent node is less than or equal to its children. This ensures that the smallest element is always at the root. For instance, in a min-heap like `[10, 20, 15, 40, 50]`, the root (10) is smaller than both its children (20 and 15).


## Heap Representation (Array-Based)
A Binary Heap is a Complete Binary Tree, and it is typically implemented using an array.

- The root element is stored at index `arr[0]`.

The following index relationships apply for any node at index `i`:

- `arr[(i - 1) / 2]` → Returns the **parent node**
- `arr[(2 * i) + 1]` → Returns the **left child node**
- `arr[(2 * i) + 2]` → Returns the **right child node**

This array-based representation is space-efficient and avoids the overhead of pointers typically required for linked representations of trees.

# Heapify Operations

Heapify operations are crucial for maintaining the heap property after insertion or deletion. The two main heapify operations are **Up-Heapify (Bubble-Up)** and **Down-Heapify (Bubble-Down)**.

---

## 1. Up-Heapify (Bubble-Up)

**Up-Heapify** is used after inserting a new element to restore the heap property by moving the element up the tree.

### Steps:
1. Insert the new element at the end of the heap.
2. Compare the inserted element with its parent.
3. If the heap property is violated:
   - In a **max-heap**, swap if the new element is **greater** than the parent.
   - In a **min-heap**, swap if the new element is **smaller** than the parent.
4. Repeat until:
   - The heap property is restored, **or**
   - The element becomes the **root**.

### Example (Max-Heap):
Given heap: `[40, 30, 20]`

- Insert 50 → `[40, 30, 20, 50]`
- Compare 50 with parent 30 → swap → `[40, 50, 20, 30]`
- Compare 50 with parent 40 → swap → `[50, 40, 20, 30]`
- Heap property restored.

### Time Complexity:
- **O(log n)**

---

## 2. Down-Heapify (Bubble-Down)

**Down-Heapify** is used after deleting the root element to restore the heap property by moving the new root element down the tree.

### Steps:
1. Replace the root element with the last element in the heap.
2. Remove the last element from the heap.
3. Compare the new root with its children.
4. If the heap property is violated:
   - In a **max-heap**, swap with the **larger** child.
   - In a **min-heap**, swap with the **smaller** child.
5. Repeat until:
   - The heap property is restored, **or**
   - The element reaches a **leaf node**.

### Example (Max-Heap):
Given heap: `[50, 30, 40, 10, 20]`

- Delete root (50)
- Replace with last element: `[20, 30, 40, 10]`
- Compare 20 with 30 and 40 → swap with 40: `[40, 30, 20, 10]`
- Heap property is restored.

### Time Complexity:
- **O(log n)**



## 1. Insertion into Heap
### Steps:
1. Insert the element at the end of the heap.
2. **Heapify Up**: Compare the inserted node with its parent and swap if the heap property is violated. Repeat until the root or heap property is restored.

### Time Complexity:
- **O(log n)**

### Example (Min-Heap):
```cpp
#include <iostream>
#include <queue>
#include <vector>

int main() {
    std::priority_queue<int, std::vector<int>, std::greater<int>> heap;

    heap.push(5);
    heap.push(2);
    heap.push(8);

    // Print heap elements (not in heap order since priority_queue doesn't expose internal structure)
    std::vector<int> output;
    while (!heap.empty()) {
        output.push_back(heap.top());
        heap.pop();
    }

    for (int val : output) {
        std::cout << val << " ";
    }
    std::cout << std::endl;

    return 0;
}
```

---

## 2. Deletion from Heap (usually root)
### Steps:
1. Replace the root with the last element in the heap.
2. Remove the last element.
3. **Heapify Down**: Compare the new root with its children and swap with the smaller (min-heap) or larger (max-heap) child if needed. Repeat until heap property is restored.

### Time Complexity:
- **O(log n)**

### Python Example:
```cpp
#include <iostream>
#include <queue>
#include <vector>

int main() {
    std::vector<int> elements = {2, 5, 8};
    std::priority_queue<int, std::vector<int>, std::greater<int>> heap(elements.begin(), elements.end());

    int min_val = heap.top();
    heap.pop();

    std::cout << min_val << std::endl; // 2

    std::vector<int> remaining;
    while (!heap.empty()) {
        remaining.push_back(heap.top());
        heap.pop();
    }

    for (int val : remaining) {
        std::cout << val << " ";
    }
    std::cout << std::endl; // 5 8

    return 0;
}
```

## Heap Representation
- In C++
```cpp
#include <iostream>
#include <vector>
#include <stdexcept>

class MaxHeap {
public:
    void insert(int val) {
        heap.push_back(val);
        heapifyUp(heap.size() - 1);
    }

    int deleteMax() {
        if (heap.empty()) {
            throw std::out_of_range("Heap is empty");
        }
        int maxVal = heap.front();
        heap.front() = heap.back();
        heap.pop_back();
        heapifyDown(0);
        return maxVal;
    }

private:
    std::vector<int> heap;

    void heapifyUp(int index) {
        int parent = (index - 1) / 2;
        if (index > 0 && heap[index] > heap[parent]) {
            std::swap(heap[index], heap[parent]);
            heapifyUp(parent);
        }
    }

    void heapifyDown(int index) {
        int largest = index;
        int left = 2 * index + 1;
        int right = 2 * index + 2;

        if (left < heap.size() && heap[left] > heap[largest]) {
            largest = left;
        }
        if (right < heap.size() && heap[right] > heap[largest]) {
            largest = right;
        }
        if (largest != index) {
            std::swap(heap[index], heap[largest]);
            heapifyDown(largest);
        }
    }
};

int main() {
    MaxHeap heap;
    heap.insert(10);
    heap.insert(20);
    heap.insert(5);
    for (int val : heap.heap) {
        std::cout << val << " "; // Output: 20 10 5
    }
    std::cout << std::endl;
    std::cout << heap.deleteMax() << std::endl; // Output: 20
    for (int val : heap.heap) {
        std::cout << val << " "; // Output: 10 5
    }
    std::cout << std::endl;
    return 0;
}
```

## Applications of Heaps in DSA

### 1. Priority Queues
Heaps are commonly used to implement **priority queues**, which allow efficient retrieval and removal of the highest (or lowest) priority element.

### Examples:
- **Task Scheduling**: Managing tasks with different priorities in an operating system.
- **Event Simulation**: Handling events in a simulation environment where events occur at different times.

---

### 2. Heap Sort
**Heap sort** is a comparison-based sorting algorithm that uses a heap to sort elements.

### Time Complexity:
- **O(n log n)**

### Steps:
1. Build a max-heap from the input data.
2. Swap the root (largest element) with the last element.
3. Reduce the heap size and heapify the root element.
4. Repeat until the heap is empty.

### Example:
- **Sorting an array using heap sort**.

---

### 3. Graph Algorithms
Heaps improve the efficiency of various graph algorithms, especially for shortest paths and minimum spanning trees.

### Examples:
- **Dijkstra's Algorithm**: Uses a min-heap to find the shortest path from a source vertex to all others in a weighted graph.
- **Prim's Algorithm**: Uses a min-heap to construct a minimum spanning tree with the smallest possible total edge weight.

---

# Tries

A **Trie** (pronounced "try"), also known as a **Prefix Tree** or **Digital Tree**, is a specialized tree data structure used to efficiently store and retrieve strings, especially when working with dictionaries and autocomplete features. Tries are commonly used when dealing with problems involving prefix matching, spell checking, and word searches.

A trie stores characters of strings in a tree-like structure where each node represents a character of a string. A path from the root to a leaf node represents a complete word.

## Properties

A Trie has the following core properties:

1. **Root Node**:  
   The root node is empty and does not contain any character.
   
2. **Children**:  
   Each node contains a map (or array) of child nodes representing subsequent characters.

3. **End of Word Marker**:  
   A boolean flag to indicate whether a node represents the end of a valid word.

4. **Time Complexities**:  
   - Insertion: O(L)  
   - Search: O(L)  
   - Deletion: O(L)  
   Where **L** is the length of the word.

---

## Trie Operations

### 1. Insertion
To insert a word:
- Start from the root.
- For each character in the word, check if a corresponding child node exists.
- If not, create a new node.
- Move to the child node.
- After inserting the last character, mark the node as the end of the word.

### 2. Search
To search for a word:
- Start from the root and check each character.
- If the path exists and ends with an end-of-word marker, return true.
- Otherwise, return false.

### 3. Prefix Matching
- Similar to search, but we do not require the end-of-word marker.
- Useful for autocomplete features.

### 4. Deletion
- Search for the word.
- If found, unmark the end-of-word flag.
- Optionally, remove nodes that are no longer part of any other word (clean-up step).

---

## Trie Representation (Code Example in C++)

```cpp
#include <iostream>
#include <unordered_map>
using namespace std;

class TrieNode {
public:
    unordered_map<char, TrieNode*> children;
    bool isEndOfWord;

    TrieNode() {
        isEndOfWord = false;
    }
};

class Trie {
    TrieNode* root;
public:
    Trie() {
        root = new TrieNode();
    }

    void insert(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children[ch])
                node->children[ch] = new TrieNode();
            node = node->children[ch];
        }
        node->isEndOfWord = true;
    }

    bool search(string word) {
        TrieNode* node = root;
        for (char ch : word) {
            if (!node->children[ch]) return false;
            node = node->children[ch];
        }
        return node->isEndOfWord;
    }

    bool startsWith(string prefix) {
        TrieNode* node = root;
        for (char ch : prefix) {
            if (!node->children[ch]) return false;
            node = node->children[ch];
        }
        return true;
    }
};

int main() {
    Trie trie;
    trie.insert("apple");
    cout << trie.search("apple") << endl;   // true
    cout << trie.search("app") << endl;     // false
    cout << trie.startsWith("app") << endl; // true
    trie.insert("app");
    cout << trie.search("app") << endl;     // true
    return 0;
}
```
## Applications
1. Dictionary Word Storage
Efficient storage and lookup of words in dictionary-based problems.

2. Autocomplete Systems
Providing real-time suggestions by matching typed prefixes with stored words.

3. Spell Checker
Identifying whether a word is valid and suggesting possible alternatives.

# Heap & Trie Problems

### Easy
- [Kth Largest Element in a Stream](https://leetcode.com/problems/kth-largest-element-in-a-stream/) | [Solution](https://www.geeksforgeeks.org/kth-largest-element-in-a-stream/)
- [Last Stone Weight](https://leetcode.com/problems/last-stone-weight/) | [Solution](https://www.geeksforgeeks.org/last-stone-weight/)
- [Implement Trie (Prefix Tree)](https://leetcode.com/problems/implement-trie-prefix-tree/) | [Solution](https://www.geeksforgeeks.org/trie-insert-and-search/)
- [Longest Common Prefix](https://leetcode.com/problems/longest-common-prefix/) | [Solution](https://www.geeksforgeeks.org/longest-common-prefix-using-trie/)

  
### Medium
- [Top K Frequent Elements](https://leetcode.com/problems/top-k-frequent-elements/) | [Solution](https://www.geeksforgeeks.org/find-k-numbers-occurrences-given-array/)
- [Merge k Sorted Lists](https://leetcode.com/problems/merge-k-sorted-lists/) | [Solution](https://www.geeksforgeeks.org/merge-k-sorted-arrays/)
- [Add and Search Word](https://leetcode.com/problems/add-and-search-word-data-structure-design/) | [Solution](https://www.geeksforgeeks.org/trie-insert-and-search/#wildcard-search)
- [Word Search II](https://leetcode.com/problems/word-search-ii/) | [Solution](https://algowalkers.medium.com/boggle-find-possible-words-in-a-board-of-characters-using-trie-83d6b42a7916)


### Hard
- [Sliding Window Maximum](https://leetcode.com/problems/sliding-window-maximum/) | [Solution](https://www.geeksforgeeks.org/sliding-window-maximum-maximum-of-all-subarrays-of-size-k/)
- [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | [Solution](https://algowalkers.medium.com/the-skyline-problem-4f6df8fea5bb)
- [Maximum XOR of Two Numbers](https://leetcode.com/problems/maximum-xor-of-two-numbers-in-an-array/) | [Solution](https://www.geeksforgeeks.org/maximum-xor-of-two-numbers-in-an-array/)
- [Concatenated Words](https://leetcode.com/problems/concatenated-words/) | [Solution](https://algowalkers.medium.com/concatenated-words-trie-dfs-solution-10f073fc9f9b)


