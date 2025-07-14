# 📘 Graphs - Complete Guide

## 📌 What is a Graph?
A **graph** is a non-linear data structure consisting of:
- **Vertices (nodes)**: Represent entities.
- **Edges (connections)**: Represent relationships.

---

## 🧩 Components of a Graph

- **Vertices**: Nodes (can be labeled or unlabelled)
- **Edges**: Connect pairs of vertices (can be directed/undirected, weighted/unweighted)

---

## 🧠 Types of Graphs

| Type                | Description |
|---------------------|-------------|
| Null Graph          | No edges |
| Trivial Graph       | Only 1 vertex |
| Undirected Graph    | No direction to edges |
| Directed Graph      | Direction on edges |
| Connected Graph     | Path exists between every pair of vertices |
| Disconnected Graph  | At least one pair of nodes is unreachable |
| Regular Graph       | Each vertex has same degree |
| Complete Graph      | Each vertex is connected to every other |
| Cycle Graph         | Vertices form a cycle |
| Cyclic Graph        | Contains at least one cycle |
| Directed Acyclic Graph (DAG) | Directed graph with no cycles |
| Bipartite Graph     | Vertices can be divided into two sets with no intra-set edges |
| Weighted Graph      | Edges have weights |

---

## 🛠️ Graph Representations

### 📗 Adjacency Matrix

- 2D array
- `mat[i][j] = 1` means an edge exists between vertex `i` and `j`

#### Python
```python
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

### 📘 Adjacency List

- Each vertex holds a list of its neighbors

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

## ⚖️ Matrix vs List Comparison

| Operation         | Adjacency Matrix | Adjacency List |
|------------------|------------------|----------------|
| Add Edge         | O(1)             | O(1)           |
| Remove Edge      | O(1)             | O(N)           |
| Initialize Graph | O(N²)            | O(N)           |

---

## 🔍 Graph Traversal

### 🔹 Depth First Search (DFS)

#### Key Ideas:
- Traverse deep into the graph before backtracking.
- Time: O(V + E), Space: O(V)

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

---

### 🔹 Breadth First Search (BFS)

#### Key Ideas:
- Explore neighbors level-by-level
- Uses a queue

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

---

## 🔄 Traversing Disconnected Graphs

Use loops to ensure **DFS** or **BFS** is called on all components:

```python
def dfs_disconnected(adj):
    V = len(adj)
    visited = [False] * V
    res = []
    for i in range(V):
        if not visited[i]:
            dfsRec(adj, visited, i, res)
    return res
```

---

## 🖼️ Images

To include images (like graph diagrams), add them in the repo and use:
```markdown
![Alt Text](images/graph-example.png)
```

---

## ✅ Output Example

```
Adjacency List Representation:
0: 1 2
1: 0 2
2: 0 1 3
3: 2

BFS: 0 1 2 3
DFS: 0 1 2 3
```


