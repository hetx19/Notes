**Problem**: Given two strings **s1** and **s2**, determine the length of the **longest substring** that appears in both strings.

**Examples:**

**Input:** s1 = "ABCDGH", s2 = "ACDGHR"
**Output:** 4
**Explanation**: The longest common substring is "CDGH" with a length of 4.

[Visit GFG]()

---
### Tabulation

**Time Complexity**: O(n1 x n2)
**Space Complexity**: O(n1 x n2)

```cpp
class Solution {
  public:
    int longCommSubstr(string& s1, string& s2) {
		int n1 = s1.size(), n2 = s2.size();
		vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
		
		int maxi = 0;
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (s1[index1 - 1] == s2[index2 - 1]) {
					dp[index1][index2] = 1 + dp[index1 - 1][index2 - 1];
					maxi = max(maxi, dp[index1][index2]);
				} else {
					dp[index1][index2] = 0;
				}
			}
		}
		
		return maxi;
	}
};
```

---
### Space Optimization

**Time Complexity**: O(n1 x n2)  
**Space Complexity**: O(n2)

```cpp
class Solution {
  public:
    int longCommSubstr(string& s1, string& s2) {
		int n1 = s1.size(), n2 = s2.size();
		vector<int> prev(n2 + 1, 0), current(n2 + 1, 0);
		
		int maxi = 0;
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (s1[index1 - 1] == s2[index2 - 1]) {
					current[index2] = 1 + prev[index2 - 1];
					maxi = max(maxi, current[index2]);
				} else {
					current[index2] = 0;
				}
			}
			prev = current;
		}
		
		return maxi;
	}
};
```

---