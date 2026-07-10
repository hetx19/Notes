**Problem**: You are given an `m x n` integer matrix `matrix` with the following two properties:

- Each row is sorted in non-decreasing order.
- The first integer of each row is greater than the last integer of the previous row.

Given an integer `target`, return `true` _if_ `target` _is in_ `matrix` _or_ `false` _otherwise_.

You must write a solution in `O(log(m * n))` time complexity.

**Example**:
![[Search in a 2D matrix.png|250]]

**Input**: `matrix = [[1,3,5,7],[10,11,16,20],[23,30,34,60]], target = 3`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/search-a-2d-matrix/)
[Visit GFG](https://www.geeksforgeeks.org/problems/search-in-a-matrix-1587115621/1)

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

**Time Complexity**: O(m +  log n) → O(n)
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
			if (row[0] <= target && row[n - 1] >= target) {
				return binarySearch(row, target);
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
		
		int low = 0, high = m * n - 1;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int row = mid / n, col = mid % n;
			
			if (matrix[row][col] == target) {
				return true;
			} else if (matrix[row][col] > target) {
				high = mid - 1;
			} else {
				low = mid + 1;
			}
		}
		
		return false;
	}
};
```