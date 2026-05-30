 ### Overview
The **Bellman-Ford Algorithm** is a graph algorithm used to find the **shortest path from a single source vertex to all other vertices** in a weighted graph.

Unlike [[Dijkstra's Algo]], Bellman-Ford can handle:

- Negative edge weights
- Detection of negative-weight cycles

It is slower than Dijkstra’s Algorithm but more flexible.

---

### Intuition
The main idea is:
> Relax all edges repeatedly until the shortest distances stabilize.

A shortest path in a graph with `V` vertices can contain at most `V - 1` edges.
So, if we relax every edge exactly `V - 1` times, all shortest paths will be correctly computed.

---

### Problem it solves
#### Single-Source Shortest Path (SSSP)
Given:
- A weighted graph
- A source node `S`

Find:
- Shortest distance from `S` to every other node

---

### Why Bellman-Ford Is Special
Bellman-Ford works even when:
- Edge weights are negative
- Paths become shorter because of negative edges

Example:

```
A ----4----> BA ----5----> CC ----(-10)-> B
```

Shortest path from `A` to `B`:

```
A → C → B = 5 + (-10) = -5
```

Dijkstra may fail here, but Bellman-Ford works correctly.

---

### Key Properties

| Property                  | Value                 |
| ------------------------- | --------------------- |
| Graph Type                | Directed / Undirected |
| Negative Weights          | ✅ Supported           |
| Negative Cycle Detection  | ✅ Yes                 |
| Greedy Algorithm          | ❌ No                  |
| Dynamic Programming Style | ✅ Yes                 |
| Time Complexity           | `O(V × E)`            |
| Space Complexity          | `O(V)`                |

---

### Limitations

> [!warning]  
> Bellman-Ford cannot produce correct shortest paths if the graph contains a **negative-weight cycle reachable from the source**.

Example:

```
A → B (-1)B → C (-1)C → A (-1)
```

You can keep reducing path cost infinitely.

---

### Core Concept: Edge Relaxation
Relaxation means:
> Try to improve the currently known shortest distance.

If:
```
dist[u] + weight < dist[v]
```

Then update:
```
dist[v] = dist[u] + weight
```

---

### Step-by-Step Algorithm

#### Step 1: Initialize Distances
- Distance to source = `0`
- Distance to all other nodes = `∞`

---

#### Step 2: Relax All Edges `V - 1` Times
For every edge `(u, v, wt)`:

```
if dist[u] + wt < dist[v]    dist[v] = dist[u] + wt
```

Repeat this process `V - 1` times.

---

#### Step 3: Detect Negative Cycle
Relax all edges one more time.

If any distance still decreases:

```
Negative cycle exists
```

---

### Mathematical Insight
A shortest path can contain at most:

```
V - 1 edges
```

because any longer path must repeat a vertex, creating a cycle.

---

### Visualization of Relaxation

`dist[v] = min⁡(dist[v], dist[u] + w(u, v))`

---

### Negative Cycle Detection

#### How It Works
After performing `V - 1` relaxations:

- If a shorter path is still found,
- then a negative cycle exists.

---

### Why?
Because shortest paths should already be finalized after `V - 1` iterations.

Further improvement implies infinite reduction.

---

### Time Complexity

| Operation              | Complexity |
| ---------------------- | ---------- |
| Relaxation             | `O(E)`     |
| Repeated `V - 1` times | `O(V × E)` |
| Space Complexity       | `O(V)`     |

---

### Complexity Formula

`T(V , E) = O (V × E)`

---

### Comparison with Dijkstra’s Algorithm

|Feature|Bellman-Ford|Dijkstra|
|---|---|---|
|Negative Weights|✅ Yes|❌ No|
|Negative Cycle Detection|✅ Yes|❌ No|
|Faster|❌ Slower|✅ Faster|
|Greedy|❌ No|✅ Yes|
|Complexity|`O(VE)`|`O(E log V)`|

---

### When To Use Bellman-Ford
Use Bellman-Ford when:
- Graph has negative weights
- Need negative cycle detection
- Accuracy matters more than speed

Use Dijkstra when:
- All weights are non-negative
- Faster performance is required

---

### Real-World Applications

#### Networking Protocols

Used in:
- Distance Vector Routing Protocols
- RIP (Routing Information Protocol)

---

#### Currency Arbitrage Detection

Negative cycles can indicate profitable arbitrage opportunities.

---

#### Traffic Systems

Handles routes with penalties or credits.

---

#### Financial Graphs

Used in cost optimization and exchange systems.

---

### C++ Implementation - [[Bellman Ford Algorithm]]

---

### Important Edge Cases

#### Disconnected Graph

Some nodes remain:

```
∞ (infinity)
```

---

#### Negative Self Loop

```
A → A (-5)
```

This immediately forms a negative cycle.

---

#### Multiple Edges Between Same Nodes

Algorithm still works correctly.

---

### Optimization Trick

> [!tip]  
> If during an iteration no distance gets updated, stop early.

This improves performance in many practical cases.

---

### Mnemonics

#### “Relax Edges Repeatedly”

Think:

```
Bellman-Ford = Repeated Relaxation
```

---

#### Memory Trick

| Algorithm    | Handles Negative Weights? |
| ------------ | ------------------------- |
| BFS          | No                        |
| Dijkstra     | No                        |
| Bellman-Ford | Yes                       |

---

### Important Takeaways

> [!success]
> 
> - Bellman-Ford computes shortest paths from one source
> - Works with negative edge weights
> - Detects negative cycles
> - Uses repeated edge relaxation
> - Time complexity is `O(V × E)`
> - Slower than Dijkstra but more powerful

---

# Summary

The **Bellman-Ford Algorithm** is one of the most important shortest path algorithms in graph theory. It is especially useful when graphs contain **negative edge weights** or when detecting **negative cycles** is necessary.

Although slower than Dijkstra’s Algorithm, Bellman-Ford is more flexible and widely used in networking and financial systems.