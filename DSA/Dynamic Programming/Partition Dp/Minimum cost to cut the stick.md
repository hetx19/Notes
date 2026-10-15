**Problem**: Given a wooden stick of length `n` units. The stick is labelled from `0` to `n`. For example, a stick of length **6** is labelled as follows:

![[Minimum cost to cut the stick.png]]

Given an integer array `cuts` where `cuts[i]` denotes a position you should perform a cut at.

You should perform the cuts in order, you can change the order of the cuts as you wish.

The cost of one cut is the length of the stick to be cut, the total cost is the sum of costs of all cuts. When you cut a stick, it will be split into two smaller sticks (i.e. the sum of their lengths is the length of the stick before the cut). Please refer to the first example for a better explanation.

Return _the minimum total cost_ of the cuts.

**Example**:
![[Minimum cost to cut the stick - 1.png|351]]
**Input:** `n = 7, cuts = [1,3,4,5]`
**Output:** 16
**Explanation:** `Using cuts order = [1, 3, 4, 5] as in the input leads to the following scenario:`
![[Minimum cost to cut the stick - 2.png|285]]
The first cut is done to a rod of length 7 so the cost is 7. The second cut is done to a rod of length 6 (i.e. the second part of the first cut), the third is done to a rod of length 4 and the last cut is to a rod of length 3. The total cost is 7 + 6 + 4 + 3 = 20.
Rearranging the cuts to be `[3, 5, 1, 4]` for example will lead to a scenario with total cost = 16 (as shown in the example photo 7 + 4 + 3 + 2 = 16).

[Visit Leetcode](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/)

---
### Brute Force
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& cuts, int start, int end) {
		if (start > end) {
			return 0;
		}
		
		int mini = INT_MAX;
		
		for (int index = start; index <= end; index++) {
			int cost = (cuts[end + 1] - cuts[start - 1]) + solve(cuts, start, index - 1) + solve(cuts, index + 1, end);
			
			mini = min(mini, cost);
		}
		
		return mini;
	}
	
  public:
	int minCost(int n, vector<int>& cuts) {
		int length = cuts.size();
		cuts.insert(cuts.begin(), 0);
		cuts.push_back(n);
		
		sort(cuts.begin(), cuts.end());
		
		return solve(cuts, 1, length);
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
	int solve(vector<int>& cuts, int start, int end, vector<vector<int>>& dp) {
		if (start > end) {
			return 0;
		}
		
		if (dp[start][end] != -1) {
			return dp[start][end];
		}
		
		int mini = INT_MAX;
		
		for (int index = start; index <= end; index++) {
			int cost = (cuts[end + 1] - cuts[start - 1]) + solve(cuts, start, index - 1, dp) + solve(cuts, index + 1, end, dp);
			
			mini = min(mini, cost);
		}
		
		return dp[start][end] = mini;
	}
	
  public:
	int minCost(int n, vector<int>& cuts) {
		int length = cuts.size();
		cuts.insert(cuts.begin(), 0);
		cuts.push_back(n);
		
		sort(cuts.begin(), cuts.end());
		
		vector<vector<int>> dp(length + 1, vector<int>(length + 1, - 1));
		
		return solve(cuts, 1, length, dp);
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
	int minCost(int n, vector<int>& cuts) {
		int length = cuts.size();
		cuts.insert(cuts.begin(), 0);
		cuts.push_back(n);
		
		sort(cuts.begin(), cuts.end());
		
		vector<vector<int>> dp(length + 2, vector<int>(length + 2, 0));
		
		for (int start = length; start > 0; start--) {
			for (int end = 1; end <= length; end++) {
				if (start > end) {
					continue;
				}
				
				int mini = INT_MAX;
				
				for (int index = start; index <= end; index++) {
					int cost = (cuts[end + 1] - cuts[start - 1]) + dp[start][index - 1] + dp[index + 1][end];
					mini = min(cost, mini);
				}
				
				dp[start][end] = mini;
			}
		}
		
		return dp[1][length];
	}
};
```