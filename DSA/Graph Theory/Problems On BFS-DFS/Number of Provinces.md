**Problem**: There are `n` cities. Some of them are connected, while some are not. If city `a` is connected directly with city `b`, and city `b` is connected directly with city `c`, then city `a` is connected indirectly with city `c`.

A **province** is a group of directly or indirectly connected cities and no other cities outside of the group.

You are given an `n x n` matrix `isConnected` where `isConnected[i][j] = 1` if the `ith` city and the `jth` city are directly connected, and `isConnected[i][j] = 0` otherwise.

Return \*the total number of **provinces\***.

**Example**:
![[Number of provinces.png|218]]

**Input:** isConnected = `[[1,1,0],[1,1,0],[0,0,1]]`
**Output:** 2

[Vist Leetcode](https://leetcode.com/problems/number-of-provinces/description/)
[Vist GFG](https://www.geeksforgeeks.org/problems/number-of-provinces/1)

---

### By DFS traversal

```cpp
class Solution {
  private:
    void DFS(vector<vector<int>> &adjLs, vector<bool> &visited, int node) {
        visited[node] = true;

        for (int it : adjLs[node]) {
            if (!visited[it]) {
                DFS(adjLs, visited, it);
            }
        }
    }

  public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int V = isConnected.size();
        vector<vector<int>> adjLs(V);

        for (int i = 0; i < V; i++) {
            for (int j = i + 1; j < V; j++) {
                if (isConnected[i][j] == 1) {
                    adjLs[i].push_back(j);
                    adjLs[j].push_back(i);
                }
            }
        }

        int provinces = 0;
        vector<bool> visited(V, false);

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                provinces++;
                DFS(adjLs, visited, i);
            }
        }

        return provinces;
    }
};
```

**Time Complexity:** O(V<sup>2</sup> + V + 2E) → O(V<sup>2</sup>)
**Space Complexity:** O(V + 2E) + O(V) + O(V) → O(V + 2E)

---

### By BFS traversal

```cpp
class Solution {
  private:
    void BFS(vector<vector<int>> &adjLs, vector<bool> &visited, int start) {
        visited[start] = true;
        queue<int> q;
        q.push(start);

        while (!q.empty()) {
	        int node = q.front();
            q.pop();

            for (int it : adjLs[node]) {
                if (!visited[it]) {
                    visited[it] = true;
                    q.push(it);
                }
            }
        }
    }

  public:
    int findCircleNum(vector<vector<int>>& isConnected) {
        int V = isConnected.size();
        vector<vector<int>> adjLs(V);

        for (int i = 0; i < V; i++) {
            for (int j = i + 1; j < V; j++) {
                if (isConnected[i][j] == 1) {
                    adjLs[i].push_back(j);
                    adjLs[j].push_back(i);
                }
            }
        }

        int provinces = 0;
        vector<bool> visited(V, false);

        for (int i = 0; i < V; i++) {
            if (!visited[i]) {
                provinces++;
                BFS(adjLs, visited, i);
            }
        }

        return provinces;
    }
};
```

**Time Complexity:** O(V<sup>2</sup> + V + 2E) → O(V<sup>2</sup>)
**Space Complexity:** O(V + 2E) + O(V) + O(V) → O(V + 2E)

---

>[!tip]
>Matrix is given so we can improve space complexity by implementing BFS/DFS using the adjacency Matrix.

---

### By using DisjointSet - [[DisjointSet Data Structure]]

**Time Complexity**: O(V<sup>2</sup>)
**Space Complexity**: O(V<sup>2</sup>)

```cpp
class DisjointSet {
  private:
	vector<int> parent, size;
	
  public:
	DisjointSet(int V) {
		parent.resize(V);
		size.resize(V, 1);

		for (int i = 0; i < V; i++) {
			parent[i] = i;
		}
	}
	
	int getParent(int node) {
		return parent[node];
	}
	
	int findParent(int node) {
		if (node == parent[node]) {
			return node;
		}
		
		return parent[node] = findParent(parent[node]);
	}

  

	void unionBySize(int u, int v) {
		int pu = findParent(u), pv = findParent(v);

		if (pu == pv) {
			return;
		}
		
		if (size[pu] < size[pv]) {
			parent[pu] = pv;
			size[pv] += pu;
		} else {
			parent[pv] = pu;
			size[pu] += pv;
		}
	}
};

class Solution {
  public:
	int findCircleNum(vector<vector<int>>& isConnected) {
		int V = isConnected.size();
		DisjointSet ds(V);

		for (int i = 0; i < V; i++) {
			for (int j = 0; j < V; j++) {
				if (isConnected[i][j] == 1) {
					ds.unionBySize(i, j);
				}
			}
		}
		
		int ans = 0;
		
		for (int i = 0; i < V; i++) {
			if (i == ds.getParent(i)) {
				ans++;
			}
		}
		
		return ans;
	}
};
```