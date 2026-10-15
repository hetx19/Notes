**Problem**: Given a string `s`, partition `s` such that every substring of the partition is a palindrome.

Return _the **minimum** cuts needed for a palindrome partitioning of_ `s`.

**Example**:
**Input**: s = "aab"
**Output**: 1
**Explanation**: `The palindrome partitioning ["aa","b"] could be produced using 1 cut.`

[Visit Leetcode](https://leetcode.com/problems/palindrome-partitioning-ii/)

---
### Brute Force
By using Recursion - [[Recursion]]

**Time Complexity**: O(n x 2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution { 
  private:
    bool isPalindrome(string s, int start, int end) {
        while (start < end) {
            if (s[start] != s[end]) {
                return false;
            }
            
            start++;
            end--;
        }

        return true;
    }

    int solve(string s, int index) {
        int n = s.size();

        if (index == n) {
            return 0;
        }

        int minPartitions = INT_MAX;

        for (int i = index; i < n; i++) {
            if (isPalindrome(s, index, i)) {
                int partitions = 1 + solve(s, i + 1);
                minPartitions = min(minPartitions, partitions);
            }
        }

        return minPartitions;
    }

  public:
    int minCut(string s) {
        return solve(s, 0) - 1;
    }
};
```

---
### Memoization

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(n) + O(n)

```cpp
class Solution {
private:
    bool isPalindrome(string s, int start, int end) {
        while (start < end) {
            if (s[start] != s[end]) {
                return false;
            }
            
            start++;
            end--;
        }

        return true;
    }

    int solve(string& s, int index, vector<int>& dp) {
        int n = s.size();

        if (index == n) {
            return 0;
        }

        if (dp[index] != -1) {
            return dp[index];
        }

        int minPartitions = INT_MAX;

        for (int i = index; i < n; i++) {
            if (isPalindrome(s, index, i)) {
                int partitions = 1 + solve(s, i + 1, dp);
                minPartitions = min(minPartitions, partitions);
            }
        }

        return dp[index] = minPartitions;
    }

  public:
    int minCut(string s) {
        int n = s.size();
        vector<int> dp(n, -1);

        return solve(s, 0, dp) - 1;
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
	int minCut(string s) {
		int n = s.size();
		vector<vector<bool>> pal(n, vector<bool>(n, false));
		
		for (int i = n - 1; i >= 0; i--) {
			for (int j = i; j < n; j++) {
				if (s[i] == s[j] && (j - i <= 1 || pal[i + 1][j - 1])) {
					pal[i][j] = true;
				}
			}
		}
		
		vector<int> dp(n + 1, 0);
		for (int index = n - 1; index >= 0; index--) {
			int best = INT_MAX;
			for (int i = index; i < n; i++) {
				if (pal[index][i]) {
					best = min(best, 1 + dp[i + 1])
				}
			}
			
			dp[index] = best;
		}
		
		return dp[0] - 1;
	}
};
```

---