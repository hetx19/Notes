**Problem**: You are given an `n x n` 2D `matrix` representing an image, rotate the image by **90** degrees (clockwise).

You have to rotate the image [**in-place**](https://en.wikipedia.org/wiki/In-place_algorithm), which means you have to modify the input 2D matrix directly. **DO NOT** allocate another 2D matrix and do the rotation.

**Example**:
![[Rotate Image.png]]
**Input:** matrix = `[[1,2,3],[4,5,6],[7,8,9]]`
**Output:** `[[7,4,1],[8,5,2],[9,6,3]]`

[Visit Leetcode](https://leetcode.com/problems/rotate-image/)
[Visit GFG](https://www.geeksforgeeks.org/problems/rotate-by-90-degree-1587115621/1)

---

### Brute Force
By creating a new matrix

**Time Complexity**: O(2n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	void rotate(vector<vector<int>>& matrix) {
		int n = matrix.size();
		
		vector<vector<int>> temp(n, vector<int>(n, 0));
		
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				temp[j][n - i - 1] = matrix[i][j];
			}
		}
		
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < n; j++) {
				matrix[i][j] = temp[i][j];
			}
		}
	}
};
```

---

### Optimal Solution
By Doing transpose and then reversing the rows

**Time Complexity**: O(n<sup>2</sup>)  
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	void transpose(vector<vector<int>>& matrix) {
		int n = matrix.size();
		
		for (int i = 0; i < n; i++) {
			for (int j = i + 1; j < n; j++) {
				swap(matrix[i][j], matrix[j][i]);
			}
		}
	}
	
  public:
	void rotate(vector<vector<int>>& matrix) {
		int n = matrix.size();
		transpose(matrix);
		
		for (int i = 0; i < n; i++) {
			reverse(matrix[i].begin(), matrix[i].end());
		}
	}
};
```

>[!tip]
>For anti-clockwose rotation first reverse the rows and then take transpose

---