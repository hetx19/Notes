**Problem**: Given an `m x n` `matrix`, return _all elements of the_ `matrix` _in spiral order_.

**Example**:
![[Spiral Matrix.png|177]]
**Input**: `matrix = [[1,2,3],[4,5,6],[7,8,9]]`
**Output**: `[1,2,3,6,9,8,7,4,5]`

[Visit Leetcode](https://leetcode.com/problems/spiral-matrix/)
[Visit GFG](https://www.geeksforgeeks.org/problems/spirally-traversing-a-matrix-1587115621/1)

---

### Optimal Solution
By using 4 pointer Approach

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	vector<int> spiralOrder(vector<vector<int>>& matrix) {
		int n = matrix.size(), m = matrix[0].size();
		vector<int> ans;
		
		int left = 0, right = m - 1;
		int top = 0, bottom = n - 1;
		
		while (left <= right && top <= bottom) {
			for (int i = left; i <= right; i++) {
				ans.push_back(matrix[top][i]);
			}
			top++;
			
			for (int i = top; i <= bottom; i++) {
				ans.push_back(matrix[i][right]);
			}
			right--;
			
			if (top <= bottom) {
				for (int i = right; i >= left; i--) {
					ans.push_back(matrix[bottom][i]);
				}
				bottom--;
			}
			
			if (left <= right) {
				for (int i = bottom; i >= top; i--) {
					ans.push_back(matrix[i][left]);
				}
				left++;
			}
		}
		
		return ans;
	}
};
```

---