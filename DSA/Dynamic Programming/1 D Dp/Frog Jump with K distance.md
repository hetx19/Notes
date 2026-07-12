**Problem**: A frog wants to climb a staircase with n steps. Given an integer array heights, where `heights[i]` contains the height of the i<sup>th</sup> step, and an integer k.  

To jump from the i<sup>th</sup> step to the j<sup>th</sup> step, the frog requires **abs(`heights[i] - heights[j]`)** energy, where abs() denotes the absolute difference. The frog can jump from the ith step to any step in the `range [i + 1, i + k]`, provided it exists.

Return the **minimum** amount of energy required by the frog to go from the 0th step to the (n-1)<sup>th</sup> step.

**Example**:
**Input**: `heights = [10, 5, 20, 0, 15], k = 2`
**Output**: 15
**Explanation**:
0th step -> 2nd step, cost = abs(10 - 20) = 10
2nd step -> 4th step, cost = abs(20 - 15) = 5
Total cost = 10 + 5 = 15.

---
### Brute Force Solution
By using Recursion - [Recursion](app://obsidian.md/Recursion)

**Time Complexity**: O(k<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution { 
  private:
    int solve(vector<int>& height, int index, int k) {
        if (index == 0) {
            return 0;
        }

        int minSteps = INT_MAX;

        for (int i = 1; i <= k; i++) {
            if (index - i >= 0) {
                int jump = solve(height, index - i, k) + abs(height[index] - height[index - i]);
                minSteps = min(minSteps, jump);
            }
        }

        return minSteps;
    }

  public:
    int minCost(vector<int>& height, int k) {
        int n = height.size();
        return solve(height, n - 1, k);
    }
};
```

---

### Memoization

**Time Complexity**: O(kn)  
**Space Complexity**: O(n) + O(n) → vector + Recursive Stack Space

```cpp
class Solution {
private:
    int solve(vector<int>& height, int index, int k, vector<int>& dp) {
        if (index == 0) {
            return dp[0] = 0;
        }

        if (dp[index] != -1) {
            return dp[index];
        }

        int minSteps = INT_MAX;

        for (int i = 1; i <= k; i++) {
            if (index - i >= 0) {
                int jump = solve(height, index - i, k, dp) + abs(height[index] - height[index - i]);
                minSteps = min(minSteps, jump);
            }
        }

        return dp[index] = minSteps;
    }

  public:
    int minCost(vector<int>& height, int k) {
        int n = height.size();
        vector<int> dp(n, -1);
        return solve(height, n - 1, k, dp);
    }
};
```

---

### Tabulation

**Time Complexity**: O(kn)  
**Space Complexity**: O(n)

```cpp
class Solution {
public:
    int minCost(vector<int>& height, int k) {
        int n = height.size();
        vector<int> dp(n, 0);

        for (int i = 1; i < n; i++) {
            int minSteps = INT_MAX;

            for (int j = 1; j <= k; j++) {
                if (i - j >= 0) {
                    int jump = dp[i - j] + abs(height[i] - height[i - j]);
                    minSteps = min(minSteps, jump);
                }
            }

            dp[i] = minSteps;
        }

        return dp[n - 1];
    }
};
```

---

### Space Optimization

> [!tip]
> Unlike the classic Frog Jump problem (`k = 2`), each state depends on the previous **k** states. Therefore, we would need to keep the last **k** DP values, resulting in **O(k)** space. Since **k** can be as large as **n**, the worst-case space complexity remains **O(n)**. Hence, the tabulation solution is already asymptotically optimal.


---