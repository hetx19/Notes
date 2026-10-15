**Problem**: Given a `m * n` matrix of ones and zeros, return how many **square** submatrices have all ones.

**Example**:

**Input**: `matrix = [[0,1,1,1], [1,1,1,1], [0,1,1,1]]`
**Output**: 15
**Explanation**: 
There are **10** squares of side 1.
There are **4** squares of side 2.
There is  **1** square of side 3.
Total number of squares = 10 + 4 + 1 = **15**.

[Visit Leetcode](https://leetcode.com/problems/count-square-submatrices-with-all-ones/)

---
### Optimal Approach

**Time Complexity**: O(m x n)
**Space Complexity**: O(m x n)

```cpp
class Solution {
  public:
	int countSquares(vector<vector<int>>& matrix) {
		int m = matrix.size(), n = matrix[0].size();
		vector<vector<int>> dp(m + 1, vector<int>(n + 1, 0));
		
		int ans = 0;
		
		for (int i = 0; i < m; i++) {
			dp[i][0] = matrix[i][0];
			ans += dp[i][0];
		}
		
		for (int j = 1; j < n; j++) {
			dp[0][j] = matrix[0][j];
			ans += dp[0][j];
		}
		
		for (int i = 1; i < m; i++) {
			for (int j = 1; j < n; j++) {
				if (matrix[i][j] == 0) {
					dp[i][j] = 0;
				} else {
					dp[i][j] = min({dp[i - 1][j - 1], dp[i][j - 1], dp[i - 1][j]}) + 1;
					ans += dp[i][j];
				}
			}
		}
		
		return ans;
	}
};
```