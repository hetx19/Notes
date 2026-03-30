**Problem:** Given a **connected undirected graph** containing **V** vertices represented by a 2-d adjacency list **`adj[][]`**, where each `adj[i]` represents the list of vertices connected to vertex `i`. Perform a **Depth First Search (DFS)** traversal starting from vertex 0, visiting vertices from left to right as per the given adjacency list, and return a list containing the DFS traversal of the graph.

**Note:** Do traverse in the **same order** as they are in the given **adjacency list**.

##### Example: Input: `[[2, 3, 1], [0], [0, 4], [0], [2]]`

![[DFS Example.png|285]]

##### Output: `[0, 2, 4, 3, 1]`

```cpp
void dfsTraversal(vector<vector<int>>& adj, vector<bool>& visited, int node, vector<int>& ans) {
	visited[node] = true;
	ans.push_back(node);

	for (auto it : adjList[node]) {
		if (!visited[it]) {
			dfsTraversal(adj, visited, it, ans);
		}
	}
}

vector<int> dfs(vector<vector<int>>& adj) {
	int v = adj.size();

	vector<bool> visited(v, false);
	vector<int> ans;

	dfsTraversal(adj, visited, 0, ans);

	return ans;
}
```

**Time Complexity:** O(V + 2E)
**Space Complexity:** O(V) + O(V)

