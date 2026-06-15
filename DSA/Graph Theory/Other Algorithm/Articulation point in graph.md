### Definition
An **Articulation Point** (or **Cut Vertex**) is a vertex in an **undirected graph** whose removal increases the number of connected components in the graph.

In other words, removing the vertex (along with its incident edges) disconnects some part of the graph.

---

### Core Idea
The algorithm uses a single DFS traversal and maintains:
- **Discovery Time (`disc`)**: Time when a node is first visited.
- **Low Value (`low`)**: Earliest discovered vertex reachable from the node's subtree.

A node `u` is an articulation point if removing it disconnects one of its DFS subtrees from the rest of the graph.

---

### Why Does It Work?
Consider a DFS tree rooted at `u`.

If a child `v` cannot reach any ancestor of `u` through a back edge, then the entire subtree rooted at `v` depends on `u` for connectivity.

This condition is detected using:

```text
low[v] >= disc[u]
```

which means:
- The subtree rooted at `v` cannot reach an ancestor of `u`.
- Removing `u` disconnects `v`'s subtree.
Therefore, `u` is an articulation point.

---

### Important Concepts

#### Discovery Time
Each node receives a unique timestamp when first visited.

Example:

```text
disc[0] = 0
disc[1] = 1
disc[2] = 2
...
```

---

#### Low Value
For a node `u`:

```text
low[u] = min(disc[u], low[v] for DFS children, disc[v] for back edges)
```

It represents the earliest discovered vertex reachable from the subtree rooted at `u`.

---

#### DFS Tree
Articulation points are identified using the DFS tree structure.

Two special cases exist:
1. Root node
2. Non-root node

---

### Conditions for Articulation Point
#### Case 1: Root Node
If the DFS root has more than one child:

```text
children > 1
```
then the root is an articulation point.

Why?

Because each child belongs to a separate DFS subtree, and removing the root disconnects them.

---

#### Case 2: Non-Root Node
For a DFS tree edge:

```text
u → v
```

if:

```text
low[v] >= disc[u]
```

then `u` is an articulation point.

Why?

Because the subtree rooted at `v` cannot reach any ancestor of `u`.
Removing `u` disconnects that subtree.

---

### Steps
#### Step 1: Initialize Arrays

```cpp
disc[V] = -1
low[V]
isArticulation[V] = false
```

Maintain:
- DFS timer
- Parent node

---

#### Step 2: Run DFS
For every unvisited node:
- Assign discovery time.
- Compute low values.
- Count DFS children.

---

#### Step 3: Update Low Values
##### Tree Edge
If neighbor is unvisited:

```cpp
dfs(v);
low[u] = min(low[u], low[v]);
```

---

##### Back Edge
If neighbor is already visited and is not the parent:

```cpp
low[u] = min(low[u], disc[v]);
```

---

#### Step 4: Check Articulation Conditions
##### Root Node

```cpp
if (parent == -1 && children > 1)
```

mark as articulation point.

##### Non-Root Node

```cpp
if (parent != -1 && low[v] >= disc[u])
```

mark as articulation point.

---

### Complexity Analysis

| Operation         | Complexity   |
| ----------------- | ------------ |
| DFS Traversal     | O(V + E)     |
| Low Value Updates | O(E)         |
| **Total**         | **O(V + E)** |

#### Space Complexity

```text
O(V)
```

for:
- Discovery array
- Low array
- Recursion stack
- Articulation array
Excluding graph storage.
Including adjacency list storage:

```text
O(V + E)
```

---

### C++ Implementation

```cpp
class ArticulationPoint {
  private:
    int timer = 0;

    void dfs(int node, int parent, vector<vector<int>>& adj, vector<int>& disc, vector<int>& low, vector<int>& isArticulation) {
        disc[node] = low[node] = timer++;
        int children = 0;

        for (int ir : adj[node]) {
            if (neigh == parent)
                continue;
                
            if (disc[it] == -1) {
                children++;
                dfs(it, node, adj, disc, low, isArticulation);

                low[node] = min(low[node], low[it]);

                if (parent != -1 && low[it] >= disc[node]) {
                    isArticulation[node] = 1;
                }
            } else {
                low[node] = min(low[node], disc[it]);
            }
        }

        if (parent == -1 && children > 1) {
            isArticulation[node] = 1;
        }
    }

  public:
    vector<int> getArticulationPoints(int V, vector<vector<int>>& adj) {
        vector<int> disc(V, -1);
        vector<int> low(V);

        vector<int> isArticulation(V, 0);

        for (int i = 0; i < V; i++) {
            if (disc[i] == -1) {
                dfs(i, -1, adj, disc, low, isArticulation);
            }
        }

        vector<int> result;

        for (int i = 0; i < V; i++) {
            if (isArticulation[i]) {
                result.push_back(i);
            }
        }

        return result;
    }
};
```

---

### Example
#### Graph
```text
    0
   / \
  1---2
       \
        3
       / \
      4---5
```

#### Discovery Times
```text
disc[0] = 0
disc[1] = 1
disc[2] = 2
disc[3] = 3
disc[4] = 4
disc[5] = 5
```

#### Low Values
```text
low[0] = 0
low[1] = 0
low[2] = 0
low[3] = 3
low[4] = 3
low[5] = 3
```

#### Articulation Points
```text
{2, 3}
```

Removing:
- `2` disconnects `{3,4,5}`
- `3` disconnects `{4,5}` from the rest

---

### Interview Notes
#### When to Use?
- Network reliability analysis.
- Critical router/server detection.
- Road network connectivity.
- Graph decomposition problems.
- Bridge and biconnected component algorithms.

---

#### Relationship with Tarjan's Algorithm
Both use:

```text
disc[]
low[]
DFS
```

The difference is what we're looking for:

| Problem            | Condition         |
| ------------------ | ----------------- |
| SCC                | disc[u] == low[u] |
| Articulation Point | low[v] >= disc[u] |
| Bridge             | low[v] > disc[u]  |

---

#### Key Observation
For a child `v` of `u`:

```text
low[v] >= disc[u]
```

means the subtree rooted at `v` cannot reach an ancestor of `u`.
Therefore, removing `u` disconnects the graph
This is the most important condition in the algorithm.

---

#### Common Pitfalls
1. Applying the algorithm to directed graphs.
2. Forgetting the special root-node case.
3. Using `>` instead of `>=`.
4. Forgetting to ignore the parent edge.
5. Not running DFS from every unvisited node.
6. Updating low values incorrectly for back edges.

---

#### Memory Trick

```text
disc = when node is discovered
low  = earliest reachable ancestor
```

```text
low[child] >= disc[parent] ⇒ parent is articulation point
```

```text
1. DFS
2. Compute Low Values
3. Check low[child] >= disc[parent]
4. Mark Articulation Point
```

**Articulation Point = DFS + Low-Link + Critical Vertex Detection**