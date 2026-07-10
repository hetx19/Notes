**Problem**: Write an efficient algorithm that searches for a value `target` in an `m x n` integer matrix `matrix`. This matrix has the following properties:

- Integers in each row are sorted in ascending from left to right.
- Integers in each column are sorted in ascending from top to bottom.

**Example**:
![[Search in 2D matrix - II.png|195]]

**Input**: `matrix = [[1,4,7,11,15],[2,5,8,12,19],[3,6,9,16,22],[10,13,14,17,24],[18,21,23,26,30]], target = 5`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/search-a-2d-matrix-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/search-in-a-matrix17201720/1)

---

### Brute Force
By using linear search function - [[Linear Search]]

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	bool searchMatrix(vector<vector<int>>& matrix, int target) {
		for (auto &row : matrix) {
			for (int &num : row) {
				if (num == target) {
					return true;
				} else if (num > target) {
					return false;
				}
			}
		}
		
		return false;
	}
};
```

---

### Better Solution
By using Binary Search in every Row - [[Binary Search]]

**Time Complexity**: O(m log n) → O(n log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	bool binarySearch(vector<int> &row, int target) {
		int n = row.size();
		int low = 0, high = n - 1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (row[mid] == target) {
				return true;
			} else if (row[mid] > target) {
				high = mid - 1;
			} else {
				low = mid + 1;
			}
		}
		
		return false;
	}
	
  public:
	bool searchMatrix(vector<vector<int>>& matrix, int target) {
		int n = matrix[0].size();
		
		for (auto &row : matrix) {
			if (binarySearch(row, target)) {
				return true;
			}
		}
		
		return false;
	}
};
```

---

### Optimal Solution
By assuming the 2D matrix into 1D array + Binary Search - [[Binary Search]]

**Time Complexity**: O(log (mn))
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	bool searchMatrix(vector<vector<int>>& matrix, int target) {
		int m = matrix.size(), n = matrix[0].size();
		int top = 0, right = m - 1;
		
		while (top < n && right >= 0) {
			if (matrix[top][right] == target) {
				return true;
			} else if (matrix[top][right] > target) {
				right--;
			} else {
				top++;
			}
		}
		
		return false;
	}
};
```

---