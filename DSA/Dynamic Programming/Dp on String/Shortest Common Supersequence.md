**Problem**: Given two strings `str1` and `str2`, return _the shortest string that has both_ `str1` _and_ `str2` _as **subsequences**_. If there are multiple valid strings, return **any** of them.

A string `s` is a **subsequence** of string `t` if deleting some number of characters from `t` (possibly `0`) results in the string `s`.

**Example**:
**Input**: str1 = "abac", str2 = "cab"
**Output**: "cabac"
**Explanation**: 
str1 = "abac" is a subsequence of "cabac" because we can delete the first "c".
str2 = "cab" is a subsequence of "cabac" because we can delete the last "ac".
The answer provided is the shortest such string that satisfies these properties.

[Visit Leetcode](https://leetcode.com/problems/shortest-common-supersequence/)
[Visit GFG](https://www.geeksforgeeks.org/problems/shortest-common-supersequence0322/1)

---
### Optimal Solution
By Tabulation

**Time Complexity**: O(n1 x n2) + O(n1 + n2)
**Space Complexity**: O(n1 x n2)

```cpp
class Solution {
  public:
	string shortestCommonSupersequence(string str1, string str2) {
		int n1 = str1.size(), n2 = str2.size();
		vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (str1[index1 - 1] == str2[index2 - 1]) {
					dp[index1][index2] = 1 + dp[index1 - 1][index2 - 1];
				} else {
					dp[index1][index2] = max(dp[index1 - 1][index2], dp[index1][index2 - 1]);
				}
			}
		}
		
		string ans = "";
		int index1 = n1, index2 = n2;
		
		while (index1 > 0 && index2 > 0) {
			if (str1[index1 - 1] == str2[index2 - 1]) {
				ans += str1[index1 - 1];
				index1--;
				index2--;
			} else if (dp[index1 - 1][index2] > dp[index1][index2 - 1]) {
				ans += str1[index1 - 1];
				index1--;
			} else {
				ans += str2[index2 - 1];
				index2--;
			}
		}
		
		while (index1 > 0) {
			ans += str1[index1 - 1];
			index1--;
		}
		
		while (index2 > 0) {
			ans += str2[index2 - 1];
			index2--;
		}
		
		reverse(ans.begin(), ans.end());
		return ans;
	}
};
```

---
