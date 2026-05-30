### Definition

**Topological sorting** is a linear ordering of vertices in a **Directed Acyclic Graph (DAG)** such that for every directed edge **u → v**, vertex **u** comes before **v** in the ordering.
![[Topo Sort.png|186]]
**_Topological Sort_**: `[0, 3, 1, 4, 2]`
There can be multiple valid orderings.

---

### Key Points

- Applicable **only for DAGs** (no cycles).
- Not possible if the graph contains a **cycle**.
- Multiple valid topological orders may exist.

---

### Applications

- Task scheduling (e.g., prerequisites in courses)
- Build systems (compilation order)
- Dependency resolution
- Job scheduling

---

### Implementation

#### By DFS Traversal

```cpp
class Solution {
  private:
	void dfs(vector<vector<int>> &adj, vector<bool> &visited, stack<int> &st, int node) {
		visited[node] = true;

		for (int it : adj[node]) {
			if (!visited[it]) {
				dfs(adj, visited, st, it);
			}
		}

		st.push(node);
	}

  public:
	vector<int> TopoSort(vector<vector<int>> &adj) {
		int V = adj.size();
		vector<bool> visited(V, false);

		stack<int> st;

		for (int i = 0; i < V; i++) {
			if (!visited[i]) {
				dfs(adj, visited, st, i);
			}
		}

		vector<int> ans;

		while (!st.empty()) {
			ans.push_back(st.top());
			st.pop();
		}

		return ans;
	}
};
```

#### By BFS Traversal / Khan's Algorithm

```cpp
class Solution {
  public:
	vector<int> TopoSort(vector<vector<int>> &adj) {
		int V = adj.size();
		vector<int> indegree(V, 0);

		for (int i = 0; i < V; i++) {
			for (int it : adj[i]) {
				indegree[it]++;
			}
		}

		queue<int> q;
		for (int i = 0; i < V; i++) {
			if (indegree[i] == 0) {
				q.push(i);
			}
		}

		vector<int> ans;

		while (!q.empty()) {
			int node = q.front();
			q.pop();

			ans.push_back(node);

			for (int it : adj[node]) {
				indegree[it]--;
				if (indegree[it] == 0) {
					q.push(it);
				}
			}
		}

		return ans;
	}
};
```

**Time Complexity:** O(V + E)
**Space Complexity:** O(V) + O(V) + (V) → O(V)

