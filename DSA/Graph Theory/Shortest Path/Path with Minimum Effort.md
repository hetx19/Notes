**Problem**: You are a hiker preparing for an upcoming hike. You are given `heights`, a 2D array of size `rows x columns`, where `heights[row][col]` represents the height of cell `(row, col)`. You are situated in the top-left cell, `(0, 0)`, and you hope to travel to the bottom-right cell, `(rows-1, columns-1)` (i.e., **0-indexed**). You can move **up**, **down**, **left**, or **right**, and you wish to find a route that requires the minimum **effort**.

A route's **effort** is the **maximum absolute difference** in heights between two consecutive cells of the route.

Return _the minimum **effort** required to travel from the top-left cell to the bottom-right cell._

**Example**:

![185](https://assets.leetcode.com/uploads/2020/10/04/ex1.png)

**Input:** `heights = [[1,2,2],[3,8,2],[5,3,5]]`
**Output:** 2
**Explanation:** The route of `[1,3,5,3,5]` has a maximum absolute difference of 2 in consecutive cells.
This is better than the route of `[1,2,2,2,5]`, where the maximum absolute difference is 3.

[Visit Leetcode](https://leetcode.com/problems/path-with-minimum-effort/description/)

---

###

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class Solution {
  public:
	int minimumEffortPath(vector<vector<int>>& heights) {
		int n = heights.size(), m = heights[0].size();
		vector<vector<int>> distance(n, vector<int>(m, INT_MAX));
		
		priority_queue<pair<int, pair<int, int>>, vector<pair<int, pair<int, int>>>, greater<pair<int, pair<int, int>>>> pq;
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		distance[0][0] = 0;
		pq.push({0, {0, 0}});
		
		while (!pq.empty()) {
			auto it = pq.top();
			pq.pop();
			
			int difference = it.first;
			int row = it.second.first;
			int col = it.second.second;
			
			if (row == n - 1 && col == m - 1) {
				return difference;
			}
			
			for (int i = 0; i < 4; i++) {
				int nrow = row + deltaRow[i];
				int ncol = col + deltaCol[i];
				
				if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m) {
					int newEffort = max(abs(heights[row][col] - heights[nrow][ncol]), difference);
					
					if (newEffort < distance[nrow][ncol]) {
						distance[nrow][ncol] = newEffort;
						pq.push({newEffort, {nrow, ncol}});
					}
				}
			}
		}
		
		return 0;
	}
};
```

---