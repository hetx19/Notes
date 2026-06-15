**Problem**: You are in a city that consists of `n` intersections numbered from `0` to `n - 1` with **bi-directional** roads between some intersections. The inputs are generated such that you can reach any intersection from any other intersection and that there is at most one road between any two intersections.

You are given an integer `n` and a 2D integer array `roads` where `roads[i] = [ui, vi, timei]` means that there is a road between intersections `ui` and `vi` that takes `timei` minutes to travel. You want to know in how many ways you can travel from intersection `0` to intersection `n - 1` in the **shortest amount of time**.

Return _the **number of ways** you can arrive at your destination in the **shortest amount of time**_. Since the answer may be large, return it **modulo** `109 + 7`.

**Example**:
![[Number of Ways to Arrive at Destination.png|199]]
**Input:** n = 7, `roads = [[0,6,7],[0,1,2],[1,2,3],[1,3,3],[6,3,3],[3,5,1],[6,5,1],[2,5,1],[0,4,5],[4,6,2]]`
**Output:** 4
**Explanation:** The shortest amount of time it takes to go from intersection 0 to intersection 6 is 7 minutes.
The four ways to get there in 7 minutes are:
- 0 ➝ 6
- 0 ➝ 4 ➝ 6
- 0 ➝ 1 ➝ 2 ➝ 5 ➝ 6
- 0 ➝ 1 ➝ 3 ➝ 5 ➝ 6

[Visit Leetcode](https://leetcode.com/problems/number-of-ways-to-arrive-at-destination/)

---

### By Dijkstra's Algorithm
**Time Complexity**: O(E log n )
**Space Complexity**: O(n) + O(n) + O(n) ➝ O(n)

```cpp
class Solution {
  public:
	int countPaths(int n, vector<vector<int>> &roads) {
		const int MOD = 1e9 + 7;
		vector<vector<pair<int, int>>> adj(n);
		
		for (auto road : roads) {
			adj[road[0]].push_back({road[1], road[2]});
			adj[road[1]].push_back({road[0], road[2]});
		}
		
		priority_queue<pair<long long, int>, vector<pair<long long, int>>, greater<pair<long long, int>>> pq;
		
		vector<long long> distance(n, LLONG_MAX);
		vector<int> ways(n, 0);
		
		distance[0] = 0;
		pq.push({0, 0});
		ways[0] = 1;
		
		while (!pq.empty()) {
			int node = pq.top().second;
			long long dis = pq.top().first;
			pq.pop();
			
			for (auto it : adj[node]) {
				int adjNode = it.first;
				int edgeWeight = it.second;
				
				if (dis + edgeWeight < distance[adjNode]) {
					distance[adjNode] = dis + edgeWeight;
					pq.push({distance[adjNode], adjNode});
					ways[adjNode] = ways[node];
				} else if (dis + edgeWeight == distance[adjNode]) {
					ways[adjNode] = (ways[adjNode] + ways[node]) % MOD;
				}
			}
		}
		
		return ways[n - 1] % MOD;
	}
};
```
