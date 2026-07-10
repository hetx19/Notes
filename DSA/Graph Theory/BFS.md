**Problem:** Given a **connected undirected graph** containing **V** vertices, represented by a 2-d adjacency list **`adj[][]`**, where each `adj[i]` represents the list of vertices connected to vertex `i`. Perform a **Breadth First Search (BFS)** traversal starting from vertex `0`, visiting vertices from left to right according to the given adjacency list, and return a list containing the BFS traversal of the graph.

**Note:** Do traverse in the **same order** as they are in the given **adjacency list**.

##### Example: Input: `[[1, 3, 1], [0], [0, 4], [0], [2]]`

![[BFS Example.png|295]]

##### Output: `[0, 2, 3, 1, 4]`
---
### By using Queue - [[DSA/STL/Queue|Queue]]

```cpp
class Solution {
  public:
	vector<int> bfs(vector<vector<int>>& adj) {
		int v = adj.size();

		vector<bool> visited(v, false);
		vector<int> ans;
		visited[0] = true;

		queue<int> q;
		q.push(0);

		while (!q.empty()) {
			int node = q.front();
			q.pop();
			ans.push_back(node);

			for (int it : adj[node]) {
				if (!visited[it]) {
					q.push(it);
					visited[it] = true;
				}
			}
		}

		return ans;
	}
};
```

**Time Complexity:** O(V + 2E)
**Space Complexity:** O(V) + O(V) → O(V)

