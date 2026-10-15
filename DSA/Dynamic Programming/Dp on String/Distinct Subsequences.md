**Problem**: Given two strings s and t, return _the number of distinct_ **_subsequences_** _of_ s _which equals_ t.

The test cases are generated so that the answer fits on a 32-bit signed integer.

**Example**:
**Input**: s = "rabbbit", t = "rabbit"
**Output**: 3
**Explanation**: As shown below, there are 3 ways you can generate "rabbit" from s.
**rabb**b**it**`
**ra**b**bbit**`
**rab**b**bit**`

[Visit Leetcode](https://leetcode.com/problems/distinct-subsequences/)
[Visit GFG](https://geeksforgeeks.org/problems/number-of-distinct-subsequences0909/1)

---
### Brute Force Solution
By using recursion - [[Recursion]]

**Time Complexity**: O(2<sup>(n1 + n2)</sup>) → exponential
**Space Complexity**: O((n1 + n2)) → Linear

```cpp
class Solution {
  private:
	int solve(string s, int index1, string t, int index2) {
		if (index2 < 0) {
			return 1;
		}
		
		if (index1 < 0) {
			return 0;
		}
		
		if (s[index1] == t[index2]) {
			return solve(s, index1 - 1, t, index2 - 1) + solve(s, index1 - 1, t, index2);
		}
		
		return solve(s, index1 - 1, t, index2);
	}
	
  public:
	int numDistinct(string s, string t) {
		int n1 = s.size(), n2 = t.size();
		
		return solve(s, n1 - 1, t, n2 - 1);
	}
};
```

---
### Memoization

**Time Complexity**: O(n1 x n2)
**Space Complexity**: O(n1 x n2) + O(n1 + n2) → Recursive Stack Space

```cpp
class Solution {
  private:
	int solve(string s, int index1, string t, int index2, vector<vector<int>>& dp) {
		if (index2 < 0) {
			return 1;
		}
		
		if (index1 < 0) {
			return 0;
		}
		
		if (dp[index1][index2] != -1) {
			return dp[index1][index2];
		}
		
		if (s[index1] == t[index2]) {
			return dp[index1][index2] = solve(s, index1 - 1, t, index2 - 1, dp) + solve(s, index1 - 1, t, index2, dp);
		}
		
		return dp[index1][index2] = solve(s, index1 - 1, t, index2, dp);
	}
	
  public:
	int numDistinct(string s, string t) {
		int n1 = s.size(), n2 = t.size();
		vector<vector<int>> dp(n1, vector<int>(n2, -1));
		
		return solve(s, n1 - 1, t, n2 - 1, dp);
	}
};
```

---
### Tabulation

**Time Complexity**: O(n1 x n2)
**Space Complexity**: O(n1 x n2)

```cpp
class Solution {
  public:
	int numDistinct(string s, string t) {
		int n1 = s.size(), n2 = t.size();
		vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
		
		for (int index1 = 0; index1 <= n1; index1++) {
			dp[index1][0] = 1;
		}
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (s[index1 - 1] == t[index2 - 1]) {
					dp[index1][index2] = dp[index1 - 1][index2 - 1] + dp[index1 - 1][index2];
				} else {
					dp[index1][index2] = dp[index1 - 1][index2];
				}
			}
		}
		
		return dp[n1][n2];
	}
};
```

---
### Space Optimization - I

**Time Complexity**: O(n1 x n2)
**Space Complexity**: O(2 x n2)

```cpp
class Solution {
  public:
	int numDistinct(string s, string t) {
		int n1 = s.size(), n2 = t.size();
		vector<int> prev(n2 + 1, 0), current(n2 + 1, 0);
		
		prev[0] = current[0] = 1;
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (s[index1 - 1] == t[index2 - 1]) {
					current[index2] = prev[index2 - 1] + prev[index2];
				} else {
					current[index2] = prev[index2];
				}
			}
			prev = current;
		}
		
		return prev[n2];
	}
};
```

---
### Space Optimization- II

**Time Complexity**: O(n1 x n2)
**Space Complexity**: O(n2)

```cpp
class Solution {
  public:
	int numDistinct(string s, string t) {
		int n1 = s.size(), n2 = t.size();
		vector<int> dp(n2 + 1, 0);
		
		dp[0] = 1;
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = n2; index2 > 0; index2--) {
				if (s[index1 - 1] == t[index2 - 1]) {
					dp[index2] = dp[index2 - 1] + dp[index2];
				}
			}
		}
		
		return dp[n2];
	}
};
```

---