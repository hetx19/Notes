**Problem**: Geek is going for a training program for n days. He can perform any of these activities: **Running**, **Fighting**, and **Learning** Practice. Each activity has some point on each day. As Geek wants to improve all his skills, he can't do the same activity on two consecutive days. Given a 2D matrix **`mat[][]`,** where `mat[i][0]`, `mat[i][1]`, and `mat[i][2]` represent the merit points for **Running**, **Fighting**, and **Learning** on the i<sup>th</sup> day, determine the maximum total merit points Geek can achieve .

**Example**:
**Input**: `mat[][]= [[1, 2, 5], [3, 1, 1], [3, 3, 3]]`
**Output**: 11
**Explanation**: Geek will learn a new move and earn 5 point then on second day he will do running and earn 3 point and on third day he will do fighting and earn 3 points so, maximum merit point will be 11.

[Visit GFG](https://www.geeksforgeeks.org/problems/geeks-training/1)

---
### Brute Force Solution

**Time Complexity**: O(3<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
    int solve(vector<vector<int>>& mat, int day, int last) {
        if (day == 0) {
            int maxi = 0;
            for (int i = 0; i < 3; i++) {
                if (i != last) {
                    maxi = max(maxi, mat[day][i]);
                }
            }
            
            return maxi;
        }
        
        int maxi = 0;
        for (int i = 0; i < 3; i++) {
            if (i != last) {
                int points = mat[day][i] + solve(mat, day - 1, i);
                maxi = max(maxi, points);
            }
        }
        
        return maxi;
    }
    
  public:
    int maximumPoints(vector<vector<int>>& mat) {
        int n = mat.size();
        return solve(mat, n - 1, 3);
    }
};
```

---
### Memoization

**Time Complexity**: O(n * 4 * 3)
**Space Complexity**: O(n * 4) + O(n) 

```cpp
class Solution {
  private:
    int solve(vector<vector<int>>& mat, int day, int last, vector<vector<int>>& dp) {
        if (day == 0) {
            int maxi = 0;
            for (int i = 0; i < 3; i++) {
                if (i != last) {
                    maxi = max(maxi, mat[day][i]);
                }
            }
            
            return dp[day][last] = maxi;
        }
        
        if (dp[day][last] != -1) {
            return dp[day][last];
        }
        
        int maxi = 0;
        for (int i = 0; i < 3; i++) {
            if (i != last) {
                int points = mat[day][i] + solve(mat, day - 1, i, dp);
                maxi = max(maxi, points);
            }
        }
        
        return dp[day][last] =  maxi;
    }
    
  public:
    int maximumPoints(vector<vector<int>>& mat) {
        int n = mat.size();
        vector<vector<int>> dp(n, vector<int> (4, -1));
        
        return solve(mat, n - 1, 3, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(n * 4 * 3)
**Space Complexity**: O(n * 4)

```cpp
class Solution {
  public:
    int maximumPoints(vector<vector<int>>& mat) {
        int n = mat.size();
        vector<vector<int>> dp(n, vector<int> (4, -1));
        
        dp[0][0] = max(mat[0][1], mat[0][2]);
        dp[0][1] = max(mat[0][0], mat[0][2]);
        dp[0][2] = max(mat[0][0], mat[0][1]);
        dp[0][3] = max(mat[0][1], max(mat[0][1], mat[0][2]));
        
        for (int day = 1; day < n; day++) {
            for (int last = 0; last < 4; last++) {
                dp[day][last] = 0;
                
                for (int task = 0; task < 3; task++) {
                    if (task != last) {
                        int points = mat[day][task] + dp[day - 1][task];
                        dp[day][last] = max(dp[day][last], points);
                    }
                }
            }
        }
        
        return dp[n - 1][3];
    }
};
```

---
### Space Optimization

**Time Complexity**: O(n * 4 * 3)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int maximumPoints(vector<vector<int>>& mat) {
        int n = mat.size();
        vector<int> prev(4, 0);
        
        prev[0] = max(mat[0][1], mat[0][2]);
        prev[1] = max(mat[0][0], mat[0][2]);
        prev[2] = max(mat[0][0], mat[0][1]);
        prev[3] = max(mat[0][1], max(mat[0][1], mat[0][2]));
        
        for (int day = 1; day < n; day++) {
            vector<int> current(4, 0);
            for (int last = 0; last < 4; last++) {
                current[last] = 0;
                
                for (int task = 0; task < 3; task++) {
                    if (task != last) {
                        int points = mat[day][task] + prev[task];
                        current[last] = max(current[last], points);
                    } 
                }
            }
            
            prev = current;
        }
        
        return prev[3];
    }
};
```

---