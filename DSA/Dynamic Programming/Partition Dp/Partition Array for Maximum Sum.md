**Problem**: Given an integer array `arr`, partition the array into (contiguous) subarrays of length **at most** `k`. After partitioning, each subarray has their values changed to become the maximum value of that subarray.

Return _the largest sum of the given array after partitioning. Test cases are generated so that the answer fits in a **32-bit** integer._

**Example**:
**Input**: `arr = [1,15,7,9,2,5,10], k = 3`
**Output**: 84
**Explanation**: `arr becomes [15,15,15,9,10,10,10]`

[Visit Leetcode](https://leetcode.com/problems/partition-array-for-maximum-sum/)

---
### Brute Force
By using Recursion - [[Recursion]]

**Time Complexity**: O(k<sup>n</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& arr, int k, int start) {
		int n = arr.size();
		
		if (start == n) {
			return 0;
		}
		
		int maxSum = 0;
		int maxElement = 0;
		
		for (int length = 1; length <= k && start + length <= n; length++) {
			maxElement = max(maxElement, arr[start + length - 1]);
			
			int currentSum = maxElement * length + solve(arr, k, start + length);
			
			maxSum = max(maxSum, currentSum);
		}
		
		return maxSum;
	}
	
  public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
	    int n = arr.size();
	    
	    return solve(arr, k, 0);
    }
};
```

---
### Memoization

**Time Complexity**: O(nk)
**Space Complexity**: O(n) + O(n)

```cpp
class Solution {
  private:
	int solve(vector<int>& arr, int k, int start, vector<int>& dp) {
		int n = arr.size();
		
		if (start == n) {
			return 0;
		}
		
		if (dp[start] != -1) {
			return dp[start];
		}
		
		int maxSum = 0;
		int maxElement = 0;
		
		for (int length = 1; length <= k && start + length <= n; length++) {
			maxElement = max(maxElement, arr[start + length - 1]);
			
			int currentSum = maxElement * length + solve(arr, k, start + length, dp);
			
			maxSum = max(maxSum, currentSum);
		}
		
		return dp[start] = maxSum;
	}
	
  public:
    int maxSumAfterPartitioning(vector<int>& arr, int k) {
	    int n = arr.size();
	    vector<int> dp(n, -1);
	    
	    return solve(arr, k, 0, dp);
    }
};
```

---
### Tabulation

**Time Complexity**: O(nk)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int maxSumAfterPartitioning(vector<int>& arr, int k) {
		int n = arr.size();
		vector<int> dp(n + 1, 0);
		
		for (int start = n - 1; start >= 0; start--) {
			int maxSum = INT_MIN, maxElement = INT_MIN;
			int length = 0;
			
			for (int index = start; index < min(start + k, n); index++) {
				length++;
				maxElement = max(maxElement, arr[index]);
				
				int sum = (length * maxElement) + dp[index + 1];
				maxSum = max(maxSum, sum);
			}
			
			dp[start] = maxSum;
		}
		
		return dp[0];
	}
};
```

---