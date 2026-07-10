**Problem**: You are given a 2D binary array `arr[][]` consisting of only `1`s and `0`s. Each row of the array is sorted in non-decreasing order. Your task is to find and return the index of the first row that contains the maximum number of `1`s. If no such row exists, return `-1`.

**Note**:
- The array follows 0-based indexing.
- The number of rows and columns in the array are denoted by `n`.

**Example**:

**Input**: `arr[][] = [[0,1,1,1], [0,0,1,1], [1,1,1,1], [0,0,0,0]]
**Output**: 2
**Explanation**: Row 2 contains the most number of `1`s (4 `1`s). Hence, the output is `2`.

[Visit GFG](https://www.geeksforgeeks.org/problems/row-with-max-1s0023/1)

---

### Brute Force
By traversing the entire matrix

**Time Complexity**: O(n<sup>2</sup>)
Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int rowWithMax1s(vector<vector<int>> &arr) {
		int n = arr.size();
		int countMax = 0, index = -1;
		
		for (auto &row : arr) {
			int count = 0;
			
			for (int &num : row) {
				count += num;
			}
			
			if (count > countMax) {
				index = i;
				countMax = count;
			}
		}
		
		return index;
    }
};
```

---

### By using Binary Search
By using lower bound - [[Lower Bound]]

**Time Complexity**: (n log n)
**Space Complexity**: (1)

```cpp
class Solution {
  public:
	int rowWithMax1s(vector<vector<int>> &arr) {
		int n = arr.size();
		int countMax = 0, index = -1;
		
		for (int i = 0; i < n; i++) {
			int count = n - (lower_bound(arr[i].begin(), arr[i].end(), 1) - arr[i].begin());
			
			if (count > countMax) {
				index = i;
				countMax = count;
			}
		}
		
		return index;
    }
};
```

---