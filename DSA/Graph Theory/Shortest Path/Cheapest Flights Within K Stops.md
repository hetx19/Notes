**Problem**: There are `n` cities connected by some number of flights. You are given an array `flights` where `flights[i] = [fromi, toi, pricei]` indicates that there is a flight from city `fromi` to city `toi` with cost `pricei`.

You are also given three integers `src`, `dst`, and `k`, return _**the cheapest price** from_ `src` _to_ `dst` _with at most_ `k` _stops._ If there is no such route, return `-1`.

**Example**:
![[Cheapest Flights Within K Stops.png|195]]
**Input:** n = 4, flights = `[[0,1,100],[1,2,100],[2,0,100],[1,3,600],[2,3,200]]`, src = 0, dst = 3, k = 1
**Output:** 700
**Explanation:**
The graph is shown above.
The optimal path with at most 1 stop from city 0 to 3 is marked in red and has cost 100 + 600 = 700.
Note that the path through cities `[0,1,2,3]` is cheaper but is invalid because it uses 2 stops.

[Visit Leetcode](https://leetcode.com/problems/cheapest-flights-within-k-stops/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/cheapest-flights-within-k-stops/1)

---

### By using BFS + Dijkstra's Algorithm

**Time Complexity**: O(kE) → O(E)
**Space Complexity**: O(n + E)

```cpp
class Solution {
  public:
	int findCheapestPrice(int n, vector<vector<int>>& flights, int src, int dst, int k) {
		int E = flights.size();
		vector<vector<pair<int, int>>> adj(n);
		
		for (auto &flight : flights) {
			adj[flight[0]].push_back({flight[1], flight[2]});
		}
		
		queue<pair<int, pair<int, int>>> q;
		vector<int> distance(n, INT_MAX);
		distance[src] = 0;
		q.push({0, {src, 0}});
		
		while (!q.empty()) {
			int stops = q.front().first;
			int node = q.front().second.first;
			int cost = q.front().second.second;
			q.pop();
			
			if (stops > k) {
				continue;
			}
			
			for (auto it : adj[node]) {
				int adjNode = it.first;
				int edgeWeight = it.second;
				
				if (cost + edgeWeight < distance[adjNode] && stops <= k) {
				// Some platforms shows error. Then remove stops <= k condition
					distance[adjNode] = cost + edgeWeight;
					q.push({stops + 1, {adjNode, distance[adjNode]}});
				}
			}
			
		}
		
		return (distance[dst] == INT_MAX) ? -1 : distance[dst];
	}
};
```