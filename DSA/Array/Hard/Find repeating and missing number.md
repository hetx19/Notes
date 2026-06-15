**Problem**: You are given a **0-indexed** 2D integer matrix `grid` of size `n * n` with values in the range `[1, n2]`. Each integer appears **exactly once** except `a` which appears **twice** and `b` which is **missing**. The task is to find the repeating and missing numbers `a` and `b`.

Return _a **0-indexed** integer array_ `ans` _of size_ `2` _where_ `ans[0]` _equals to_ `a` _and_ `ans[1]` _equals to_ `b`_._

**Example**:

**Input**: grid = `[[1,3],[2,2]]`
**Output**: `[2,4]`
**Explanation:** Number 2 is repeated and number 4 is missing so the answer is `[2,4]`.

[Visit Leetcode](https://leetcode.com/problems/find-missing-and-repeated-values/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-missing-and-repeating2512/1)

---

### Brute Force
By doing linear search for all elements

**Time Complexity**: O(n<sup>4</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int countElement(vector<vector<int>>& grid, int target) {
		int count = 0;
		
		for (auto &it : grid) {
			for (int &num : it) {
				if (it == target) {
					count++;
				}
			}
		}
		
		return count;
	}
	
  public:
	vector<int> findMissingAndRepeatedValues(vector<vector<int>>& grid) {
		vector<int> ans(2, 0);
		int n = grid.size();
		int size = n * n;
		
		for (int i = 1; i <= size; i++) {
			int count = countElement(grid, target);
			
			if (count == 0) {
				ans[1] = i;
			} else if (count == 2) {
				ans[0] = i;
			}
		}
		
		return ans;
	}
};
```

---

### Better Solution
By using concept of hashing - [[Hashing]]

**Time Complexity**: O(2n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	vector<int> findMissingAndRepeatedValues(vector<vector<int>>& grid) {
		vector<int> ans(2, 0);
		int n = grid.size();
		int size = n * n;
		
		unordered_map<int, int> mpp;
		
		for (auto &row : grid) {
			for (int &num : row) {
				mpp[num]++;
			}
		}
		
		for (auto &row : grid) {
			for (int &it : row) {
				if (mpp[it] == 0) {
					ans[1] = it;
				} else if (mpp[it] == 2) {
					ans[0] = it;
				}
			}
		}
		
		return ans;
	}
};
```

---

### Optimal Solution
By using Simple Mathematics

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
public:
    vector<int> findMissingAndRepeatedValues(vector<vector<int>>& grid) {
        long long size = grid.size();
        long long n = size * size;
        
        long long expectedSum = n * (n + 1) / 2;
        long long expectedSqSum = n * (n + 1) * (2 * n + 1) / 6;
        
        long long actualSum = 0, actualSqSum = 0;
        
        for (auto &row : grid) {
	        for (int &num : row) {
	            actualSum += num;
	            actualSqSum += 1LL * num * num;
	        }
        }
        
        long long diff = expectedSum - actualSum;
        long long sqDiff = expectedSqSum - actualSqSum;
        
        long long sumMR = sqDiff / diff;
        
        int missing = (diff + sumMR) / 2;
        int repeating = sumMR - missing;

        return {repeating, missing};
    }
};
```

### Alternative Approach
By using XOR

```cpp
class Solution {
  public:
	vector<int> findMissingAndRepeatedValues(vector<vector<int>>& grid) {
		int n = grid.size();
		int size = n * n;
		int xr = 0;

		for(int i = 0; i < size; i++) {
			xr = xr ^ grid[i / n][i % n];
			xr = xr ^ (i + 1);
		}  

		int number = (xr & ~(xr - 1));
		int zero = 0, one = 0;  

		for(int i = 0; i < size; i++) {
			if((grid[i / n][i % n] & number) != 0) {
				one = one ^ grid[i / n][i % n];
			} else {
				zero = zero ^ grid[i / n][i % n];
			}
			
			if(((i + 1) & number) != 0) {
				one = one ^ (i + 1);
			} else {
				zero = zero ^ (i + 1);
			}
		}
		
		int cnt = 0;
		for(int i = 0; i < size; i++) {
			if(grid[i / n][i % n] == zero) {
				cnt++;
			}
		}

		if(cnt == 2) {
			return {zero, one};
		}
		
		return {one, zero};
	}
};
```

---