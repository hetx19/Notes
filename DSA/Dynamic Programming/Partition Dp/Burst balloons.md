**Problem**: You are given `n` balloons, indexed from `0` to `n - 1`. Each balloon is painted with a number on it represented by an array `nums`. You are asked to burst all the balloons.

If you burst the `ith` balloon, you will get `nums[i - 1] * nums[i] * nums[i + 1]` coins. If `i - 1` or `i + 1` goes out of bounds of the array, then treat it as if there is a balloon with a `1` painted on it.

Return _the maximum coins you can collect by bursting the balloons wisely_.

**Example**:

**Input**: `nums = [3,1,5,8]`
**Output**: 167
**Explanation:**
`nums = [3,1,5,8] --> [3,5,8] --> [3,8] --> [8] --> []`
`coins =  3 * 1 * 5 + 3 * 5 * 8 + 1 * 3 * 8 + 1 * 8 * 1 = 167`

[Visit Leetcode]()

---
### Brute Force
By using Recursion - [[Recursion]]

**Time Complexity**: O(3<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& nums, int start, int end) {
		if (start > end) {
			return 0;
		}
		
		int maxCoins = INT_MIN;
		
		for (int index = start; index <= end; index++) {
			int coins = nums[start - 1] * nums[index] * nums[end + 1] + solve(nums, start, index - 1) + solve(nums, index + 1, end);
			
			maxCoins = max(maxCoins, coins);
		}
		
		return maxCoins;
	}
	
  public:
	int maxCoins(vector<int>& nums) {
		int n = nums.size();
		nums.insert(nums.begin(), 1);
		nums.push_back(1);
		
		return solve(nums, 1, n);
	}
};
```

---
### Memoization

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n<sup>2</sup>) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& nums, int start, int end, vector<vector<int>>& dp) {
		if (start > end) {
			return 0;
		}
		
		if (dp[start][end] != -1) {
			return dp[start][end];
		}
		
		int maxCoins = INT_MIN;
		
		for (int index = start; index <= end; index++) {
			int coins = nums[start - 1] * nums[index] * nums[end + 1] + solve(nums, start, index - 1, dp) + solve(nums, index + 1, end, dp);
			
			maxCoins = max(maxCoins, coins);
		}
		
		return dp[start][end] = maxCoins;
	}
	
  public:
	int maxCoins(vector<int>& nums) {
		int n = nums.size();
		nums.insert(nums.begin(), 1);
		nums.push_back(1);
		
		vector<vector<int>> dp(n + 1, vector<int>(n + 1, -1));
		
		return solve(nums, 1, n, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	int maxCoins(vector<int>& nums) {
		int n = nums.size();
		nums.insert(nums.begin(), 1);
		nums.push_back(1);
		
		vector<vector<int>> dp(n + 2, vector<int>(n + 2, 0));
		
		for (int start = n; start > 0; start--) {
			for (int end = 0; end <= n; end++) {
				if (start > end) {
					continue;
				}
				
				int maxCoins = INT_MIN;
				
				for (int index = start; index <= end; index++) {
					int coins = nums[start - 1] * nums[index] * nums[end + 1] + dp[start][index - 1] + dp[index + 1][end];
			
					maxCoins = max(maxCoins, coins);
				}
				
				dp[start][end] = maxCoins;
			}
		}
		
		return dp[1][n];
	}
};
```

---