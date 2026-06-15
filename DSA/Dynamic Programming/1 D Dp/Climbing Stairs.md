**Problem**: You are climbing a staircase. It takes `n` steps to reach the top.

Each time you can either climb `1` or `2` steps. In how many distinct ways can you climb to the top?

**Example**:

**Input**: n = 2
**Output:** 2
**Explanation**: There are two ways to climb to the top.
1. 1 step + 1 step
2. 2 steps

Intution: Fibonacci Sequence

[Visit Leetcode](https://leetcode.com/problems/climbing-stairs/)

---
### Brute Force 
By using Recursion - [[Recursion]]

**Time Complexity**: O(2<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int fibo(int n) {
		if (n <= 1) {
			return 1;
		}
		
		
		
		return fibo(n - 1) + fibo(n - 2);
	}
	
  public:
	int climbStairs(int n) {
		return fibo(n);
	}
};
```

---

### Memoization

**Time Complexity**: O(n)
**Space Complexity**: O(n) + O(n)

```cpp
class Solution {
  private:
    int fibo(int n, vector<int>& dp) {
        if (n <= 1) {
            return dp[n];
        }

        if (dp[n] != -1) {
            return dp[n];
        }

        return dp[n] = fibo(n - 1, dp) + fibo(n - 2, dp);
    }

  public:
    int climbStairs(int n) {
        vector<int> dp(n + 1, -1);
        
        dp[0] = dp[1] = 1;

        return fibo(n, dp);
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
    int climbStairs(int n) {
        vector<int> dp(n + 1, -1);
        dp[0] = dp[1] = 1;
        
        for (int i = 2; i <= n; i++) {
	        dp[i] = dp[i - 1] + dp[i - 2];
        }

        return dp[n];
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
    int climbStairs(int n) {
        int a = 1, b = 1;
        
        for (int i = 2; i <= n; i++) {
	        int c = a + b;
	        a = b;
	        b = c;
        }

        return b;
    }
};
```

---





