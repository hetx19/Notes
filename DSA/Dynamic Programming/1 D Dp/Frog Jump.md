**Problem**: Given an integer array **height[]** where `height[i]` represents the height of the **i-th** stair, a frog starts from the **first stair** and wants to reach the **last stair**. From any stair **i**, the frog has two options: it can either jump to the **(i+1)th** stair or the **(i+2)th** stair. The cost of a jump is the absolute difference in height between the two stairs. Determine the **minimum total cost** required for the frog to reach the last stair.

**Example**:
**Input**: `heights[] = [20, 30, 40, 20]`
**Output**: 20
**Explanation**:  Minimum cost is incurred when the frog jumps from stair 0 to 1 then 1 to 3:  
jump from stair 0 to 1: cost = |30 - 20| = 10  
jump from stair 1 to 3: cost = |20 - 30| = 10  
Total Cost = 10 + 10 = 20

[Visit GFG](https://www.geeksforgeeks.org/problems/geek-jump/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& height, int index) {
		if (index == 0) {
			return 0;
		}
		
		int jumpOne = solve(height, index - 1) + abs(height[index] - height[index - 1]);
        int jumpTwo = (index > 1) ? solve(height, index - 2) + abs(height[index] - height[index - 2]) : INT_MAX;
        
        return min(jumpOne, jumpTwo);
	}
	
  public:
    int minCost(vector<int>& height) {
        int n = height.size();
        return solve(height, n - 1);
    }
};
```

---
### Memoization

**Time Complexity**: O(n)
**Space Complexity**: O(n) + O(n) → vector + Recursive Stack Space

```cpp
class Solution {
  private:
	int solve(vector<int>& height, int index, vector<int>& dp) {
		if (index == 0) {
			return dp[index] = 0;
		}
		
		if (dp[index] != -1) {
			return dp[index];
		}
		
		int jumpOne = solve(height, index - 1, dp) + abs(height[index] - height[index - 1]);
        int jumpTwo = (index > 1) ? solve(height, index - 2, dp) + abs(height[index] - height[index - 2]) : INT_MAX;
        
        return dp[index] = min(jumpOne, jumpTwo);
	}
	
  public:
    int minCost(vector<int>& height) {
        int n = height.size();
        vector<int> dp(n, -1);
        
        return solve(height, n - 1, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(n)  
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
    int minCost(vector<int>& height) {
        int n = height.size();
        vector<int> dp(n, 0);
        
        for (int i = 1; i < n; i++) {
            int jumpOne = dp[i - 1] + abs(height[i] - height[i - 1]);
            int jumpTwo = (i > 1) ? dp[i - 2] + abs(height[i] - height[i - 2]) : INT_MAX;
            
            dp[i] = min(jumpOne, jumpTwo);
        }
        
        return dp[n - 1];
    }
};
```

---
### Space Optimization

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int minCost(vector<int>& height) {
        int n = height.size();
        int prev = 0, prev2 = 0;
        
        for (int i = 1; i < n; i++) {
            int jumpOne = prev + abs(height[i] - height[i - 1]);
            int jumpTwo = (i > 1) ? prev2 + abs(height[i] - height[i - 2]) : INT_MAX;
            
            int curr = min(jumpOne, jumpTwo);
            prev2 = prev;
            prev = curr;
        }
        
        return prev;
    }
};
```

---