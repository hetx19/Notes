**Problem**: Given two strings `word1` and `word2`, return _the minimum number of operations required to convert `word1` to `word2`_.

You have the following three operations permitted on a word:

- Insert a character
- Delete a character
- Replace a character

**Example**:
**Input**: word1 = "horse", word2 = "ros"
**Output**: 3
**Explanation**: 
horse -> rorse (replace 'h' with 'r')
rorse -> rose (remove 'r')
rose -> ros (remove 'e')

[Visit Leetcode](https://leetcode.com/problems/edit-distance)
[Visit GFG](https://www.geeksforgeeks.org/problems/edit-distance3702/1)

---
### Brute Force Solution
By using recursion - [[Recursion]]

**Time Complexity**: O(3<sup>(n1 + n2)</sup>) → exponential
**Space Complexity**: O((n1 + n2)) → Linear

```cpp
class Solution {
  private:
	int solve(string word1, int index1, string word2, int index2) {
		if (index1 < 0) {
			return index2 + 1;
		}
		
		if (index2 < 0) {
			return index1 + 1;
		}
		
		if (word1[index1] == word2[index2]) {
			return solve(word1, index1 - 1, word2, index2 - 1);
		}
		
		return 1 + min({solve(word1, index1 - 1, word2, index2), solve(word1, index1, word2, index2 - 1), solve(word1, index1 - 1, word2, index2 - 1)});
	}
	
  public:
	int minDistance(string word1, string word2) {
		int n1 = word1.size(), n2 = word2.size();
		
		return solve(word1, n1 - 1, word2, n2 - 1);
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
	int solve(string word1, int index1, string word2, int index2, vector<vector<int>>& dp) {
		if (index1 < 0) {
			return index2 + 1;
		}
		
		if (index2 < 0) {
			return index1 + 1;
		}
		
		if (dp[index1][index2] != -1) {
			return dp[index1][index2];
		}
		
		if (word1[index1] == word2[index2]) {
			return dp[index1][index2] = solve(word1, index1 - 1, word2, index2 - 1, dp);
		}
		
		return dp[index1][index2] = 1 + min({solve(word1, index1 - 1, word2, index2, dp), solve(word1, index1, word2, index2 - 1, dp), solve(word1, index1 - 1, word2, index2 - 1, dp)});
	}
	
  public:
	int minDistance(string word1, string word2) {
		int n1 = word1.size(), n2 = word2.size();
		vector<vector<int>> dp(n1, vector<int>(n2, -1));
		
		return solve(word1, n1 - 1, word2, n2 - 1, dp);
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
	int minDistance(string word1, string word2) {
		int n1 = word1.size(), n2 = word2.size();
		vector<vector<int>> dp(n1 + 1, vector<int>(n2 + 1, 0));
		
		for (int index1 = 0; index1 <= n1; index1++) {
			dp[index1][0] = index1;
		}
		
		for (int index2 = 0; index2 <= n2; index2++) {
			dp[0][index2] = index2;
		}
		
		for (int index1 = 1; index1 <= n1; index1++) {
			for (int index2 = 1; index2 <= n2; index2++) {
				if (word1[index1 - 1] == word2[index2 - 1]) {
					dp[index1][index2] = dp[index1 - 1][index2 - 1];
				} else {
					dp[index1][index2] = 1 + min({dp[index1 - 1][index2 - 1], dp[index1][index2 - 1], dp[index1 - 1][index2]});
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
	int minDistance(string word1, string word2) {
		int n1 = word1.size(), n2 = word2.size();
		vector<int> prev(n2 + 1, 0), current(n2 + 1, 0);
		
		for (int index2 = 0; index2 <= n2; index2++) {
			prev[index2] = index2;
		}
		
		for (int index1 = 1; index1 <= n1; index1++) {
			current[0] = index1;
			for (int index2 = 1; index2 <= n2; index2++) {
				if (word1[index1 - 1] == word2[index2 - 1]) {
					current[index2] = prev[index2 - 1];
				} else {
					current[index2] = 1 + min({prev[index2 - 1], current[index2 - 1], prev[index2]});
				}
			}
			prev = current;
		}
		
		return prev[n2];
	}
};
```

---