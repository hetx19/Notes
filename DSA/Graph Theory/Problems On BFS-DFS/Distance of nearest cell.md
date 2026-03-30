**Problem**: Given an `m x n` binary matrix `mat`, return the distance of the nearest `0` for each cell.

The distance between two cells sharing a common edge is `1`.
Distance = |x<sub>2</sub> - x<sub>1</sub>| + |y<sub>2</sub> - y<sub>1</sub>|

[Vist_leetcode](https://leetcode.com/problems/01-matrix/description/)
[Vist_GFG](https://www.geeksforgeeks.org/problems/distance-of-nearest-cell-having-1-1587115620/1)

For Example:
mat = `[[0, 0, 0], [0, 1, 0], [0, 0, 0]]`
output = `[[0, 0, 0], [0, 1, 0], [0, 0, 0]]`

---

Optimal Approach
**Idea**: By a simple BFS traversal

**Time Complexity:** O(5 _ (m _ n)) → O(m _ n)
**Space Complexity** O(3 _ (m _ n)) → O(m _ n)

```cpp
vector<vector<int>> nearest(vector<vector<int>> &mat) {
	int m = mat.size(), n = mat[0].size();

	vector<vector<int>> distances(m, vector<int>(n, 0));
	vector<vector<bool>> visited(m, vector<bool>(n, false));

	queue<pair<pair<int, int>, int>> q;

	for (int i = 0; i < m; i++) {
		for (int j = 0; j < n; j++) {
			if (mat[i][j] == 0) {
				q.push({{i, j}, 0});
				visited[i][j] = true;
			}
		}
	}

	int deltaRow[] = {-1, 0, +1, 0};
	int deltaCol[] = {0, -1, 0, +1};

	while (!q.empty()) {
		int row = q.front().first.first;
		int col = q.front().first.second;
		int dis = q.front().second;

		q.pop();

		distances[row][col] = dis;

		for (int i = 0; i < 4; i++) {
			int nrow = row + deltaRow[i];
			int ncol = col + deltaCol[i];

			if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n && !visited[nrow][ncol]) {
				q.push({{nrow, ncol}, dis + 1});
				visited[nrow][ncol] = true;
			}
		}
	}

	return distances;
}
```

---

