### Definition
**Kosaraju's Algorithm** is a graph algorithm used to find all **Strongly Connected Components (SCCs)** in a **directed graph**.

A **Strongly Connected Component** is a maximal group of vertices where every vertex is reachable from every other vertex in the group.

---

### Core Idea
The algorithm performs **two DFS traversals**:
1. Run DFS on the original graph and store nodes according to their finishing times.
2. Reverse all edges of the graph.
3. Process nodes in decreasing order of finishing time on the reversed graph.
4. Each DFS in the reversed graph gives one SCC.

---

### Why Does It Work?
The finishing order obtained from the first DFS ensures that when we start DFS on the reversed graph, we always begin from a component that cannot be reached from any unprocessed SCC.

This guarantees that each DFS traversal on the reversed graph discovers exactly one SCC.

---

### Steps

#### Step 1: DFS on Original Graph
- Visit all nodes.
- After exploring all neighbors of a node, push it onto a stack.

#### Step 2: Reverse the Graph
For every edge:

```text
u → v
```

Create:

```text
v → u
```

#### Step 3: DFS on Reversed Graph
- Pop nodes from the stack.
- If the node is unvisited:
    - Run DFS on the reversed graph.
    - All visited nodes form one SCC.

---

### Complexity Analysis

| Operation     | Complexity   |
| ------------- | ------------ |
| First DFS     | O(V + E)     |
| Reverse Graph | O(E)         |
| Second DFS    | O(V + E)     |
| **Total**     | **O(V + E)** |

#### Space Complexity

```text
O(V + E)
```

for adjacency lists, stack, and visited arrays.

---

## C++ Implementation

```cpp
class Kosaraju { 
  private:
    void dfs1(int node, vector<vector<int>>& adj, vector<int>& visited, stack<int>& st) {
        visited[node] = 1;

        for (int &it : adj[node]) {
            if (!visited[it]) {
                dfs1(it, adj, visited, st);
            }
        }

        st.push(node);
    }

    void dfs2(int node, vector<vector<int>>& revAdj, vector<int>& visited, vector<int>& component) {
        visited[node] = 1;
        component.push_back(node);

        for (int &it : revAdj[node]) {
            if (!visited[it]) {
                dfs2(it, revAdj, visited, component);
            }
        }
    }

public:
    vector<vector<int>> getSCCs(int V, vector<vector<int>>& adj) {
        stack<int> st;
        vector<int> visited(V, 0);

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                dfs1(i, adj, visited, st);
            }
        }

        vector<vector<int>> revAdj(V);

        for (int u = 0; u < V; u++) {
            for (int v : adj[u]) {
                revAdj[v].push_back(u);
            }
        }

        fill(visited.begin(), visited.end(), 0);
        vector<vector<int>> sccs;

        while (!st.empty()) {
            int node = st.top();
            st.pop();

            if (!visited[node]) {
                vector<int> component;
                dfs2(node, revAdj, visited, component);
                sccs.push_back(component);
            }
        }

        return sccs;
    }
};
```

---

### Example

#### Graph

```text
0 → 2
↑   ↓
1 ←

0 → 3 → 4
```

#### SCCs

```text
{0,1,2}
{3}
{4}
```

---

### Interview Notes
### When to Use?
- Finding SCCs in directed graphs.
- Detecting cycles in component graphs.
- Condensation DAG construction.
- Dependency analysis.
- Compiler optimizations.

#### Key Observation
Nodes inside an SCC behave like a single super-node because every node can reach every other node.

After compressing all SCCs into single nodes, the resulting graph is always a **DAG (Directed Acyclic Graph)**.
Kosaraju's algorithm is often used as the first step in constructing the SCC condensation graph.

---

### Common Pitfalls
1. Forgetting to push nodes **after** DFS completion.
2. Reversing edges incorrectly.
3. Not resetting the visited array before the second DFS.
4. Using BFS instead of DFS for finish-time ordering.

---

### Memory Trick

```text
1. DFS → Stack
2. Reverse Graph
3. DFS using Stack Order
4. Each DFS = One SCC
```

**Kosaraju = DFS + Reverse + DFS**

[Visit Leetcode](https://leetcode.com/problems/maximum-number-of-non-overlapping-substrings/)