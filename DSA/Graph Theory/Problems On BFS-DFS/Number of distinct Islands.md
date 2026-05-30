**Problem**: Given a boolean 2D matrix **grid** of size **n** * **m**. You have to find the number of distinct islands where a group of connected 1s (horizontally or vertically) forms an island. Two islands are considered to be distinct if and only if one island is not equal to another (not rotated or reflected).

**Example**:
**Input:** `grid[][] = [[1, 1, 0, 0, 0], [1, 1, 0, 0, 0], [0, 0, 0, 1, 1], [0, 0, 0, 1, 1]]`
**Output:** 1

[Visit GFG](https://www.geeksforgeeks.org/problems/number-of-distinct-islands/1)

---

**Time Complexity**: O(mn) + O(4mn) → O(mn)
**Space Complexity**: O(mn) + O(mn) + O(mn) → O(mn)

### By DFS Traversal

```cpp
class Solution {
  private:
	void dfs(vector<vector<int>> &grid, vector<vector<bool>> &visited, int row, int col, int deltaRow[], int deltaCol[], vector<pair<int, int>> &list, int r0, int c0, int n, int m) {
		visited[row][col] = true;
		list.push_back({row - r0, col - c0});
		
		for (int i = 0; i < 4; i++) {
			int nrow = row + deltaRow[i];
			int ncol = col + deltaCol[i];
			
			if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !visited[nrow][ncol] && grid[nrow][ncol] == 1) {
				dfs(grid, visited, nrow, ncol, deltaRow, deltaCol, list, r0, c0, n, m);
			}
		}
	}
	
  public:
	int countDistinctIslands(vector<vector<int>>& grid) {
		int n = grid.size(), m = grid[0].size();
        vector<vector<bool>> visited(n, vector<bool>(m, false));
        set<vector<pair<int, int>>> st;
        
        int deltaRow[] = {-1, 0, +1, 0};
        int deltaCol[] = {0, -1, 0, +1};
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
	            if (!visited[i][j] && grid[i][j] == 1) {
		            vector<pair<int, int>> list;
		            dfs(grid, visited, i, j, deltaRow, deltaCol, list, i, j, n, m);
		            st.insert(list);
	            }
            }
        }
        
        return st.size();
	}
};
```

---

### By BFS Traversal

```cpp
class Solution {
  public:
	int countDistinctIslands(vector<vector<int>>& grid) {
		int n = grid.size(), m = grid[0].size();
        vector<vector<bool>> visited(n, vector<bool>(m, false));
        set<vector<pair<int, int>>> st;
        
        int deltaRow[] = {-1, 0, +1, 0};
        int deltaCol[] = {0, -1, 0, +1};
        
        for (int i = 0; i < n; i++) {
            for (int j = 0; j < m; j++) {
	            if (!visited[i][j] && grid[i][j] == 1) {
		            queue<pair<int, int>> q;
		            vector<pair<int, int>> list;
		            
		            int r0 = i, c0 = j;
		            
		            q.push({i, j});
		            visited[i][j] = true;
		            
		            while (!q.empty()) {
			            int row = q.front().first();
			            int col = q.front().second();
			            q.pop();
			            
			            list.push_back({row - r0, col - c0});
			            
			            for (int k = 0; k < 4; k++) {
				            int nrow = row + deltaRow[k];
				            int ncol = col + deltaCol[k];
				            
				            if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && !visited[nrow][ncol] && grid[nrow][ncol] == 1) {
					            visited[nrow][ncol] = true;
					            q.push({nrow, ncol});
					        }
				        }
		            }
		            
		            st.insert(list);
	            }
            }
        }
        
        return st.size();
	}
};
```