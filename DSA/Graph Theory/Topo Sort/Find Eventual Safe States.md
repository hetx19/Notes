**Problem**: There is a directed graph of `n` nodes with each node labeled from `0` to `n - 1`. The graph is represented by a **0-indexed** 2D integer array `graph` where `graph[i]` is an integer array of nodes adjacent to node `i`, meaning there is an edge from node `i` to each node in `graph[i]`.
A node is a **terminal node** if there are no outgoing edges. A node is a **safe node** if every possible path starting from that node leads to a **terminal node** (or another safe node).
Return *an array containing all the **safe nodes** of the graph*. The answer should be sorted in **ascending** order.

**Example:**
![[Safe States.png|427]]
**Input:** graph = `[[1,2],[2,3],[5],[0],[5],[],[]]`
**Output:** `[2,4,5,6]`
**Explanation:** The given graph is shown above.
Nodes 5 and 6 are terminal nodes as there are no outgoing edges from either of them.
Every path starting at nodes 2, 4, 5, and 6 all lead to either node 5 or 6.

[Vist LeetCode](https://leetcode.com/problems/find-eventual-safe-states/description/)
[Vist_GFG](https://www.geeksforgeeks.org/problems/eventual-safe-states/1)

---

### Method 1). By Cycle Detection

**Time Complexity:** O(V + E)
**Space Complexity:** O(V) + O(V) + O(V) → O(V)

```cpp
class Solution {
  private:
	bool dfsCheck(vector<vector<int>> &adj, vector<bool> &visited, vector<bool> &pathVisited, int node, vector<bool> &check) {
		visited[node] = true;
		pathVisited[node] = true;
		check[node] = false;

		for (int it : adj[node]) {
			if (!visited[it]) {
				if (dfsCheck(adj, visited, pathVisited, it, check)) {
					check[node] = false;
					return true;
				}
			} else if (pathVisited[it]) {
				check[node] = false;
				return true;
			}
		}

		check[node] = true;
		pathVisited[node] = false;
		return false;
	}
	
  public:
	vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
		int V = graph.size();

	    vector<bool> visited(V, false);
	    vector<bool> pathVisited(V, false);
	    vector<bool> check(V, false);

	    vector<int> ans;

	    for (int i = 0; i < V; i++) {
		    if (!visited[i]) {
			    dfsCheck(graph, visited, pathVisited, i, check);
		    }
	    }

	    for (int i = 0; i < V; i++) {
		    if (check[i]) {
			    ans.push_back(i);
		    }
	    }

	    return ans;
	}
};
```

---

### Method 2). By Topological Sort

**Time Complexity:** O(V + E) + O(V log V)
**Space Complexity:** O(V) + O(V) → O(V)

```cpp
class Solution {
  public:
	vector<int> eventualSafeNodes(vector<vector<int>>& graph) {
		int V = graph.size();
		vector<vector<int>> adj(V);
		vector<int> indegree(V, 0);

		for (int i = 0; i < V; i++) {
			for (int it : graph[i]) {
				adj[it].push_back(i);
				indegree[i]++;
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

		sort(ans.begin(), ans.end());

		return ans;
	}
};
```
