**Problem**: Given an `m x n` integer matrix `matrix`, if an element is `0`, set its entire row and column to `0`'s.

You must do it [in place](https://en.wikipedia.org/wiki/In-place_algorithm).

**Example**:
![[Set matrix to zero.png]]

**Input**: `matrix = [[1,1,1],[1,0,1],[1,1,1]]`
**Output**: `[[1,0,1],[0,0,0],[1,0,1]]`

[Visit Leetcode](https://leetcode.com/problems/set-matrix-zeroes)
[Visit GFG](https://www.geeksforgeeks.org/problems/set-matrix-zeroes/1)

---

### Brute Force
Approach: If found Zero, then change the entire row & col to -1, And the traverse the Matrix and change -1 to 0

**Time Complexity**: O((nm)(n + m) + (nm)) → O(n<sup>3</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	void markRow(vector<vector<int>>& matrix, int row) {
		int m = matrix[0].size();
		
		for (int j = 0; j < m; j++) {
			if (matrix[row][j] != 0) {
				matrix[row][j] = -1;
			}
		}
	}
	
	void markCol(vector<vector<int>>& matrix, int col) {
		int n = matrix.size();
		
		for (int i = 0; i < n; i++) {
			if (matrix[i][col] != 0) {
				matrix[i][col] = -1;
			}
		}
	}
	
  public:
    void setZeroes(vector<vector<int>>& matrix) {
	    int n = matrix.size(), m = matrix[0].size();
	    
	    for (int i = 0; i < n; i++) {
		    for (int j = 0; j < m; j++) {
			    if (matrix[i][j] == 0) {
				    markRow(matrix, i);
				    markCol(matrix, j);
			    }
		    }
	    }
	    
	    for (int i = 0; i < n; i++) {
		    for (int j = 0; j < m; j++) {
			    if (matrix[i][j] == -1) {
				    matrix[i][j] = 0;
			    }
		    }
	    }
    }
};
```

---

### Better Solution
By using concept of Hashing - [[Hashing]]

**Time Complexity**: O(2(mn)) → Optimal Time Compexity
**Space Complexity**: O(m + n)

```cpp
class Solution {
  public:
    void setZeroes(vector<vector<int>>& matrix) {
	    int n = matrix.size(), m = matrix[0].size();
	    
	    vector<int> row(n, 0), col(m, 0);
	    
	    for (int i = 0; i < n; i++) {
		    for (int j = 0; j < m; j++) {
			    if (matrix[i][j] == 0) {
				    row[i] = 1;
				    col[j] = 1;
			    }
		    }
	    }
	    
	    for (int i = 0; i < n; i++) {
		    for (int j = 0; j < m; j++) {
			    if (row[i] == 1 || col[j] == 1) {
				    matrix[i][j] = 0;
			    }
		    }
	    }
    }
};
```

---

### Optimal Solution
Let us optimise the extra space

**Time Complexity**: O(2(mn))
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	void setZeroes(vector<vector<int>>& matrix) {
		int n = matrix.size(), m = matrix[0].size();
		
		int col0 = 1;
		
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (matrix[i][j] == 0) {
					matrix[i][0] = 0;
					if (j != 0) {
						matrix[0][j] = 0;
					} else {
						col0 = 0;
					}
				}
			}
		}
		
		for (int i = 1; i < n; i++) {
			for (int j = 1; j < m; j++) {
				if (matrix[i][0] == 0 || matrix[0][j] == 0) {
					matrix[i][j] = 0;
				}
			}
		}
		
		if (matrix[0][0] == 0) {
			for (int j = 0; j < m; j++) {
				matrix[0][j] = 0;
			}
		}
		
		if (col0 == 0) {
			for (int i = 0; i < n; i++) {
				matrix[i][0] = 0;
			}
		}
	}
};
```

---