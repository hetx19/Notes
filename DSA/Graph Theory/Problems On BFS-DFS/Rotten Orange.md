**Problem**: You are given an `m x n` `grid` where each cell can have one of three values:

- `0` representing an empty cell,
- `1` representing a fresh orange, or
- `2` representing a rotten orange.

Every minute, any fresh orange that is **4-directionally adjacent** to a rotten orange becomes rotten.

Return _the minimum number of minutes that must elapse until no cell has a fresh orange_. If _this is impossible, return_ `-1`.

[Vist Leetcode](https://leetcode.com/problems/rotting-oranges/)
[Visit GFG](https://www.geeksforgeeks.org/problems/rotten-oranges2536/1)

**Example:**
**Input:** grid = `[[2,1,1],[1,1,0],[0,1,1]]`
**Output:** 4
![[Rotten Oranges.png]]

---

### By BFS Traversal
**Time Complexity**: O(mn) + O(4mn) → O(mn)
**Space Complexity**: O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  public:
	int orangesRotting(vector<vector<int>>& grid) {
		if (grid.empty()) return 0;
	
		int n = grid.size(), m = grid[0].size();
		
		queue<pair<pair<int, int>, int>> q;
		vector<vector<bool>> visited(n, vector<bool> (m, false));
		
		int cntFresh = 0;
		
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (grid[i][j] == 1) {
					cntFresh++;
				} else if (grid[i][j] == 2) {
					visited[i][j] = true;
					q.push({{i, j}, 0});
				}
			}
		}
		
		int time = 0;
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		while (!q.empty()) {
			int row = q.front().first.first;
			int col = q.front().first.second;
			int t = q.front().second;
			q.pop();
			
			time = max(t, time);
			
			for (int i = 0; i < 4; i++) {
				int nrow = row + deltaRow[i];
				int ncol = col + deltaCol[i];
				
				if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !visited[nrow][ncol] && grid[nrow][ncol] == 1) {
					visited[nrow][ncol] = true;
					cntFresh--;
					q.push({{nrow, ncol}, t + 1});
				}
			}
		}
		
		if (cntFresh > 0) {
			return -1;
		}
		
		return time;
	}
};
```