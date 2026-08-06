**Problem**: Given a string `s`. In one step you can insert any character at any index of the string.

Return _the minimum number of steps_ to make `s` palindrome.

A **Palindrome String** is one that reads the same backward as well as forward.

**Example**:
**Input**: s = "zzazz"
**Output**: 0
**Explanation**: The string "zzazz" is already palindrome we do not need any insertions.

[Visit Leetcode](https://leetcode.com/problems/minimum-insertion-steps-to-make-a-string-palindrome/)
[Visit GFG](https://www.geeksforgeeks.org/problems/form-a-palindrome2544/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2^(n1+n2)) → (exponential)
**Space Complexity**: O(n1 + n2) → (recursion stack)

```cpp
class Solution {
  private:
	int solve(string text1, string text2, int index1, int index2) {
		if (index1 == 0 || index2 == 0) {
			return 0;
		}
		
		if (text1[index1 - 1] == text2[index2 - 1]) {
			return 1 + solve(text1, text2, index1 - 1, index2 - 1);
		}
		
		return max(solve(text1, text2, index1 - 1, index2), solve(text1, text2, index1, index2 - 1));
	}
	
	int longestCommonSubsequence(string text1, string text2) {
		int n1 = text1.size(), n2 = text2.size();
		
		return solve(text1, text2, n1, n2);
	}
	
	int longestPalindromeSubseq(string s) {
		string reversedString = s;
		reverse(reversedString.begin(), reversedString.end());
		return longestCommonSubsequence(s, reversedString);
	}
	
  public:
	int minInsertions(string s) {
		int n = s.size();
		return n - longestPalindromeSubseq(s);
	}
};
```

---
### Memoization

**Time Complexity**: O(n1 * n2)
**Space Complexity**: O(n1 * n2) + O(n1 + n2) → (recursion stack)

```cpp
class Solution {
  private:
	int solve(string text1, string text2, int index1, int index2, vector<vector<int>>& dp) {
		if (index1 == 0 || index2 == 0) {
			return 0;
		}
		
		if (dp[index1][index2] != -1) {
			return dp[index1][index2];
		}
		
		if (text1[index1 - 1] == text2[index2 - 1]) {
			return dp[index1][index2] = 1 + solve(text1, text2, index1 - 1, index2 - 1, dp);
		}
		
		return dp[index1][index2] = max(solve(text1, text2, index1 - 1, index2, dp), solve(text1, text2, index1, index2 - 1, dp));
	}
	
	int longestCommonSubsequence(string text1, string text2) {
		int n1 = text1.size(), n2 = text2.size();
		vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, -1));
		
		return solve(text1, text2, n1, n2, dp);
	}

	int longestPalindromeSubseq(string s) {
		string reversedString = s;
		reverse(reversedString.begin(), reversedString.end());
		return longestCommonSubsequence(s, reversedString);
	}
	
  public:
	int minInsertions(string s) {
		int n = s.size();
		return n - longestPalindromeSubseq(s);
	}
};
```

---
### Tabulation

**Time Complexity**: O(n1 * n2)
**Space Complexity**: O(n1 * n2)

```cpp
class Solution {
  private:
	int longestCommonSubsequence(string text1, string text2) {
		int n1 = text1.size(), n2 = text2.size();
		vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
		
		for (int index2 = 0; index2 <= n2; index2++) {
			dp[0][index2] = 0;
		}
		
		for (int index1 = 0; index1 <= n1; index1++) {
			dp[index1][0] = 0;
		}
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (text1[index1 - 1] == text2[index2 - 1]) {
					dp[index1][index2] = 1 + dp[index1 - 1][index2 - 1];
				} else {
					dp[index1][index2] = max(dp[index1 - 1][index2], dp[index1][index2 - 1]);
				}
			}
		}
		
		return dp[n1][n2];
	}
	
	int longestPalindromeSubseq(string s) {
		string reversedString = s;
		reverse(reversedString.begin(), reversedString.end());
		return longestCommonSubsequence(s, reversedString);
	}
	
  public:
	int minInsertions(string s) {
		int n = s.size();
		return n - longestPalindromeSubseq(s);
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n1 * n2)
**Space Complexity**: O(n2)

```cpp
class Solution {
  private:
	int longestCommonSubsequence(string text1, string text2) {
		int n1 = text1.size(), n2 = text2.size();
		vector<int> prev(n2 + 1, 0), current(n2 + 1, 0);
		
		for (int index2 = 0; index2 <= n2; index2++) {
			prev[index2] = 0;
		}
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (text1[index1 - 1] == text2[index2 - 1]) {
					current[index2] = 1 + prev[index2 - 1];
				} else {
					current[index2] = max(prev[index2], current[index2 - 1]);
				}
			}
			prev = current;
		}
		
		return current[n2];
	}
	
	int longestPalindromeSubseq(string s) {
		string reversedString = s;
		reverse(reversedString.begin(), reversedString.end());
		return longestCommonSubsequence(s, reversedString);
	}
	
  public:
	int minInsertions(string s) {
		int n = s.size();
		return n - longestPalindromeSubseq(s);
	}
};
```

---