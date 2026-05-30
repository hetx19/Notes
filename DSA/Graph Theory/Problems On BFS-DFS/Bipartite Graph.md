**Problem**: There is an **undirected** graph with `n` nodes, where each node is numbered between `0` and `n - 1`. You are given a 2D array `graph`, where `graph[u]` is an array of nodes that node `u` is adjacent to. More formally, for each `v` in `graph[u]`, there is an undirected edge between node `u` and node `v`. The graph has the following properties:

- There are no self-edges (`graph[u]` does not contain `u`).
- There are no parallel edges (`graph[u]` does not contain duplicate values).
- If `v` is in `graph[u]`, then `u` is in `graph[v]` (the graph is undirected).
- The graph may not be connected, meaning there may be two nodes `u` and `v` such that there is no path between them.

A graph is **bipartite** if the nodes can be partitioned into two independent sets `A` and `B` such that **every** edge in the graph connects a node in set `A` and a node in set `B`.

Return `true` _if and only if it is **bipartite**_.

**Example**:
![[Bipartite Graph.png]]
**Input:** graph = `[[1,2,3],[0,2],[0,1,3],[0,2]]`
**Output:** false
**Explanation:** There is no way to partition the nodes into two independent sets such that every edge connects a node in one and a node in the other.

[Visit Leetcode](https://leetcode.com/problems/is-graph-bipartite/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/bipartite-graph/1)

---
### By DFS Traversal

**Time Complexity:** O(V + 2E)
**Space Complexity:** O(V) + O(V) → O(V)

```cpp
class Solution {
  private:
	bool checkComponent(vector<vector<int>> &adj, vector<int> &color, int node, int col) {
		color[node] = col;
		
		for (int it : adj[node]) {
			if (color[it] == -1) {
				if (!checkComponent(adj, color, it, !col)) {
					return false;
				}
			} else if (color[it] == col) {
				return false;
			}
		}
		
		return true;
	}
	
  public:
	bool isBipartite(vector<vector<int>>& graph) {
		int V = graph.size();
		vector<int> color(V, -1);
		
		for (int i = 0; i < V; i++) {
			if (color[i] == -1) {
				if (!checkComponent(graph, color, i, 0)) {
					return false;
				}
			}
		}
		
		
		return true;
	}
};
```

---
### By BFS Traversal

**Time Complexity:** O(V + 2E)
**Space Complexity:** O(V) + O(V) → O(V)

```cpp
class Solution {
  private:
	bool checkComponent(vector<vector<int>> &adj, vector<int> &color, int start) {
		queue<int> q;
		color[start] = 0;
		q.push(start);
		
		while (!q.empty()) {
			int node = q.front();
			q.pop();
			
			for (int it : adj[node]) {
				if (color[it] == -1) {
					color[it] = !color[node];
					q.push(it);
				} else if (color[it] == color[node]) {
					return false;
				}
			}
		}
		
		return true;
	}
	
  public:
	bool isBipartite(vector<vector<int>>& graph) {
		int V = graph.size();
		vector<int> color(V, -1);
		
		for (int i = 0; i < V; i++) {
			if (color[i] == -1) {
				if (!checkComponent(graph, color, i)) {
					return false;
				}
			}
		}
		
		
		return true;
	}
};
```
