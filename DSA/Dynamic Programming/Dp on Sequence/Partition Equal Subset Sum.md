**Problem**: Given an integer array `nums`, return `true` _if you can partition the array into two subsets such that the sum of the elements in both subsets is equal or_ `false` _otherwise_.

**Example**:
**Input**: `nums = [1,5,11,5]`
**Output**: true
**Explanation**: The array can be partitioned as `[1, 5, 5]` and `[11]`.

[Visit Leetcode](https://leetcode.com/problems/partition-equal-subset-sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/subset-sum-problem2014/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
    bool solve(vector<int>& arr, int index, int sum) {
        if (sum == 0) {
            return true;
        }

        if (index == 0) {
            return arr[0] == sum;
        }

        bool notPart = solve(arr, index - 1, sum);
        bool part = false;
        
        if (arr[index] <= sum) {
            part = solve(arr, index - 1, sum - arr[index]);
        }

        return part | notPart;
    }
    
    bool isSubsetSum(vector<int>& arr, int sum) {
        int n = arr.size();
        return solve(arr, n - 1, sum);
    }
    
  public:
	bool canPartition(vector<int>& nums) {
		int sum = accumulate(nums.begin(), nums.end(), 0);
		
		if (sum & 1) {
			return false;
		}
		
		return isSubsetSum(nums, sum / 2);
	}
};
```

---
### Memoization

**Time Complexity**: O(sum * n)
**Space Complexity**: O(sum * n) + O(n)

```cpp
class Solution {
  private:
    bool solve(vector<int>& arr, int index, int sum, vector<vector<int>>& dp) {
        if (sum == 0) {
            return dp[index][sum] = true;
        }

        if (index == 0) {
            return dp[index][sum] = arr[0] == sum;
        }

        if (dp[index][sum] != -1) {
            return dp[index][sum];
        }

        bool notPart = solve(arr, index - 1, sum, dp);
        bool part = false;
        
        if (arr[index] <= sum) {
            part = solve(arr, index - 1, sum - arr[index], dp);
        }

        return dp[index][sum] = part | notPart;
    }
    
    bool isSubsetSum(vector<int>& arr, int sum) {
        int n = arr.size();
        vector<vector<int>> dp(n, vector<int> (sum + 1, -1));
        
        return solve(arr, n - 1, sum, dp);
    }
     
  public:
	bool canPartition(vector<int>& nums) {
		int sum = accumulate(nums.begin(), nums.end(), 0);
		
		if (sum & 1) {
			return false;
		}
		
		return isSubsetSum(nums, sum / 2);
	}
};
```

---
### Tabulation

**Time Complexity**: O(sum * n)
**Space Complexity**: O(sum * n)

```cpp
class Solution {
  private:
    bool isSubsetSum(vector<int>& arr, int sum) {
        int n = arr.size();
        vector<vector<bool>> dp(n, vector<bool>(sum + 1, false));

        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }

        if (arr[0] <= sum) {
            dp[0][arr[0]] = true;
        }

        for (int index = 1; index < n; index++) {
            for (int target = 1; target <= sum; target++) {
                bool notTake = dp[index - 1][target];
                bool take = false;

                if (arr[index] <= target) {
                    take = dp[index - 1][target - arr[index]];
                }

                dp[index][target] = take | notTake;
            }
        }

        return dp[n - 1][sum];
    }
    
  public:
	bool canPartition(vector<int>& nums) {
		int sum = accumulate(nums.begin(), nums.end(), 0);
		
		if (sum & 1) {
			return false;
		}
		
		return isSubsetSum(nums, sum / 2);
	}
};
```

---
### Space Optimization

**Time Complexity**: O(sum * n)
**Space Complexity**: O(sum)

```cpp
class Solution {
  private:
    bool isSubsetSum(vector<int>& arr, int sum) {
        int n = arr.size();
        vector<bool> prev(sum + 1, false), current(sum + 1, false);

        current[0] = prev[0] = true;
        prev[arr[0]] = true;

        for (int index = 1; index < n; index++) {
            for (int target = 1; target <= sum; target++) {
                bool notTake = prev[target];
                bool take = false;

                if (arr[index] <= target) {
                    take = prev[target - arr[index]];
                }

                current[target] = take | notTake;
            }
            
            prev = current;
        }

        return prev[sum];
    }
    
  public:
	bool canPartition(vector<int>& nums) {
		int sum = accumulate(nums.begin(), nums.end(), 0);
		
		if (sum & 1) {
			return false;
		}
		
		return isSubsetSum(nums, sum / 2);
	}
};
```

---