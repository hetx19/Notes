**Problem**: Given an integer array `nums`, return _the length of the longest **strictly increasing**_ _**subsequence**_.

**Example**:
**Input**: `nums = [10,9,2,5,3,7,101,18]`
**Output**: 4
**Explanation:** The longest increasing subsequence is `[2,3,7,101]`, therefore the length is 4.

[Visit Leetcode](https://leetcode.com/problems/longest-increasing-subsequence/)
[Visit GFG](https://www.geeksforgeeks.org/problems/longest-increasing-subsequence-1587115620/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& nums, int n, int index, int prevIndex) {
		if (index > n) {
			return 0;
		}
		
		int len = solve(nums, n, index + 1, prevIndex);
		
		if (prevIndex == 0 || nums[index - 1] > nums[prevIndex - 1]) {
			len = max(1 + solve(nums, n, index + 1, index), len);
		}
		
		return len;
	}
	
  public:
	int lengthOfLIS(vector<int>& nums) {
		int n = nums.size();
		
		return solve(nums, n, 1, 0);
	}
};
```

---
### Memoization

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>) + O(n) → Recursive Stack Space

```cpp
class Solution {
  private:
	int solve(vector<int>& nums, int n, int index, int prevIndex, vector<vector<int>>& dp) {
		if (index > n) {
			return 0;
		}
		
		if (dp[index][prevIndex] != -1) {
			return dp[index][prevIndex];
		}
		
		int len = solve(nums, n, index + 1, prevIndex, dp);
		
		if (prevIndex == 0 || nums[index - 1] > nums[prevIndex - 1]) {
			len = max(1 + solve(nums, n, index + 1, index, dp), len);
		}
		
		return dp[index][prevIndex] = len;
	}
	
  public:
	int lengthOfLIS(vector<int>& nums) {
		int n = nums.size();
		vector<vector<int>> dp(n + 1, vector<int>(n + 1, -1));
		
		return solve(nums, n, 1, 0, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n<sup>2</sup>)

```cpp
class Solution {
  public:
	int lengthOfLIS(vector<int>& nums) {
		int n = nums.size();
		vector<vector<int>> dp(n + 2, vector<int>(n + 1, 0));
		
		for (int index = n; index > 0; index--) {
			for (int prevIndex = 0; prevIndex < index; prevIndex++) {
				int len = dp[index + 1][prevIndex];
				
				if (prevIndex == 0 || nums[index - 1] > nums[prevIndex - 1]) {
					len = max(1 + dp[index + 1][index], len);
				}
				
				dp[index][prevIndex] = len;
			}
		}
		
		return dp[1][0];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int lengthOfLIS(vector<int>& nums) {
		int n = nums.size();
		vector<int> current(n + 1, 0), next(n + 1, 0);
		
		for (int index = n; index > 0; index--) {
			for (int prevIndex = 0; prevIndex < index; prevIndex++) {
				int len = next[prevIndex];
				
				if (prevIndex == 0 || nums[index - 1] > nums[prevIndex - 1]) {
					len = max(1 + next[index], len);
				}
				
				current[prevIndex] = len;
			}
			
			next = current;
		}
		
		return current[0];
	}
};
```

---
### Optimal Approach
By using Lower bound function - [[Lower Bound]]

**Time Complexity**: O(n log n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int lengthOfLIS(vector<int>& nums) {
		vector<int> temp;
		int length = 0;
		
		for (int num : nums) {
			if (temp.empty() || num > temp.back()) {
				temp.push_back(num);
				length++;
			} else {
				temp[lower_bound(temp.begin(), temp.end(), num) - temp.begin()] = num;
			}
		}
		
		return length;
	}
};
```