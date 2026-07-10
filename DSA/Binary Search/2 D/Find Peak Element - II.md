**Problem**: A **peak** element in a 2D grid is an element that is **strictly greater** than all of its **adjacent** neighbors to the left, right, top, and bottom.

Given a **0-indexed** `m x n` matrix `mat` where **no two adjacent cells are equal**, find **any** peak element `mat[i][j]` and return _the length 2 array_ `[i,j]`.

You may assume that the entire matrix is surrounded by an **outer perimeter** with the value `-1` in each cell.

You must write an algorithm that runs in `O(m log(n))` or `O(n log(m))` time.

**Example**:
![[Find Peak Element - II.png|195]]
**Input**: `mat = [[1,4],[3,2]]`
**Output**: `[0,1]`
**Explanation**: Both 3 and 4 are peak elements so `[1,0]` and `[0,1]` are both acceptable answers.

[Visit Leetcode](https://leetcode.com/problems/find-a-peak-element-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-the-peak-element-in-a-2d-matrix/1)

---
### Optimal Solution

**Time Complexity**: O(n log m)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
    int maxElement(vector<vector<int>>& mat, int col) {
		int m = mat.size(), maxValue = INT_MIN, index = -1;
		
		for (int i = 0; i < m; i++) {
			if (mat[i][col] > maxValue) {
				maxValue = mat[i][col];
				index = i;
			}
		}
		
		return index;
	}
	
  public:
    vector<int> findPeakGrid(vector<vector<int>>& mat) {
        int n = mat[0].size();
        int low = 0, high = n - 1;
    
        while (low <= high) {
            int mid = low + ((high - low) / 2);

        
            int row = maxElement(mat, mid);
            int left = (mid > 0) ? mat[row][mid - 1] : INT_MIN;
            int right = (mid + 1 < n) ? mat[row][mid + 1] : INT_MIN;

            if (mat[row][mid] >= left &&  mat[row][mid] >= right) {
                return {row, mid};
            } else if (right > mat[row][mid]) {
                low = mid + 1;
            } else {
                high = mid - 1;
            }
        }

        return {-1, -1};
    }
};
```

---