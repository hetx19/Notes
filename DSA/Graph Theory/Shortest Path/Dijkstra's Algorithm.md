**Problems**: Given an undirected, weighted graph with **V** vertices numbered from 0 to V-1 and **E** edges, represented by 2d array **edges[][]**, where `edges[i]=[u, v, w]` represents the **edge** between the nodes u and v having w **edge weight**.
You have to find the **shortest distance** of all the vertices from the source vertex **src**, and return an array of integers where the **i<sup>th</sup>** element denotes the shortest distance between **ith** node and source vertex **src**.

**Note:** The Graph is connected and doesn't contain any negative weight edge.  
It is guaranteed that all the shortest distance will fit in a 32-bit integer.

**Example**:
**Input:** V = 3, `edges[][] = [[0, 1, 1], [1, 2, 3], [0, 2, 6]]`, src = 2
**Output:** `[4, 3, 0]`
![[Dijkstra's Algorithm.png|250]]

[Visit GFG](https://www.geeksforgeeks.org/problems/implementing-dijkstra-set-1-adjacency-matrix/1)

---

### By Using Queue Data Structure - [[DSA/STL/Queue|Queue]]
- Not Good

**Time Complexity**: O(E logV)
**Space Complexity**: O(V + E)

```cpp
class Solution {
  public:
    vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        vector<vector<pair<int, int>>> adj(V);
        
        for (auto &e : edges) {
            adj[e[0]].push_back({e[1], e[2]});
            adj[e[1]].push_back({e[0], e[2]});
        }
        
        queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> q;
        vector<int> distance(V, INT_MAX);
        
        distance[src] = 0;
        pq.push({0, src});
        
        while (!q.empty()) {
            int dis = q.front().first;
            int node = q.front().second;
            q.pop();
            
            for (auto it : adj[node]) {
                int edgeWeight = it.second;
                int adjNode = it.first;
                
                if (dis + edgeWeight < distance[adjNode]) {
                    distance[adjNode] = dis + edgeWeight;
                    q.push({distance[adjNode], adjNode});
                }
            }
        }
         
        return distance;
    }
};
```

---

### By Using Set Data Structure - [[Priority Queue]]
- Mostly Commonly Used

**Time Complexity**: O(E logV)
**Space Complexity**: O(V + E)

```cpp
class Solution {
  public:
    vector<int> dijkstra(int V, vector<vector<int>> &edges, int src) {
        vector<vector<pair<int, int>>> adj(V);
        
        for (auto &e : edges) {
            adj[e[0]].push_back({e[1], e[2]});
            adj[e[1]].push_back({e[0], e[2]});
        }
        
        priority_queue<pair<int, int>, vector<pair<int, int>>, greater<pair<int, int>>> pq;
        vector<int> distance(V, INT_MAX);
        
        distance[src] = 0;
        pq.push({0, src});
        
        while (!pq.empty()) {
            int dis = pq.top().first;
            int node = pq.top().second;
            pq.pop();
            
            for (auto it : adj[node]) {
                int edgeWeight = it.second;
                int adjNode = it.first;
                
                if (dis + edgeWeight < distance[adjNode]) {
                    distance[adjNode] = dis + edgeWeight;
                    pq.push({distance[adjNode], adjNode});
                }
            }
        }
         
        return distance;
    }
};
```

---

### By Using Set Data Structure - [[Set]]
- Best Method

**Time Complexity**: O(E logV)
**Space Complexity**: O(V + E)

```cpp
class Solution {
  public:
	vector<int> dijkstra(int V, vector<vector<int>>& edges, int src) {
		vector<vector<pair<int, int>>> adj(V);
		
		for (auto &e : edges) {
			adj[e[0]].push_back({e[1], e[2]});
			adj[e[1]].push_back({e[0], e[2]});
		}
		
		set<pair<int, int>> st;
		vector<int> distance(V, INT_MAX);
		
		distance[src] = 0;
		st.insert({0, src});
		
		while (!st.empty()) {
			auto it = *(st.begin());
			int dis = it.first;
			int node = it.second;
			st.erase(it);
			
			for (auto &it : adj[node]) {
				int edgeWeight = it.second;
                int adjNode = it.first;
                
                if (dis + edgeWeight < distance[adjNode]) {
                    if (distance[adjNode] != INT_MAX) {
                        st.erase({distance[adjNode], adjNode});
                    }
                    distance[adjNode] = dis + edgeWeight; 
                    st.insert({distance[adjNode], adjNode});
                }
			}
		}
		
		return distance;
	}
};
```

---