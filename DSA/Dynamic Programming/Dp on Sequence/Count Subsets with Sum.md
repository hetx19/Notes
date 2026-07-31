**Problem**: Given an array **`arr`** of non-negative integers and an integer **`target`**, the task is to count all subsets of the array whose sum is equal to the given `target`.

**Example**:
**Input**: `arr[] = [5, 2, 3, 10, 6, 8], target = 10`
**Output**: 3
**Explanation**: The subsets {5, 2, 3}, {2, 8}, and {10} sum up to the target 10.

[Visit GFG](https://www.geeksforgeeks.org/problems/perfect-sum-problem5633/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& arr, int target, int index) {
		if (index == 0) {
            if (target == 0 && arr[0] == 0) {
                return 2;
            }
            
            if (target == 0 || arr[0] == target) {
                return 1;
            }
            
            return 0;
        }
		
		int pick = (arr[index] <= target) ? solve(arr, target - arr[index], index - 1) : 0;
		int notPick = solve(arr, target, index - 1);
		
		return pick + notPick;
	}
	
  public:
    int perfectSum(vector<int>& arr, int target) {
		int n = arr.size();
		return solve(arr, target, n - 1);
    }
};
```

---
### Memoization

**Time Complexity**: O(`n x target`)
**Space Complexity**: O(`n x target`) + O(n)

```cpp
class Solution {
  private:
    int solve(vector<int>& arr, int target, int index, vector<vector<int>>& dp) {
        if (index == 0) {
            if (target == 0 && arr[0] == 0) {
                return 2;
            }
            
            if (target == 0 || arr[0] == target) {
                return 1;
            }
            
            return 0;
        }

        if (dp[index][target] != -1) {
            return dp[index][target];
        }

        int notPick = solve(arr, target, index - 1, dp);
        int pick = (arr[index] <= target) ? solve(arr, target - arr[index], index - 1, dp) : 0;

        return dp[index][target] = (pick + notPick);
    }
	
  public:
    int perfectSum(vector<int>& arr, int target) {
		int n = arr.size();
		vector<vector<int>> dp(n, vector<int>(target + 1, -1));
		return solve(arr, target, n - 1, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(`n x target`)
**Space Complexity**: O(`n x target`)

```cpp
class Solution { 
  public:
    int perfectSum(vector<int>& arr, int target) {
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int>(target + 1, 0));

        dp[0][0] = (arr[0] == 0) ? 2 : 1;

        if (arr[0] != 0 && arr[0] <= target) {
            dp[0][arr[0]] = 1;
        }

        for (int index = 1; index < n; index++) {
            for (int t = 0; t <= target; t++) {
                int notPick = dp[index - 1][t];

                int pick = 0;
                if (arr[index] <= t) {
                    pick = dp[index - 1][t - arr[index]];
                }

                dp[index][t] = pick + notPick;
            }
        }

        return dp[n - 1][target];
    }
};
```

---
### Space Optimization

**Time Complexity**: O(`n x target`)
**Space Complexity**: O(`target`)

```cpp
class Solution {
  public:
    int perfectSum(vector<int>& arr, int target) {
        int n = arr.size();
        vector<int> prev(target + 1, 0), current(target + 1, 0);

        prev[0] = (arr[0] == 0) ? 2 : 1;

        if (arr[0] != 0 && arr[0] <= target) {
            prev[arr[0]] = 1;
        }

        for (int index = 1; index < n; index++) {
            for (int t = 0; t <= target; t++) {
                int notPick = prev[t];

                int pick = 0;
                if (arr[index] <= t) {
                    pick = prev[t - arr[index]];
                }

                current[t] = pick + notPick;
            }
            prev = current;
        }

        return prev[target];
    }
};
```

---