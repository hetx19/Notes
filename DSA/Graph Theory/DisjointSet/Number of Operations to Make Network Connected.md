**Problem**: There are `n` computers numbered from `0` to `n - 1` connected by ethernet cables `connections` forming a network where `connections[i] = [ai, bi]` represents a connection between computers `ai` and `bi`. Any computer can reach any other computer directly or indirectly through the network.

You are given an initial computer network `connections`. You can extract certain cables between two directly connected computers, and place them between any pair of disconnected computers to make them directly connected.

Return _the minimum number of times you need to do this in order to make all the computers connected_. If it is not possible, return `-1`.

**Example**:
![[Number of Operations to Make Network Connected.png|334]]

**Input**: n = 4, connections = `[[0,1],[0,2],[1,2]]`
**Output**: 1
**Explanation**: Remove cable between computer 1 and 2 and place between computers 1 and 3.

[Visit Leetcode](https://leetcode.com/problems/number-of-operations-to-make-network-connected/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/connecting-the-graph/1)

---

### By using DisjointSet - [[DisjointSet Data Structure]]

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class DisjointSet {
  public:
	vector<int> parent, rank;
	
	DisjointSet(int V) {
		parent.resize(V);
		rank.resize(V, 0);
		
		for (int i = 0; i < V; i++) {
			parent[i] = i;
		}
	}
	
	int findParent(int node) {
		if (node == parent[node]) {
			return node;
		}
		
		return parent[node] = findParent(parent[node]);
	}
	
	void unionByRank(int u, int v) {
		int pu = findParent(u), pv = findParent(v);
		
		if (pu == pv) {
			return;
		}
		
		if (rank[pu] < rank[pv]) {
			parent[pu] = pv;
		} else if (rank[pu] > rank[pv]) {
			parent[pv] = pu;
		} else {
			parent[pv] = pu;
			rank[pu]++;
		}
	}
};

class Solution {
  public:
	int makeConnected(int n, vector<vector<int>>& connections) {
		int numberOfEdges = connections.size();
		int cntExtraEdges = 0;
		int numberOfConnectedComponents = 0;
		
		DisjointSet ds(n);
		
		for (auto &e : connections) {
			int u = e[0]; int v = e[1];
			
			if (ds.findParent(u) == ds.findParent(v)) {
				cntExtraEdges++;
			} else {
				ds.unionByRank(u, v);
			}
		}
		
		for (int i = 0; i < n; i++) {
			if (ds.parent[i] == i) {
				numberOfConnectedComponents++;
			}
		}
		 
		int ans = numberOfConnectedComponents - 1;
		
		return ((cntExtraEdges >= ans) ? ans : -1);
	}
};
```