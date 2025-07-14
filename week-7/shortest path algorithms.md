# Dijkstra’s Algorithm

# What is Dijkstra’s Algorithm?

It is a popular algorithm for solving single-source shortest path problems having non-negative edge weight in the graphs i.e., it is to find the shortest distance between two vertices on a graph. 

# Problem Statement

Given a graph G=(V,E), where:

* V is the set of vertices,

* E is the set of edges with non-negative weights w(u,v) ≥ 0

The goal is to compute the minimum distance from a given source node s∈V to every other node v∈V.

# Working Principle

Dijkstra’s Algorithm is based on the **greedy approach**. At each step, it selects the vertex with the **smallest tentative distance** that has not yet been visited and updates the distances of its neighboring vertices.

# Algorithm Steps

1. ## Initialization: 

   * Set the distance of the **source node** to 0\.  
   * Set the distance of **all other nodes** to infinity.  
   * Maintain a **priority queue (or min-heap)** to efficiently fetch the next closest unvisited node.

2. ## Processing Nodes: 

   * While the priority queue is not empty:

     * Extract the node u with the minimum tentative distance.  
     * For each unvisited neighbor v of u, compute the new tentative distance:  
        New\_distance \= distance\[u\]+ weight(u,v)  
     * If the new distance is smaller than the current known distance, update it.

3. ## Termination: 

   * Repeat the process until all nodes are visited or the queue is empty.

### **Example**

Consider the following graph:

       `A`  
     `/ \`  
   `1/   \4`  
   `B     C`  
    `\   /`  
    `2\ /1`  
      `D`

**Steps**:

* Start from A: distances → A=0, B=∞, C=∞, D=∞  
* Visit A: update B=1, C=4  
* Visit B: update D \= 1 \+ 2 \= 3  
* Visit D: update C \= min(4, 3+1) \= 4  
* Visit C: done

**Final shortest distances from A**:

* A \= 0  
* B \= 1  
* D \= 3  
* C \= 4

# Code

## Python

def dijkstra(graph, start):  
    \# Initialize distances and visited set  
    visited \= set()  
    distances \= {node: float('inf') for node in graph}  
    distances\[start\] \= 0

    while len(visited) \< len(graph):  
        \# Select the unvisited node with the smallest distance  
        current\_node \= None  
        for node in graph:  
            if node not in visited:  
                if current\_node is None or distances\[node\] \< distances\[current\_node\]:  
                    current\_node \= node

        \# Mark the node as visited  
        visited.add(current\_node)

        \# Update distances to neighbors  
        for neighbor, weight in graph\[current\_node\]:  
            if distances\[current\_node\] \+ weight \< distances\[neighbor\]:  
                distances\[neighbor\] \= distances\[current\_node\] \+ weight

    return distances

## C++

\#include \<iostream\>  
\#include \<limits\>

using namespace std;

const int INF \= 1e9; // A large number representing infinity  
const int V \= 4;     // Number of vertices

void dijkstra(int graph\[V\]\[V\], int start) {  
    int dist\[V\];  
    bool visited\[V\];

    // Step 1: Initialize distances and visited array  
    for (int i \= 0; i \< V; i++) {  
        dist\[i\] \= INF;  
        visited\[i\] \= false;  
    }  
    dist\[start\] \= 0;

    // Step 2: Find shortest path for all vertices  
    for (int i \= 0; i \< V \- 1; i++) {  
        int minDist \= INF, u;

        // Find the unvisited node with the smallest distance  
        for (int j \= 0; j \< V; j++) {  
            if (\!visited\[j\] && dist\[j\] \< minDist) {  
                minDist \= dist\[j\];  
                u \= j;  
            }  
        }

        visited\[u\] \= true;

        // Update distances of neighbors of u  
        for (int v \= 0; v \< V; v++) {  
            if (\!visited\[v\] && graph\[u\]\[v\] && dist\[u\] \+ graph\[u\]\[v\] \< dist\[v\]) {  
                dist\[v\] \= dist\[u\] \+ graph\[u\]\[v\];  
            }  
        }  
    }

    // Print shortest distances  
    for (int i \= 0; i \< V; i++) {  
        cout \<\< "Distance from " \<\< start \<\< " to " \<\< i \<\< " is " \<\< dist\[i\] \<\< endl;  
    }  
}

## Javascript

function dijkstra(graph, start) {  
  const distances \= {};  
  const visited \= {};

  // Initialize distances  
  for (let node in graph) {  
    distances\[node\] \= Infinity;  
  }  
  distances\[start\] \= 0;

  while (Object.keys(visited).length \< Object.keys(graph).length) {  
    // Find the unvisited node with the smallest distance  
    let closestNode \= null;  
    for (let node in distances) {  
      if (\!visited\[node\]) {  
        if (closestNode \=== null || distances\[node\] \< distances\[closestNode\]) {  
          closestNode \= node;  
        }  
      }  
    }

    visited\[closestNode\] \= true;

    // Update distances to neighbors  
    for (let neighbor of graph\[closestNode\]) {  
      let \[nextNode, weight\] \= neighbor;  
      let newDist \= distances\[closestNode\] \+ weight;  
      if (newDist \< distances\[nextNode\]) {  
        distances\[nextNode\] \= newDist;  
      }  
    }  
  }

  return distances;  
}

# Time Complexity

| Data Structure | Time Complexity | When Used |
| ----- | ----- | ----- |
|         Array | O(V^2) | Dense graphs |
| Min-Heap (Binary Heap) | O((V+E)log⁡V) | Sparse graphs |

# Advantages

* Efficient and accurate for graphs with non-negative weights  
* Simple to implement  
* Optimized with data structures like heaps and Fibonacci heaps

# Limitations

* **Does not support negative edge weights**  
* Not the most optimal for very large graphs (in which A\* or Bidirectional Dijkstra might be better)  
* Needs the whole graph in memory

# Bellman ford algorithm

# What is it used for?

The **Bellman-Ford algorithm** is a **shortest path algorithm** used to find the **shortest distances from a single source vertex to all other vertices** in a **weighted graph**. Unlike Dijkstra’s algorithm, Bellman-Ford **can handle negative weight edges**, which makes it more versatile.

The main idea of the Bellman-Ford algorithm is **relaxation**. It tries to **improve the shortest path** estimates by iteratively updating the distances between connected vertices.

# Relaxation Process

If there is an edge from vertex `u` to `v` with weight `w`, and if the distance to `v` can be minimized by going through `u`, then we update the distance to `v`.

`if dist[u] + w < dist[v]: dist[v] = dist[u] + w`

This process is repeated **V-1 times**, where `V` is the number of vertices.

In the end, we do **one more iteration** over all edges to check for **negative weight cycles**. If we can still relax any edge, then a negative cycle exists.

# Time Complexity

 `O(V * E)`  
 – because we relax all `E` edges `V-1` times.

# Space Complexity

 `O(V)`  
 – for the `dist[]` array storing distances from the source.

# Code

## Python

def bellman\_ford(graph, V, E, source):  
    dist \= \[float('inf')\] \* V  
    dist\[source\] \= 0

    \# Relax all edges V-1 times  
    for \_ in range(V \- 1):  
        for u, v, w in graph:  
            if dist\[u\] \!= float('inf') and dist\[u\] \+ w \< dist\[v\]:  
                dist\[v\] \= dist\[u\] \+ w

    \# Check for negative weight cycle  
    for u, v, w in graph:  
        if dist\[u\] \!= float('inf') and dist\[u\] \+ w \< dist\[v\]:  
            print("Graph contains a negative weight cycle")  
            return

    print("Vertex Distance from Source")  
    for i in range(V):  
        print(f"{i}\\t{dist\[i\]}")

## C++ 

\#include \<iostream\>  
using namespace std;

const int MAX \= 100;  
const int INF \= 1e9;

int main() {  
    int V, E;  
    cin \>\> V \>\> E; // number of vertices and edges

    int edges\[MAX\]\[3\]; // each edge has (u, v, w)

    for (int i \= 0; i \< E; i++) {  
        cin \>\> edges\[i\]\[0\] \>\> edges\[i\]\[1\] \>\> edges\[i\]\[2\];  
    }

    int dist\[MAX\];  
    for (int i \= 0; i \< V; i++) dist\[i\] \= INF;

    int src;  
    cin \>\> src;  
    dist\[src\] \= 0;

    // Relax all edges V-1 times  
    for (int i \= 0; i \< V \- 1; i++) {  
        for (int j \= 0; j \< E; j++) {  
            int u \= edges\[j\]\[0\];  
            int v \= edges\[j\]\[1\];  
            int w \= edges\[j\]\[2\];

            if (dist\[u\] \!= INF && dist\[u\] \+ w \< dist\[v\]) {  
                dist\[v\] \= dist\[u\] \+ w;  
            }  
        }  
    }

    // Check for negative weight cycle  
    for (int j \= 0; j \< E; j++) {  
        int u \= edges\[j\]\[0\];  
        int v \= edges\[j\]\[1\];  
        int w \= edges\[j\]\[2\];

        if (dist\[u\] \!= INF && dist\[u\] \+ w \< dist\[v\]) {  
            cout \<\< "Negative weight cycle detected\\n";  
            return 0;  
        }  
    }

    // Print distances  
    for (int i \= 0; i \< V; i++) {  
        if (dist\[i\] \== INF)  
            cout \<\< "INF ";  
        else  
            cout \<\< dist\[i\] \<\< " ";  
    }

    return 0;  
}

## Javascript

function bellmanFord(V, edges, source) {  
    let dist \= Array(V).fill(Infinity);  
    dist\[source\] \= 0;

    // Relax all edges V-1 times  
    for (let i \= 0; i \< V \- 1; i++) {  
        for (let \[u, v, w\] of edges) {  
            if (dist\[u\] \+ w \< dist\[v\]) {  
                dist\[v\] \= dist\[u\] \+ w;  
            }  
        }  
    }

    // Check for negative weight cycle  
    for (let \[u, v, w\] of edges) {  
        if (dist\[u\] \+ w \< dist\[v\]) {  
            console.log("Negative weight cycle detected");  
            return;  
        }  
    }

    // Print distances  
    console.log("Shortest distances from source " \+ source \+ ":");  
    for (let i \= 0; i \< V; i++) {  
        console.log("Vertex", i, "Distance:", dist\[i\]);  
    }  
}

// Example usage:  
const V \= 5;  
const edges \= \[  
    \[0, 1, 6\], \[0, 2, 7\],  
    \[1, 2, 8\], \[1, 3, 5\], \[1, 4, \-4\],  
    \[2, 3, \-3\], \[2, 4, 9\],  
    \[3, 1, \-2\],  
    \[4, 0, 2\], \[4, 3, 7\]  
\];

bellmanFord(V, edges, 0);

# Floyd warshall algorithm

# Introduction

The **Floyd-Warshall algorithm** is a classical dynamic programming algorithm used to **find the shortest paths between all pairs of vertices** in a weighted graph. Unlike Dijkstra’s or Bellman-Ford algorithms that find the shortest path from a single source, Floyd-Warshall provides a full-pairwise shortest path matrix.

# Problem Statement

Given a **directed weighted graph** G=(V,E), where:

* V is the set of vertices (|V| \= n),  
* E is the set of edges with weights (can be negative but no negative cycles),

Find the **shortest path between every pair of vertices** (i,j) such that the sum of weights along the path from i to j is minimized.

# Assumptions

* The graph can have negative weights.  
* The graph **should not** contain **negative weight cycles** (if it does, the algorithm can detect them).  
* Self-loops (i.e., i→i) are assumed to be 0\.

# Main Idea

The Floyd-Warshall algorithm uses **Dynamic Programming**. The idea is:

Let:

* D^(k)\[i\]\[j\] be the shortest distance from vertex i to vertex j using only vertices from the set {1,2,...,k} as intermediate vertices.

Recursive relation:

![][image1]

This means:

* Either the shortest path from i to j **does not** go through vertex k, or  
* It **does** go through k, and we sum the shortest paths from i→k and k→j.

# Algorithm Steps

1. **Initialize** the distance matrix `dist[i][j]` as follows:

   * `dist[i][j] = 0` if i=j

   * `dist[i][j] = weight(i, j)` if edge (i,j) exists

   * `dist[i][j] = ∞` if (i,j) ∉ E  
2. **Iteratively update** all distances using the recursive relation.

#  Time and Space Complexity

| Aspect | Complexity |
| :---- | :---- |
| Time | O(V^3) |
| Space | O(V^2) |

# Code 

## Python

INF \= 99999  
V \= 4

\# Graph as adjacency matrix  
graph \= \[  
    \[0,     3,     INF,   5\],  
    \[2,     0,     INF,   4\],  
    \[INF,   1,     0,     INF\],  
    \[INF,   INF,   2,     0\]  
\]

\# Floyd-Warshall algorithm  
for k in range(V):  
    for i in range(V):  
        for j in range(V):  
            graph\[i\]\[j\] \= min(graph\[i\]\[j\], graph\[i\]\[k\] \+ graph\[k\]\[j\])

\# Print the result  
for row in graph:  
    for val in row:  
        print("INF" if val \== INF else val, end="\\t")  
    print()

## C++ 

\#include \<iostream\>  
using namespace std;

\#define V 4  
\#define INF 99999

int main() {  
    int graph\[V\]\[V\] \= {  
        {0,   3,   INF, 5},  
        {2,   0,   INF, 4},  
        {INF, 1,   0,   INF},  
        {INF, INF, 2,   0}  
    };

    // Floyd-Warshall algorithm  
    for (int k \= 0; k \< V; k++) {  
        for (int i \= 0; i \< V; i++) {  
            for (int j \= 0; j \< V; j++) {  
                if (graph\[i\]\[k\] \+ graph\[k\]\[j\] \< graph\[i\]\[j\])  
                    graph\[i\]\[j\] \= graph\[i\]\[k\] \+ graph\[k\]\[j\];  
            }  
        }  
    }

    // Print result  
    for (int i \= 0; i \< V; i++) {  
        for (int j \= 0; j \< V; j++) {  
            if (graph\[i\]\[j\] \== INF)  
                cout \<\< "INF\\t";  
            else  
                cout \<\< graph\[i\]\[j\] \<\< "\\t";  
        }  
        cout \<\< endl;  
    }

    return 0;  
}

## Javascript

const V \= 4;  
const INF \= 99999;

let graph \= \[  
  \[0,     3,     INF,   5\],  
  \[2,     0,     INF,   4\],  
  \[INF,   1,     0,     INF\],  
  \[INF,   INF,   2,     0\]  
\];

// Floyd-Warshall algorithm  
for (let k \= 0; k \< V; k++) {  
  for (let i \= 0; i \< V; i++) {  
    for (let j \= 0; j \< V; j++) {  
      if (graph\[i\]\[k\] \+ graph\[k\]\[j\] \< graph\[i\]\[j\]) {  
        graph\[i\]\[j\] \= graph\[i\]\[k\] \+ graph\[k\]\[j\];  
      }  
    }  
  }  
}

// Print result  
for (let i \= 0; i \< V; i++) {  
  let row \= "";  
  for (let j \= 0; j \< V; j++) {  
    row \+= (graph\[i\]\[j\] \=== INF ? "INF" : graph\[i\]\[j\]) \+ "\\t";  
  }  
  console.log(row);  
}

[image1]: <data:image/png;base64,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>