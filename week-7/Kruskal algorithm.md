# Kruskal algorithm

Kruskal's Algorithm is a **greedy algorithm** used to find the **Minimum Spanning Tree (MST)** of a **connected, undirected, weighted graph**.

* An MST is a subset of the edges that connects all vertices in the graph without forming any cycle and with the **minimum total edge weight**.

# Basic Idea

1. Sort all edges in **ascending order** of their weights.

2. Pick the **smallest edge** and **add it to the MST** if it **doesn’t form a cycle** with the already included edges.

3. Repeat step 2 until we have **V \- 1 edges** in the MST (where V is the number of vertices).

# Algorithm Steps

Let `G = (V, E)` be the graph:

1. **Sort** all edges in non-decreasing order of their weights.

2. Initialize:

   * MST set as empty  
   * DSU structure to keep track of connected components

3. For each edge `(u, v)` in the sorted edge list:

   * If `u` and `v` belong to **different sets** (i.e., not connected):  
     * **Add** edge `(u, v)` to the MST  
     * **Union** the sets of `u` and `v`

4. Stop when MST has exactly `V - 1` edges.

# Data Structures Used

* **Edge list** for all graph edges.  
    
* **Disjoint Set (Union-Find)** with:

  * `find()` to get the set of a node.  
  * `union()` to merge two sets.  
  * Path compression and union by rank for efficiency.

# Time Complexity

* Sorting edges: **O(E log E)**  
* **Overall: O(E log E)**

# Space Complexity

* **O(V \+ E)**: For edge list and DSU.

# Code

## Python 

\# Define the graph as a list of edges (u, v, weight)  
edges \= \[  
    ('A', 'B', 1),  
    ('B', 'C', 4),  
    ('A', 'C', 3),  
    ('C', 'D', 2),  
    ('B', 'D', 5\)  
\]

\# Sort edges based on weight  
edges.sort(key=lambda x: x\[2\])

\# Union-Find structure  
parent \= {}

\# Find function with path compression  
def find(node):  
    if parent\[node\] \!= node:  
        parent\[node\] \= find(parent\[node\])  
    return parent\[node\]

\# Union function  
def union(u, v):  
    root\_u \= find(u)  
    root\_v \= find(v)  
    if root\_u \!= root\_v:  
        parent\[root\_v\] \= root\_u  
        return True  
    return False

\# Initialize disjoint sets  
for u, v, \_ in edges:  
    if u not in parent:  
        parent\[u\] \= u  
    if v not in parent:  
        parent\[v\] \= v

\# Kruskal's Algorithm  
mst \= \[\]  
total\_cost \= 0

for u, v, weight in edges:  
    if union(u, v):  
        mst.append((u, v, weight))  
        total\_cost \+= weight

\# Print the result  
print("Minimum Spanning Tree:")  
for u, v, weight in mst:  
    print(f"{u} \- {v}: {weight}")  
print("Total Cost:", total\_cost)

## C++

\#include \<iostream\>  
\#include \<vector\>  
\#include \<algorithm\>  
using namespace std;

// Structure to represent an edge  
struct Edge {  
    char u, v;  
    int weight;  
};

// Find function  
char find(char node, unordered\_map\<char, char\>& parent) {  
    if (parent\[node\] \!= node)  
        parent\[node\] \= find(parent\[node\], parent);  
    return parent\[node\];  
}

// Union function  
bool union\_sets(char u, char v, unordered\_map\<char, char\>& parent) {  
    char root\_u \= find(u, parent);  
    char root\_v \= find(v, parent);  
    if (root\_u \!= root\_v) {  
        parent\[root\_v\] \= root\_u;  
        return true;  
    }  
    return false;  
}

int main() {  
    vector\<Edge\> edges \= {  
        {'A', 'B', 1},  
        {'B', 'C', 4},  
        {'A', 'C', 3},  
        {'C', 'D', 2},  
        {'B', 'D', 5}  
    };

    // Sort edges by weight  
    sort(edges.begin(), edges.end(), \[\](Edge a, Edge b) {  
        return a.weight \< b.weight;  
    });

    unordered\_map\<char, char\> parent;  
    for (auto edge : edges) {  
        parent\[edge.u\] \= edge.u;  
        parent\[edge.v\] \= edge.v;  
    }

    int totalCost \= 0;  
    cout \<\< "Minimum Spanning Tree:\\n";  
    for (auto edge : edges) {  
        if (union\_sets(edge.u, edge.v, parent)) {  
            cout \<\< edge.u \<\< " \- " \<\< edge.v \<\< ": " \<\< edge.weight \<\< endl;  
            totalCost \+= edge.weight;  
        }  
    }

    cout \<\< "Total Cost: " \<\< totalCost \<\< endl;  
    return 0;  
}

## Javascript

const edges \= \[  
  \['A', 'B', 1\],  
  \['B', 'C', 4\],  
  \['A', 'C', 3\],  
  \['C', 'D', 2\],  
  \['B', 'D', 5\]  
\];

// Sort edges by weight  
edges.sort((a, b) \=\> a\[2\] \- b\[2\]);

// Disjoint Set (Union-Find)  
const parent \= {};  
function find(x) {  
  if (parent\[x\] \!== x) {  
    parent\[x\] \= find(parent\[x\]);  
  }  
  return parent\[x\];  
}

function union(u, v) {  
  const rootU \= find(u);  
  const rootV \= find(v);  
  if (rootU \!== rootV) {  
    parent\[rootV\] \= rootU;  
    return true;  
  }  
  return false;  
}

// Initialize parents  
edges.forEach((\[u, v\]) \=\> {  
  if (\!parent\[u\]) parent\[u\] \= u;  
  if (\!parent\[v\]) parent\[v\] \= v;  
});

// Kruskal's algorithm  
let totalCost \= 0;  
const mst \= \[\];

for (const \[u, v, weight\] of edges) {  
  if (union(u, v)) {  
    mst.push(\[u, v, weight\]);  
    totalCost \+= weight;  
  }  
}

console.log("Minimum Spanning Tree:");  
mst.forEach((\[u, v, weight\]) \=\> {  
  console.log(\`${u} \- ${v}: ${weight}\`);  
});  
console.log("Total Cost:", totalCost);

# Advantages

* Simple and efficient for **sparse graphs** (where E is not much larger than V).  
* Performs better than Prim’s algorithm when the graph is represented as an edge list.

# Disadvantages

* Requires sorting all edges upfront.  
* Not as efficient for **dense graphs** if implemented naively.

# 

