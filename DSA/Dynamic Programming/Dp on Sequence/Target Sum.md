**Problem**: You are given an integer array `nums` and an integer `target`.

You want to build an **expression** out of nums by adding one of the symbols `'+'` and `'-'` before each integer in nums and then concatenate all the integers.

- For example, if `nums = [2, 1]`, you can add a `'+'` before `2` and a `'-'` before `1` and concatenate them to build the expression `"+2-1"`.

Return the number of different **expressions** that you can build, which evaluates to `target`.

**Example**:
**Input**: `nums = [1,1,1,1,1], target = 3`
**Output**: 5
**Explanation**: There are 5 ways to assign symbols to make the sum of nums be target 3.
-1 + 1 + 1 + 1 + 1 = 3
+1 - 1 + 1 + 1 + 1 = 3
+1 + 1 - 1 + 1 + 1 = 3
+1 + 1 + 1 - 1 + 1 = 3
+1 + 1 + 1 + 1 - 1 = 3

[Visit Leetcode](https://leetcode.com/problems/target-sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/target-sum-1626326450/1)

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
	
    int perfectSum(vector<int>& arr, int target) {
		int n = arr.size();
		return solve(arr, target, n - 1);
    }
    
    int countPartitions(vector<int>& arr, int diff) {
	    int totalSum = accumulate(arr.begin(), arr.end(), 0);
	    
	    if (totalSum - diff < 0 || (totalSum - diff) & 1 != 0) {
	        return 0;
	    }
	    
	    return perfectSum(arr, (totalSum - diff) / 2);
    }
    
    int findTargetSumWays(vector<int>& nums, int target) {
	    return countPartitions(nums, target);
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

    int perfectSum(vector<int>& arr, int target) {
		int n = arr.size();
		vector<vector<int>> dp(n, vector<int>(target + 1, -1));
		return solve(arr, target, n - 1, dp);
    }
    
    int countPartitions(vector<int>& arr, int diff) {
	    int totalSum = accumulate(arr.begin(), arr.end(), 0);
	    
	    if (totalSum - diff < 0 || (totalSum - diff) & 1 != 0) {
	        return 0;
	    }
	    
	    return perfectSum(arr, (totalSum - diff) / 2);
    }
    
  public:
	int findTargetSumWays(vector<int>& nums, int target) {
		return countPartitions(nums, target);
	}
};
```

---
### Tabulation

**Time Complexity**: O(`n x target`)  
**Space Complexity**: O(`n x target`)

```cpp
class Solution {
  private:
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
    
    int countPartitions(vector<int>& arr, int diff) {
	    int totalSum = accumulate(arr.begin(), arr.end(), 0);
	    
	    if (totalSum - diff < 0 || (totalSum - diff) & 1 != 0) {
	        return 0;
	    }
	    
	    return perfectSum(arr, (totalSum - diff) / 2);
    }
    
  public:
	int findTargetSumWays(vector<int>& nums, int target) {
		return countPartitions(nums, target);
	}
};
```

---
### Space Optimization

**Time Complexity**: O(`n x target`)  
**Space Complexity**: O(target)

```cpp
class Solution {
  private:
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
    
    int countPartitions(vector<int>& arr, int diff) {
	    int totalSum = accumulate(arr.begin(), arr.end(), 0);
	    
	    if (totalSum - diff < 0 || (totalSum - diff) & 1 != 0) {
	        return 0;
	    }
	    
	    return perfectSum(arr, (totalSum - diff) / 2);
    }
    
  public:
	int findTargetSumWays(vector<int>& nums, int target) {
		return countPartitions(nums, target);
	}
};
```

---