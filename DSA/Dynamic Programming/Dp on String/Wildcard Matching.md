**Problem**: Given an input string (`s`) and a pattern (`p`), implement wildcard pattern matching with support for `'?'` and `'*'` where:

- `'?'` Matches any single character.
- `'*'` Matches any sequence of characters (including the empty sequence).

The matching should cover the **entire** input string (not partial).

**Example**:
**Input:** s = "aa", p = "a"
**Output:** false
**Explanation:** "a" does not match the entire string "aa".

[Visit Leetcode]()
[Visit GFG]()

---
### Brute Force Solution
By using recursion - [[Recursion]]

**Time Complexity**: exponential
**Space Complexity**: O(n1 + n2) → Linear

```cpp
class Solution {
  private:
	bool solve(string p, int index1, string s, int index2) {
		if (index1 < 0 && index2 < 0) {
			return true;
		}
		
		if (index1 < 0 && index2 >= 0) {
			return false;
		}
		
		if (index2 < 0 && index1 >= 0) {
			for (int i = 0; i <= index1; i++) {
				if (p[i] != '*') {
					return false;
				}
			}
			
			return true;
		}
		
		if (p[index1] == s[index2] || p[index1] == '?') {
			return solve(p, index1 - 1, s, index2 - 1);
		}
		
		if (p[index1] == '*') {
			return (solve(p, index1 - 1, s, index2) || solve(p, index1, s, index2 - 1));
		}
		
		return false;
	}

  public:
	bool isMatch(string s, string p) {
		int n1 = p.size(), n2 = s.size();
		
		return solve(p, n1 - 1, s, n2 - 1);
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
	bool solve(string p, int index1, string s, int index2, vector<vector<int>>& dp) {
		if (index1 < 0 && index2 < 0) {
			return true;
		}
		
		if (index1 < 0 && index2 >= 0) {
			return false;
		}
		
		if (index2 < 0 && index1 >= 0) {
			for (int i = 0; i <= index1; i++) {
				if (p[i] != '*') {
					return false;
				}
			}
			
			return true;
		}
		
		if (dp[index1][index2] != -1) {
			return dp[index1][index2];
		}
		
		if (p[index1] == s[index2] || p[index1] == '?') {
			return dp[index1][index2] = solve(p, index1 - 1, s, index2 - 1, dp);
		}
		
		if (p[index1] == '*') {
			return dp[index1][index2] = (solve(p, index1 - 1, s, index2, dp) || solve(p, index1, s, index2 - 1, dp));
		}
		
		return dp[index1][index2] = false;
	}

  public:
	bool isMatch(string s, string p) {
		int n1 = p.size(), n2 = s.size();
		vector<vector<int>> dp(n1, vector<int>(n2, -1));
		
		return solve(p, n1 - 1, s, n2 - 1, dp);
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
	bool isMatch(string s, string p) {
		int n1 = p.size(), n2 = s.size();
		vector<vector<bool>> dp(n1 + 1, vector<bool>(n2 + 1, false));
		
		dp[0][0] = true;
		
		for (int index1 = 1; index1 <= n1; index1++) {
			bool flag = true;
			for (int i = 0; i < index1; i++) {
				if (p[i] != '*') {
					flag = false;
					break;
		        }
		    }
		    
		    dp[index1][0] = flag;
		}
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (p[index1 - 1] == s[index2 - 1] || p[index1 - 1] == '?') {
					dp[index1][index2] = dp[index1 - 1][index2 - 1];
				} else if (p[index1 - 1] == '*') {
					dp[index1][index2] = (dp[index1 - 1][index2] || dp[index1][index2 - 1]);
				} else {
					dp[index1][index2] = false;
				}
			}
		}
		
		return dp[n1][n2];
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n1 x n2)
**Space Complexity**: O(2 x n2)

```cpp
class Solution {
  public:
	bool isMatch(string s, string p) {
		int n1 = p.size(), n2 = s.size();
		vector<bool> prev(n2 + 1, false), current(n2 + 1, false);
		
		prev[0] = true;
		
		for (int index1 = 1; index1 <= n1; index1++) {
			bool flag = true;
			for (int i = 0; i < index1; i++) {
				if (p[i] != '*') {
					flag = false;
					break;
		        }
		    }
		    
		    current[0] = flag;
		    
			for (int index2 = 1; index2 <= n2; index2++) {
				if (p[index1 - 1] == s[index2 - 1] || p[index1 - 1] == '?') {
					current[index2] = prev[index2 - 1];
				} else if (p[index1 - 1] == '*') {
					current[index2] = (prev[index2] || current[index2 - 1]);
				} else {
					current[index2] = false;
				}
			}
			prev = current;
		}
		
		return prev[n2];
	}
};
```

---