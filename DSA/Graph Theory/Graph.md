## What is a Graph?

A **Graph** is a **non-linear data structure** used to represent **relationships between different entities (nodes/vertices)**.

A graph consists of:

- **Vertices (Nodes)** – The individual elements or points in the graph
- **Edges** – Connections between the vertices

Example
If cities are nodes and roads connecting them are edges, the network forms a **graph**.

Uses of Graph

- Social Media: user → vertices and friends / followers → edges
- Google Maps: City / Locations → vertices and roads → edges
- Internet: Computer → vertices and Cables → edges

Representation: G (V, E) → A graph with V vertices and E edges
V → Set of vertices, E → Set of edges

---

### Key Characteristics

- Graphs represent **pairwise relationships**.
- They can be:
  - **Undirected Graph** – edges do not have direction.
  - **Directed Graph (Digraph)** – edges have direction.
    ![[Types of Graphs.png|270]]

- Graphs may also be:
  - **Weighted** – edges have a cost or weight.
    ![[Weighted Graph.png|254]]
  - **Unweighted** – edges have no weight.
    ![[Unweighted.png|257]]

---

### Important Graph Terminology

#### Path

A **Path** is a sequence of vertices connected by edges.
Types of Paths:

- Simple Path → No vertex repeats
- Closed Path → Start and end vertex are same

Example

```code
A → B → C → D
```

This means there is a path from **A to D**.

---

### Cycle in a Graph

A Cycle is a path that starts and ends at the same vertex without repeating vertices (except the first/last).

Types of cycles:

- **Cycle in Undirected Graph**
- **Cycle in Directed Graph**

Cycle detection is a **common interview problem**. (We will Solve this further)

---

### Degree of a Vertex

#### For Undirected Graphs:

The **degree** of a vertex is the **number of edges connected to it**.

Example:

```
A --- B
|     |
C --- D
```

Degree(A) = 2
Degree(B) = 2
Degree(C) = 2
Degree(D) = 2

**_Property: Sum of degrees of all vertices = 2 × |E|_**

#### In Directed Graphs:

There are two types of degrees:

- **In-Degree** → Number of edges **coming into a vertex**
- **Out-Degree** → Number of edges **going out from a vertex**
  **_Property: Summation of in-degree of all vertices = Summation of out-degree of all vertices_**

Example:

```
A → B → C
```

For **B**:

- In-Degree = 1
- Out-Degree = 1

---

### Connected Components

A **Connected Component** is a group of vertices in a graph where **every vertex is reachable from every other vertex** in that group.

Connected components are defined for undirected graphs.
For directed graphs, we use:
• Strongly Connected Components (SCC)
• Weakly Connected Components

![[Connected Components.png|220]]

This graph has **2 connected components**.

### Important Points

- Applies mainly to **Undirected Graphs**
- Used in problems like:
  - Number of Provinces
  - Number of Islands
  - Network connectivity

Connected components are usually found using

- **DFS**
- **BFS**

---

### How To Store a Graph?

1. Adjacency Matrix
   A **2D matrix** used to represent edges.

   Example graph:

   ```
   A —-- B
   |     |
   C —-- D
   ```

   Matrix

```
  A B C D
A 0 1 1 0
B 1 0 0 1
C 1 0 0 1
D 0 1 1 0
```

Example directed graph:

```
A —-> B
^     |
|     v
C <-- D
```

Matrix

```
  A B C D
A 0 1 0 0
B 0 0 0 1
C 1 0 0 0
D 0 0 1 0
```

if:

```
matrix[i][j] = 1 → edge from i to j
```

For **undirected graphs**

```
matrix[i][j] = matrix[j][i]
```

### Space Complexity: O(V<sup>2</sup>)

Where **V = number of vertices**

1 based Graph

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjMatrix(n + 1, vector<int>(n + 1, 0));

    // Undirected Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjMatrix[u][v] = 1;
        adjMatrix[v][u] = 1;
    }

    return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjMatrix(n + 1, vector<int>(n + 1, 0));

    // Directed Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjMatrix[u][v] = 1;
    }

    return 0;
}
```

0 based Graph

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjMatrix(n, vector<int>(n, 0));

    // Undirected Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjMatrix[u][v] = 1;
        adjMatrix[v][u] = 1;
    }

    return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjMatrix(n, vector<int>(n, 0));

    // Directed Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjMatrix[u][v] = 1;
    }

    return 0;
}
```

**_For weighted Graph: Replace 1 with weight of the edge_**

---

2. Adjacency List (Most Used)
   Each vertex stores a **list of its neighbors**.

Example graph:

```
A —-- B
|  \  |
C —-- D
```

Representation

```
A : [B, C, D]
B : [A, D]
C : [A, D]
D : [A, B, C]
```

Example directed graph:

```
A —-> B
^     |
|     v
C <-- D
```

List

```
A : [B]
B : [D]
C : [A]
D : [C]
```

### Space Complexity: O(V + E)

Where **V = number of vertices & E = number of edges**
This is **much more efficient for sparse graphs**.

1 based Graph

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjList(n + 1);

    // Undirected Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjList[u].push_back(v);
        adjList[v].push_back(u);
    }

    return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjList(n + 1);

    // Directed Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjList[u].push_back(v);
    }

    return 0;
}
```

0 based Graph

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjList(n);

    // Undirected Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjList[u].push_back(v);
        adjList[v].push_back(u);
    }

    return 0;
}
```

```cpp
#include <bits/stdc++.h>
using namespace std;

int main() {
    int n, m;
    cin >> n >> m;

    vector<vector<int>> adjList(n);

    // Directed Graph
    for (int i = 0; i < m; i++) {
        int u, v;
        cin >> u >> v;
        adjList[u].push_back(v);
    }

    return 0;
}
```

**_For weighted Graph: Make a list of node and weight and store this in the list_**

```cpp
vector<vector<pair<int, int>>> adjList(n);
```

Example

```cpp
adj[u].push_back({v, weight});
```

---

### Graph Traversal Algorithms

Graph traversal means **visiting all vertices of a graph systematically**.
BFS - [[BFS]]
DFS - [[DFS]]

Shortest Path Algorithms:
Dijkstra's Algorithm - [[Dijkstra's Algo]]
Bellman Ford Algorithm - [[Bellman Ford Algo]]
Floyd Warshall Algorithm - [[Floyd Warshall Algo]]

---

### Problems on Graphs

**_Problems on BFS/DFS_**
Number of Provinces - [[Number of Provinces]]
Number of Islands - [[Number of Islands]]
Flood Fill - [[Flood Fill]]
Rotten Orange - [[Rotten Orange]]
Distance of nearest cell - [[Distance of nearest cell]]
Surrounded Regions - [[Surrounded Regions]]
Number of Enclaves - [[Number of Enclaves]]
Number of distinct Islands - [[Number of distinct Islands]]
Bipartite Graph - [[Bipartite Graph]]
Cycle Detection in Undirected Graph - [[Cycle Detection in undirected graph]]
Cycle Detection in Directed Graph - [[Cycle detection in directed graph]]

**_Problems on Topological Sort_**
Topological Sort -[[Topological Sort]]
Cycle Detection in Directed Graph - [[Cycle detection in directed graph]]
Course Schedule - [[Course Schedule]]
Course Schedule II - [[Course Schedule II]]
Find Eventual Safe States - [[Find Eventual Safe States]]
Alien Dictionary - [[Alien Dictionary]]

**_Problems on Shortest Path_**
Shortest Path in Directed Acyclic Graph - [[Shortest Path in Directed Acyclic Graph]]
Shortest Path in Undirected Graph with Unit Weight - [[Shortest Path in Undirected Graph with Unit Weight]]
Word Ladder - [[Word Ladder]]
Word Ladder II - [[Word Ladder II - GFG]]
Word Ladder II - [[Word Ladder II - Leetcode]]
Dijkstra's Algorithm - [[Dijkstra's Algorithm]]
Shortest Distance in a Binary Maze - [[Shortest Distance in a Binary Maze]]
Path with Minimum Effort - [[Path with Minimum Effort]]
Cheapest Flights Within K Stops - [[Cheapest Flights Within K Stops]]
Minimum Multiplications to reach End - [[Minimum Multiplications to reach End]]
Number of Ways to Arrive at Destination - [[Number of Ways to Arrive at Destination]]
Bellman Ford Algorithm - [[Bellman Ford Algorithm]]
Floyd Warshall Algorithm - [[Floyd Warshall Algorithm]]
Find the City With the Smallest Number of Neighbors at a Threshold Distance - [[Find the City With the Smallest Number of Neighbors at a Threshold Distance]]

**_Problems on DisjointSet_**
Prim's Algorithm - [[Minimum Spanning Tree (MST)]]
Kruskal's Algorithm - [[Minimum Spanning Tree (MST)]]
Number of Operations to Make Network Connected - [[Number of Operations to Make Network Connected]]
Most Stones Removed with Same Row or Column - [[Most Stones Removed with Same Row or Column]]
Accounts Merge - [[Accounts Merge]]
Number of Islands - II - [[Number of Islands - II]]
Making A Large Island - [[Making A Large Island]]

**_Other Algorithms_**
Kosaraju's algorithm - [[Kosaraju's algorithm]]
Tarjan's Algorithm - [[Tarjan's Algorithm]]
Articulation point in graph - [[Articulation point in graph]]
