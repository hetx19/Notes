**Problem**: You are given two strings **_‘s1’_** and **_‘s2’_**.

Return the longest common subsequence of these strings.

If there’s no such string, return an empty string. If there are multiple possible answers, return any such string.

**Note:**
Longest common subsequence of string ‘s1’ and ‘s2’ is the longest subsequence of ‘s1’ that is also a subsequence of ‘s2’. A ‘subsequence’ of ‘s1’ is a string that can be formed by deleting one or more (possibly zero) characters from ‘s1’.

**Example**:

**Input**: `s1 = “abcab”, s2 = “cbab”`
**Output**: `bab`

**Explanation**: `“bab” is one valid longest subsequence present in both strings ‘s1’ , ‘s2’.`

---
### Tabulation
[[Longest Common SubSequence]]

**Time Complexity**: O(n1 * n2) + O(n1 + n2)
**Space Complexity**: O(n1 * n2)

```cpp
class Solution {
  public:
	string longestCommonSubsequence(string text1, string text2) {
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
		
		string ans;
		int length = dp[n1][n2];
		int index = length - 1;
		int i = n1, j = n2;
		
		for (int i = 0; i < length; i++) {
			ans += '$';
		}
		
		while (i > 0 && j > 0) {
			if (text1[i - 1] == text2[j - 1]) {
				s[index] = text1[i - 1];
				index--;
				i--;
				j--;
			} else if (dp[i - 1][j] > dp[i][j - 1]) {
				i--;
			} else {
				j--;
			}
		}
		
		return ans;
	}
};
```

---