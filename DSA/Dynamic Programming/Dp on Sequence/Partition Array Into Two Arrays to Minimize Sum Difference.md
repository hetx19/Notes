**Problem**: Given an array `arr[]`  containing non-negative integers, the task is to divide it into two sets **set1** and **set2** such that the absolute difference between their sums is minimum and find the minimum difference.

**Example**
**Input**: `arr[] = [1, 6, 11, 5]`
**Output**: 1
**Explanation**: 
`Subset1 = {1, 5, 6}, sum of Subset1 = 12`
`Subset2 = {11}, sum of Subset2 = 11`
`Hence, minimum difference is 1`.

[Visit GFG](https://www.geeksforgeeks.org/problems/minimum-sum-partition3317/1)

---
### Brute Force Solution

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
    int solve(vector<int>& arr, int index, int sum1, int totalSum) {
        if (index == arr.size()) {
            int sum2 = totalSum - sum1;
            return abs(max(sum1, sum2) - min(sum1, sum2));
        }

        int take = solve(arr, index + 1, sum1 + arr[index], totalSum);
        int notTake = solve(arr, index + 1, sum1, totalSum);

        return min(take, notTake);
    }

  public:
    int minDifference(vector<int>& arr) {
        int totalSum = accumulate(arr.begin(), arr.end(), 0);
        
        return solve(arr, 0, 0, totalSum);
    }
};
```

---
### Memoization

**Time Complexity**: O(n x totalSum)
**Space Complexity**: O(n x totalSum) + O(n)

```cpp
class Solution {
  private:
    bool solve(vector<int>& arr, int index, int sum, vector<vector<int>>& dp) {
        if (sum == 0) {
            return dp[index][sum] = true;
        }

        if (index == 0) {
            return dp[index][sum] = (arr[0] == sum);
        }

        if (dp[index][sum] != -1) {
            return dp[index][sum];
        }

        bool notTake = solve(arr, index - 1, sum, dp);

        bool take = false;
        if (arr[index] <= sum) {
            take = solve(arr, index - 1, sum - arr[index], dp);
        }

        return dp[index][sum] = (take || notTake);
    }

  public:
    int minDifference(vector<int>& arr) {
        int n = arr.size();
        int totalSum = accumulate(arr.begin(), arr.end(), 0);

        vector<vector<int>> dp(n, vector<int>(totalSum + 1, -1));

        for (int sum = 0; sum <= totalSum / 2; sum++) {
            solve(arr, n - 1, sum, dp);
        }

        int ans = INT_MAX;
        for (int sum = 0; sum <= totalSum / 2; sum++) {
            if (dp[n - 1][sum] == 1) {
                ans = min(ans, totalSum - 2 * sum);
            }
        }

        return ans;
    }
};
```

---
### Tabulation

**Time Complexity**: O(n x totalSum)
**Space Complexity**: O(n x totalSum)

```cpp
class Solution {
  public:
	int minDifference(vector<int>& arr) {
		int n = arr.size();
		int totalSum = accumulate(arr.begin(), arr.end(), 0);
		
		int sum = totalSum;
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

                dp[index][target] = take || notTake;
            }
        }
        
        int mini = INT_MAX;
        for (int sum1 = 0; sum1 <= totalSum / 2; sum1++) {
	        if (dp[n - 1][sum1]) {
		        mini = min(mini, abs((totalSum - sum1) - sum1));
	        }
        }
        
        return mini;
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n x totalSum)
**Space Complexity**: O(totalSum)

```cpp
class Solution {
  public:
	int minDifference(vector<int>& arr) {
		int n = arr.size();
		int totalSum = accumulate(arr.begin(), arr.end(), 0);
		
		int sum = totalSum;
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

                current[target] = take || notTake;
            }
            
            prev = current;
        }
        
        int mini = INT_MAX;
        for (int sum1 = 0; sum1 <= totalSum / 2; sum1++) {
	        if (prev[sum1]) {
		        mini = min(mini, abs((totalSum - sum1) - sum1));
	        }
        }
        
        return mini;
	}
};
```

---