### Overview
**Dijkstra’s Algorithm** is a greedy graph algorithm used to find the **shortest path from a single source vertex to all other vertices** in a weighted graph.

It is one of the most widely used shortest path algorithms because of its efficiency and simplicity.

---

### Intuition
The idea is simple:
> Always expand the node with the currently smallest known distance.

Once a node gets the minimum possible distance, that distance is finalized and never changes again.

Think of it like:

```
Ripples spreading outward from the source node.
```

The closest nodes are processed first, then farther ones.

---

### Problem It Solves

#### Single-Source Shortest Path (SSSP)

Given:
- A weighted graph
- A source node `S`

Find:
- The minimum distance from `S` to every other vertex

---

### Important Restriction

> [!warning]  
> Dijkstra’s Algorithm does **NOT** work correctly with negative edge weights.

Example:

```
A → B = 5A → C = 2C → B = -10
```

Dijkstra may incorrectly finalize node `B` too early.
For graphs with negative weights, use:

- [[Bellman Ford Algo]]

---

### Key Properties

| Property                 | Value                     |
| ------------------------ | ------------------------- |
| Algorithm Type           | Greedy                    |
| Graph Type               | Directed / Undirected     |
| Negative Weights         | ❌ Not Supported           |
| Negative Cycle Detection | ❌ No                      |
| Data Structure Used      | Priority Queue / Min Heap |
| Time Complexity          | `O((V + E) log V)`        |
| Space Complexity         | `O(V + E)`                |

---

### Core Idea
At every step:

1. Pick the unvisited node with the smallest distance
2. Relax all its adjacent edges
3. Update shorter distances if found

---

### Relaxation Formula

`dist[v] = min⁡(dist[v], dist[u] + w(u,v))`

---

### Greedy Nature

> [!tip]  
> Once a node is removed from the priority queue with the minimum distance, its shortest distance is finalized.

This greedy property only works when all weights are non-negative.

---

### Step-by-Step Algorithm

#### Step 1: Initialize Distances
- Distance to source = `0`
- Distance to all other nodes = `∞`

---

#### Step 2: Use a Min Heap / Priority Queue
Store:

```
(distance, node)
```

---

#### Step 3: Extract Minimum Distance Node
Pick the closest unvisited node.

---

#### Step 4: Relax Adjacent Edges
For every adjacent node:

```
if dist[u] + weight < dist[v]
```

Update:

```
dist[v] = dist[u] + weight
```

Push updated value into the heap.

---

### Time Complexity

#### Using Min Heap

| Operation        | Complexity         |
| ---------------- | ------------------ |
| Insert into PQ   | `O(log V)`         |
| Extract Min      | `O(log V)`         |
| Total Complexity | `O((V + E) log V)` |

---

### Complexity Formula

`T(V, E) = O((V + E) log⁡V)`

---

### Space Complexity

| Component      | Complexity |
| -------------- | ---------- |
| Distance Array | `O(V)`     |
| Priority Queue | `O(V)`     |
| Adjacency List | `O(E)`     |
| Total          | `O(V + E)` |

---

### Why Negative Weights Break Dijkstra

Dijkstra assumes:

```
Once a shortest path is finalized,it never becomes smaller later.
```

Negative edges violate this assumption.

---

### Example Failure

```
A → B = 2A → C = 5C → B = -10
```

Dijkstra finalizes `B = 2` too early.

Actual shortest path:

```
A → C → B = -5
```

---

### Negative Cycle Detection

> [!danger]  
> Dijkstra’s Algorithm cannot detect negative cycles.

If the graph contains negative cycles:
- Results become invalid
- Algorithm assumptions break

Use:
- [[Bellman Ford Algo]]
for negative cycle detection.

---

### Edge Cases

#### 1. Disconnected Graph

Some nodes remain unreachable.

Distance remains:

```
∞
```

---

#### 2. Multiple Edges Between Same Nodes
Algorithm still works correctly.
It automatically chooses the smaller edge.

---

#### 3. Self Loops
Usually ignored unless beneficial.

---

#### 4. Large Sparse Graphs
Dijkstra performs extremely well with adjacency lists + heaps.

---

#### 5. Dense Graphs
Can become slower due to many edges.

---

### Real-World Applications

#### GPS Navigation

Used in:
- Google Maps
- Apple Maps
- Route optimization systems

---

#### Network Routing
Finds shortest data transmission paths.

---

#### Airline Systems
Used for cheapest/shortest routes.

---

#### Robotics
Helps robots determine optimal movement paths.

---

#### Gaming
NPC movement and pathfinding.

---

### C++ Implementation = [[Dijkstra's Algorithm]]

---

### Optimization Tricks

#### Lazy Deletion
Ignore outdated priority queue entries:

```
if (d > dist[node]) continue;
```

Improves efficiency.

---

### Mnemonics

#### “Pick the Closest First”

Think:

```
Dijkstra = Greedy Closest Expansion
```

---

### Important Takeaways

> [!success]
> 
> - Dijkstra finds shortest paths from one source
> - Works only with non-negative weights
> - Uses greedy strategy + priority queue
> - Faster than Bellman-Ford
> - Commonly used in real-world routing systems

---

### Summary

**Dijkstra’s Algorithm** is one of the most efficient and widely used shortest path algorithms for weighted graphs with non-negative edge weights.

It uses a greedy approach with a priority queue to always process the nearest node first. Although it cannot handle negative weights or detect negative cycles, it is extremely fast and practical for routing, navigation, and optimization problems.