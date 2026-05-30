**Problem**: There are `n` cities numbered from `0` to `n-1`. Given the array `edges` where `edges[i] = [fromi, toi, weighti]` represents a bidirectional and weighted edge between cities `fromi` and `toi`, and given the integer `distanceThreshold`.

Return the city with the smallest number of cities that are reachable through some path and whose distance is **at most** `distanceThreshold`, If there are multiple such cities, return the city with the greatest number.

Notice that the distance of a path connecting cities _**i**_ and _**j**_ is equal to the sum of the edges' weights along that path.

**Example 1**:
![[Find the City With the Smallest Number of Neighbors at a Threshold Distance.png|231]]
**Input**: n = 4, `edges = [[0,1,3],[1,2,1],[1,3,4],[2,3,1]]`, distanceThreshold = 4
**Output**: 3

**Explanation**: The figure above describes the graph. 
The neighboring cities at a distanceThreshold = 4 for each city are:
`City 0 -> [City 1, City 2]` 
`City 1 -> [City 0, City 2, City 3]` 
`City 2 -> [City 0, City 1, City 3]` 
`City 3 -> [City 1, City 2]` 
Cities 0 and 3 have 2 neighboring cities at a distanceThreshold = 4, but we have to return city 3 since it has the greatest number.

[Visit Leetcode](https://leetcode.com/problems/find-the-city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/)
[Visit GFG](https://www.geeksforgeeks.org/problems/city-with-the-smallest-number-of-neighbors-at-a-threshold-distance/1)

---
### By Using Floyd Warshall Algorithm 
**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	int findTheCity(int n, vector<vector<int>>& edges, int distanceThreshold) {
		vector<vector<int>> distance(n, vector<int> (n, INT_MAX));
		
		for (auto &edge : edges) {
			distance[edge[0]][edge[1]] = edge[2];
			distance[edge[1]][edge[0]] = edge[2];
		}
		
		for (int i = 0; i < n; i++) {
			distance[i][i] = 0;
		}
		
		for (int via = 0; via < n; via++) {
			for (int i = 0; i < n; i++) {
				for (int j = 0; j < n; j++) {
					if (distance[i][via] == INT_MAX || distance[via][j] == INT_MAX) {
						continue;
					}
					
					distance[i][j] = min(distance[i][j], distance[i][via] + distance[via][j]);
				}
			}
		}
		
		int cityCount = n;
		int cityNumber = -1; 
		
		for (int city = 0; city < n; city++) {
			int counter = 0;
			for (int adjCity = 0; adjCity < n; adjCity++) {
				if (distance[city][adjCity] <= distanceThreshold) {
					counter++;
				}
			}
			
			if (counter <= cityCount) {
				cityCount = counter;
				cityNumber = city;
			}
		}
		
		return cityNumber;
	}
};
```