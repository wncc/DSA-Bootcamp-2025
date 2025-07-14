# Graphs - Complete Guide

## What is a Graph?
A **graph** is a non-linear data structure that consists of a set of **vertices** (nodes) and a set of **edges** connecting pairs of vertices.

---

## Components of Graph
- **Vertices (Nodes)**: Fundamental units of a graph.
- **Edges (Links)**: Connections between vertices.

---

## Types of Graphs

1. **Null Graph**: No edges.
2. **Trivial Graph**: One vertex, no edges.
3. **Undirected Graph**: Edges have no direction.
4. **Directed Graph (Digraph)**: Edges have direction.
5. **Connected Graph**: All vertices are reachable from any other vertex.
6. **Disconnected Graph**: At least one vertex is unreachable.
7. **Regular Graph**: All vertices have the same degree `k`.
8. **Complete Graph**: Every vertex connected to every other vertex.
9. **Cycle Graph**: All vertices form a cycle.
10. **Cyclic Graph**: Contains at least one cycle.
11. **Directed Acyclic Graph (DAG)**: Directed with no cycles.
12. **Bipartite Graph**: Vertices can be divided into two sets with no internal edges.
13. **Weighted Graph**: Edges have weights. Can be directed or undirected.

---

## Graph Representations

### 1. Adjacency Matrix
- 2D Matrix
- `mat[i][j] = 1` if there is an edge from vertex i to j

#### Python
```python
# Adjacency Matrix in Python
def add_edge(mat, i, j):
    mat[i][j] = 1
    mat[j][i] = 1  # Undirected

def display_matrix(mat):
    for row in mat:
        print(" ".join(map(str, row)))

V = 4
mat = [[0]*V for _ in range(V)]
add_edge(mat, 0, 1)
add_edge(mat, 0, 2)
add_edge(mat, 1, 2)
add_edge(mat, 2, 3)
print("Adjacency Matrix:")
display_matrix(mat)
```

#### C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

void addEdge(vector<vector<int>> &mat, int i, int j) {
    mat[i][j] = 1;
    mat[j][i] = 1;
}

void displayMatrix(const vector<vector<int>> &mat) {
    for (auto row : mat) {
        for (int val : row) cout << val << " ";
        cout << endl;
    }
}

int main() {
    int V = 4;
    vector<vector<int>> mat(V, vector<int>(V, 0));
    addEdge(mat, 0, 1);
    addEdge(mat, 0, 2);
    addEdge(mat, 1, 2);
    addEdge(mat, 2, 3);
    cout << "Adjacency Matrix Representation" << endl;
    displayMatrix(mat);
    return 0;
}
```

#### JavaScript
```javascript
function addEdge(mat, i, j) {
    mat[i][j] = 1;
    mat[j][i] = 1;
}

function displayMatrix(mat) {
    mat.forEach(row => console.log(row.join(" ")));
}

const V = 4;
let mat = Array.from({ length: V }, () => Array(V).fill(0));
addEdge(mat, 0, 1);
addEdge(mat, 0, 2);
addEdge(mat, 1, 2);
addEdge(mat, 2, 3);
console.log("Adjacency Matrix:");
displayMatrix(mat);
```

---

### 2. Adjacency List
- Each vertex stores a list of connected vertices

#### Python
```python
def add_edge(adj, i, j):
    adj[i].append(j)
    adj[j].append(i)

def display_adj_list(adj):
    for i in range(len(adj)):
        print(f"{i}:", *adj[i])

V = 4
adj = [[] for _ in range(V)]
add_edge(adj, 0, 1)
add_edge(adj, 0, 2)
add_edge(adj, 1, 2)
add_edge(adj, 2, 3)
print("Adjacency List Representation:")
display_adj_list(adj)
```

#### C++
```cpp
#include <iostream>
#include <vector>
using namespace std;

void addEdge(vector<vector<int>>& adj, int i, int j) {
    adj[i].push_back(j);
    adj[j].push_back(i);
}

void displayAdjList(const vector<vector<int>>& adj) {
    for (int i = 0; i < adj.size(); i++) {
        cout << i << ": ";
        for (int j : adj[i]) cout << j << " ";
        cout << endl;
    }
}

int main() {
    int V = 4;
    vector<vector<int>> adj(V);
    addEdge(adj, 0, 1);
    addEdge(adj, 0, 2);
    addEdge(adj, 1, 2);
    addEdge(adj, 2, 3);
    cout << "Adjacency List Representation:" << endl;
    displayAdjList(adj);
    return 0;
}
```

#### JavaScript
```javascript
function addEdge(adj, i, j) {
    adj[i].push(j);
    adj[j].push(i);
}

function displayAdjList(adj) {
    adj.forEach((list, i) => console.log(`${i}: ${list.join(" ")}`));
}

const V = 4;
const adj = Array.from({ length: V }, () => []);
addEdge(adj, 0, 1);
addEdge(adj, 0, 2);
addEdge(adj, 1, 2);
addEdge(adj, 2, 3);
console.log("Adjacency List Representation:");
displayAdjList(adj);
```

---

| Operation            | Adjacency Matrix | Adjacency List |
|---------------------|------------------|----------------|
| Adding Edge         | O(1)             | O(1)           |
| Removing Edge       | O(1)             | O(N)           |
| Initialization      | O(N*N)           | O(N)           |

---

## Graph Traversals

### Depth First Search (DFS)
#### Python
```python
def dfsRec(adj, visited, s, res):
    visited[s] = True
    res.append(s)
    for i in range(len(adj)):
        if adj[s][i] == 1 and not visited[i]:
            dfsRec(adj, visited, i, res)

def DFS(adj):
    visited = [False] * len(adj)
    res = []
    dfsRec(adj, visited, 0, res)
    return res
```

#### C++
```cpp
void dfsRec(vector<vector<int>> &adj, vector<bool> &visited, int s, vector<int> &res) {
    visited[s] = true;
    res.push_back(s);
    for (int i : adj[s])
        if (!visited[i]) dfsRec(adj, visited, i, res);
}

vector<int> DFS(vector<vector<int>> &adj) {
    vector<bool> visited(adj.size(), false);
    vector<int> res;
    dfsRec(adj, visited, 0, res);
    return res;
}
```

#### JavaScript
```javascript
function dfsRec(adj, visited, s, res) {
    visited[s] = true;
    res.push(s);
    for (let i = 0; i < adj.length; i++) {
        if (adj[s][i] === 1 && !visited[i]) dfsRec(adj, visited, i, res);
    }
}

function DFS(adj) {
    const visited = new Array(adj.length).fill(false);
    const res = [];
    dfsRec(adj, visited, 0, res);
    return res;
}
```

---

### Breadth First Search (BFS)
#### Python
```python
from collections import deque

def bfs(adj):
    V = len(adj)
    visited = [False] * V
    res = []
    q = deque([0])
    visited[0] = True
    while q:
        curr = q.popleft()
        res.append(curr)
        for x in adj[curr]:
            if not visited[x]:
                visited[x] = True
                q.append(x)
    return res
```

#### C++
```cpp
vector<int> bfs(vector<vector<int>>& adj) {
    int V = adj.size();
    vector<bool> visited(V, false);
    vector<int> res;
    queue<int> q;
    q.push(0);
    visited[0] = true;
    while (!q.empty()) {
        int curr = q.front();
        q.pop();
        res.push_back(curr);
        for (int x : adj[curr]) {
            if (!visited[x]) {
                visited[x] = true;
                q.push(x);
            }
        }
    }
    return res;
}
```

#### JavaScript
```javascript
function bfs(adj) {
    let V = adj.length;
    let visited = new Array(V).fill(false);
    let res = [], q = [0];
    visited[0] = true;
    while (q.length > 0) {
        let curr = q.shift();
        res.push(curr);
        for (let x of adj[curr]) {
            if (!visited[x]) {
                visited[x] = true;
                q.push(x);
            }
        }
    }
    return res;
}
```

---

## Disconnected Graph Traversal
- Perform DFS/BFS from **every unvisited vertex**
## BFS for Disconnected Graph

### 🔹 Python

```python
from collections import deque

def bfsOfGraph(adj, s, visited, res):
    q = deque()
    visited[s] = True
    q.append(s)
    
    while q:
        curr = q.popleft()
        res.append(curr)
        for x in adj[curr]:
            if not visited[x]:
                visited[x] = True
                q.append(x)
    return res

def bfsDisconnected(adj):
    V = len(adj)
    res = []
    visited = [False] * V
    for i in range(V):
        if not visited[i]:
            bfsOfGraph(adj, i, visited, res)
    return res

if __name__ == "__main__":
    adj = [[1, 2], [0], [0], [4], [3, 5], [4]]
    ans = bfsDisconnected(adj)
    for i in ans:
        print(i, end=" ")
```

---

### 🔹 C++

```cpp
#include <bits/stdc++.h>
using namespace std;

void bfs(vector<vector<int>>& adj, int s, vector<bool>& visited, vector<int>& res) {
    queue<int> q;
    visited[s] = true;
    q.push(s);
    
    while (!q.empty()) {
        int curr = q.front();
        q.pop();
        res.push_back(curr);
        for (int x : adj[curr]) {
            if (!visited[x]) {
                visited[x] = true;
                q.push(x);
            }
        }
    }
}

vector<int> bfsDisconnected(vector<vector<int>>& adj) {
    int V = adj.size();
    vector<int> res;
    vector<bool> visited(V, false);
    
    for (int i = 0; i < V; ++i) {
        if (!visited[i]) {
            bfs(adj, i, visited, res);
        }
    }
    return res;
}

int main() {
    vector<vector<int>> adj = {{1, 2}, {0}, {0}, {4}, {3, 5}, {4}};
    vector<int> ans = bfsDisconnected(adj);
    for (auto i : ans) {
        cout << i << " ";
    }
    return 0;
}
```

---

### 🔹 JavaScript

```javascript
function bfsOfGraph(adj, s, visited, res) {
    let q = [];
    visited[s] = true;
    q.push(s);
    
    while (q.length > 0) {
        let curr = q.shift();
        res.push(curr);
        for (let x of adj[curr]) {
            if (!visited[x]) {
                visited[x] = true;
                q.push(x);
            }
        }
    }
    return res;
}

function bfsDisconnected(adj) {
    let V = adj.length;
    let res = [];
    let visited = new Array(V).fill(false);
    
    for (let i = 0; i < V; i++) {
        if (!visited[i]) {
            bfsOfGraph(adj, i, visited, res);
        }
    }
    return res;
}

let adj = [[1, 2], [0], [0], [4], [3, 5], [4]];
let ans = bfsDisconnected(adj);
for (let i of ans) {
    process.stdout.write(i + " ");
}
```

This document provides a complete guide to basic graph theory, representation methods, and traversal techniques implemented in Python, C++, and JavaScript for better clarity and GitHub documentation.
