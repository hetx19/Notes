**Problem**: Given a **row-wise sorted** matrix **mat[][]** of size n*m, where the number of rows and columns is always **odd**. Return the **median** of the matrix.

**Example**:
**Input**: `mat[][] = [[1, 3, 5], [2, 6, 9], [3, 6, 9]]`
**Output**: 5
**Explanation**: Sorting matrix elements gives us `[1, 2, 3, 3, 5, 6, 6, 9, 9]`. Hence, 5 is median.

[Visit GFG](https://www.geeksforgeeks.org/problems/median-in-a-row-wise-sorted-matrix1527/1)

---

### Brute Force
By converting 2D matrix into 1D array

**Time Complexity**: O(nm) * O(log(nm))
**Space Complexity**: O(nm)

```cpp
class Solution {
  private:
	vector<int> convertIn1D(vector<vector<int>> &matrix) {
		vector<int> oneD;
		
		for (auto &row : matrix) {
			for (int &num : row) {
				oneD.push_back(num);
			}
		}
		
		return oneD;
	}
	
  public:
	int median(vector<vector<int>> &matrix) {
		vector<int> oneD = convertIn1D(matrix);
		sort(oneD.begin(), oneD.end());
		
		int n = oneD.size();
		
		return oneD[n / 2];
    }
};
```

---
### Optimal Solution
By using upper bound function - [[Upper Bound]]

**Time Complexity**: O()
**Space Complexity**: O()

```cpp
class Solution {
  private:
	int countLessEqual(vector<int> &arr, int target) {
		return upper_bound(arr.begin(), arr.end(), target) - arr.begin();
	}
	
  public:
	int median(vector<vector<int>> &matrix) {
		int n = matrix.size(), m = matrix[0].size();
		int low = matrix[0][0], high = matrix[0][m - 1];
		
		for (int i = 0; i < n; i++) {
			low = min(low, matrix[i][0]);
			high = max(high, matrix[i][m - 1]);
		}
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int count = 0;
			
			for (int i = 0; i < n; i++) {
				count += countLessEqual(matrix[i], mid);
			}
			
			if (count < (n * m + 1) / 2) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return low;
    }
};
```

---