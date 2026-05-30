**Problem**: You are given an image represented by an `m x n` grid of integers `image`, where `image[i][j]` represents the pixel value of the image. You are also given three integers `sr`, `sc`, and `color`. Your task is to perform a **flood fill** on the image starting from the pixel `image[sr][sc]`.

To perform a **flood fill**:

1. Begin with the starting pixel and change its color to `color`.
2. Perform the same process for each pixel that is **directly adjacent** (pixels that share a side with the original pixel, either horizontally or vertically) and shares the **same color** as the starting pixel.
3. Keep **repeating** this process by checking neighboring pixels of the *updated* pixels and modifying their color if it matches the original color of the starting pixel.
4. The process **stops** when there are **no more** adjacent pixels of the original color to update.

Return the **modified** image after performing the flood fill.

[Visit Leetcode]()
[Visit GFG]()
 
**Example**:

**Input:** image = `[[1,1,1],[1,1,0],[1,0,1]]`, sr = 1, sc = 1, color = 2
**Output:** `[[2,2,2],[2,2,0],[2,0,1]]`

---

### By DFS traversal
**Time Complexity:** O(5mn) → O(mn)
**Space Complexity:** O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  private:
	void dfs(vector<vector<int>>& image, int row, int col, int initialColor, int deltaRow[], int deltaCol[], vector<vector<int>>& ans, int newColor) {
		int n = image.size(), m = image[0].size();
		ans[row][col] = newColor;

		for (int i = 0; i < 4; i++) {
			int nrow = row + deltaRow[i];
			int ncol = col + deltaCol[i];

			if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && image[nrow][ncol] == initialColor && ans[nrow][ncol] != newColor) {
				dfs(image, nrow, ncol, initialColor, deltaRow, deltaCol, ans, newColor);
			}
		}
	}
	
  public:
	vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
		int initialColor = image[sr][sc];

		if (initialColor == color) {
			return image;
		}
		vector<vector<int>> ans = image;

		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};

		dfs(image, sr, sc, initialColor, deltaRow, deltaCol, ans, color);

		return ans;
	}
};
```

---

### By BFS traversal
**Time Complexity:** O(5mn) → O(mn)
**Space Complexity:** O(mn) + O(mn) → O(mn)

```cpp
class Solution {
  public:
	vector<vector<int>> floodFill(vector<vector<int>>& image, int sr, int sc, int color) {
		int n = image.size(), m = image[0].size();
		int initialColor = image[sr][sc];
		
		if (initialColor == color) {
			return image;
		}
		
		vector<vector<int>> ans = image;
		queue<pair<int, int>> q;
		q.push({sr, sc});
		
		int deltaRow[] = {-1, 0, +1, 0};
		int deltaCol[] = {0, -1, 0, +1};
		
		while (!q.empty()) {
			int row = q.front().first;
			int col = q.front().second;
			q.pop();
			
			ans[row][col] = color;
			
			for (int i = 0; i < 4; i++) {
				int nrow = row + deltaRow[i];
				int ncol = col + deltaCol[i];
				
				if (nrow >= 0 && nrow < n && ncol >= 0 && ncol < m && image[nrow][ncol] == initialColor && ans[nrow][ncol] != color) {
					q.push({nrow, ncol});
				}
			}
		}
		
		return ans;
	}
};
```