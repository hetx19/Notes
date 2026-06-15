### Overview
**Floyd–Warshall Algorithm** is a dynamic programming algorithm used to find the **shortest paths between every pair of vertices** in a weighted graph.

Unlike Dijkstra or Bellman-Ford, which compute shortest paths from a single source, Floyd-Warshall computes:

> Shortest distance from every node to every other node.

---

### Intuition
The main idea is:

> Gradually allow more vertices to be used as intermediate nodes in paths.

Suppose we want the shortest path from `i` to `j`.

Initially:
- We only know direct edges.    

Then:
- Allow vertex `1` as an intermediate.
- Then allow vertices `{1,2}`.
- Then `{1,2,3}`.
- Continue until all vertices are allowed.

At each step, we ask:
```
Is it shorter to go directly?
OR
Go through vertex k?
```

---

### Problem It Solves
#### All-Pairs Shortest Path (APSP)

Given:
- A weighted graph
- Directed or undirected

Find:
```
Shortest distance between every pair of vertices.
```

---

### Key Properties

| Property                 | Value                 |
| ------------------------ | --------------------- |
| Algorithm Type           | Dynamic Programming   |
| Graph Type               | Directed / Undirected |
| Negative Weights         | ✅ Supported           |
| Negative Cycle Detection | ✅ Yes                 |
| All-Pairs Shortest Path  | ✅ Yes                 |
| Time Complexity          | `O(V³)`               |
| Space Complexity         | `O(V²)`               |

---

### Core Idea
For every pair `(i, j)`:
Check whether using vertex `k` as an intermediate node improves the path.

---

### DP State

Define:
```
dist[i][j]
```

as:
```
Shortest distance from i to j
```

using currently allowed intermediate vertices.

---

### Transition Formula
`dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`

This is the heart of Floyd-Warshall.

---

### Dynamic Programming Interpretation

At iteration `k`:
```
All paths are allowed to use vertices
{1,2,...,k}
```

as intermediate nodes.
For every pair `(i,j)`:

```
Either:
Don't use k
OR
Use k
```

Choose the better option.

---

### Step-by-Step Algorithm

#### Step 1: Initialize Distance Matrix

For every pair `(i,j)`:
```
dist[i][j] = weight(i,j)
```

If no edge exists:
```
dist[i][j] = ∞
```

For all vertices:
```
dist[i][i] = 0
```

---

#### Step 2: Iterate Through Intermediate Vertices

For every vertex:
```
k = 1 to V
```

Try using `k` as an intermediate node.

---

#### Step 3: Update All Pairs

For every pair `(i,j)`:
```
dist[i][j] =
min(
    dist[i][j],
    dist[i][k] + dist[k][j]
)
```

---

### Triple Loop Structure

```cpp
for (int k = 0; k < V; k++) {
    for (int i = 0; i < V; i++) {
        for (int j = 0; j < V; j++) {
            dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
        }
    }
}
```

---

### Why It Works

At step `k`:
```
dist[i][j]
```

contains the shortest path using only vertices:
```
{1,2,...,k-1}
```

Then we check:
```
Can vertex k improve the route?
```

If yes:
```
i → k → j
```

becomes the new shortest path.

---

### Time Complexity

Three nested loops:
```
k
i
j
```

Total:
`O(V³)`

---

### Complexity Formula

`T(V) = O(V³)`

---

### Space Complexity

Distance matrix:
```
V × V
```

Space:
`O(V²)`

---

### Negative Weights

> [!success]  
> Floyd-Warshall works correctly with negative edge weights.

Example:
```
A → B = 5
B → C = -3
```

Algorithm still computes correct shortest paths.

---

### Negative Cycle Detection

One of the biggest advantages of Floyd-Warshall:
> [!danger]  
> It can detect negative cycles.

After the algorithm finishes:
Check:
```
dist[i][i]
```

for every vertex.

If:
```
dist[i][i] < 0
```

then a negative cycle exists.

---

### Why Does This Work?

Normally:
```
Distance from a node to itself = 0
```

If it becomes negative:
```
i → ... → i < 0
```

Then some cycle reduces the path cost indefinitely.

---

### Example

Graph:
```
A → B = 1
B → C = -2
C → A = -2
```

Cycle cost:
```
1 + (-2) + (-2) = -3
```

After Floyd-Warshall:
```
dist[A][A] < 0
```

Negative cycle detected.

---

### Edge Cases

#### 1. Disconnected Graph
Some entries remain:

```
∞
```

indicating no path exists.

---

#### 2. Self Loops
Handled naturally through matrix initialization.

---

#### 3. Negative Edges
Fully supported.

---

#### 4. Negative Cycles
Can be detected using:

```
dist[i][i] < 0
```

---

#### 5. Dense Graphs
Floyd-Warshall performs well conceptually because it processes all pairs directly.

---

### Comparison With Other Shortest Path Algorithms

|Algorithm|Single Source|All Pairs|Negative Weights|Negative Cycle Detection|
|---|---|---|---|---|
|Dijkstra|✅|❌|❌|❌|
|Bellman-Ford|✅|❌|✅|✅|
|Floyd-Warshall|❌|✅|✅|✅|

---

### Real-World Applications

#### Network Routing Analysis
Compute shortest communication paths between every pair of routers.

---

#### Transportation Systems
Find shortest travel distances between all cities.

---

#### Airline Route Planning
Calculate minimum travel costs between every airport pair.

---

#### Social Networks
Determine minimum connection distance between users.

---

#### Graph Analytics
Used in transitive closure and reachability problems.

---

### C++ Implementation = [[Floyd Warshall Algorithm]]

---

### Optimization Notes

#### Use Adjacency Matrix
Floyd-Warshall naturally works on:

```
dist[V][V]
```

rather than adjacency lists.

---

#### Infinity Handling
Avoid overflow:

```cpp
if (dist[i][k] != INF && dist[k][j] != INF) {
    dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
}
```

---

### Mnemonics

#### “Try Every Vertex In Between”

Think:
```
Floyd-Warshall =
For every pair,
try every node as a middleman.
```

---

### Important Takeaways

> [!success]
> 
> - Floyd-Warshall solves All-Pairs Shortest Path (APSP)
>     
> - Uses Dynamic Programming
>     
> - Supports negative edge weights
>     
> - Detects negative cycles
>     
> - Works using a distance matrix
>     
> - Time Complexity = `O(V³)`
>     
> - Space Complexity = `O(V²)`
>     

---

### Summary
**Floyd-Warshall Algorithm** is a dynamic programming algorithm that computes the shortest paths between every pair of vertices in a graph.

It repeatedly checks whether an intermediate vertex can improve an existing path. Unlike Dijkstra, it supports negative edge weights and can detect negative cycles. Although its `O(V³)` time complexity makes it unsuitable for very large graphs, it remains one of the simplest and most elegant solutions for the All-Pairs Shortest Path problem.