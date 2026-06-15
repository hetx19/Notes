**Problem**: Given an integer array `nums`, find the subarray with the largest sum, and return _its sum_.

**Example**:
**Input**: `nums = [-2,1,-3,4,-1,2,1,-5,4]`
**Output**: 6
**Explanation**: The subarray `[4,-1,2,1]` has the largest sum 6.

[Visit Leetcode](https://leetcode.com/problems/maximum-subarray/)
[Visit GFG](https://www.geeksforgeeks.org/problems/kadanes-algorithm-1587115620/1)

---
### Brute Force Approach
By generating all the subarrays

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxSubArray(vector<int>& nums) {
		int n = nums.size();
		int maxSum = INT_MIN;
		
		for (int i = 0; i < n; i++) {
			for (int j = i; j < n; j++) {
				int sum = 0;
				for (int k = i; k <= j; k++) {
					sum += nums[k];
				}
				maxSum = max(maxSum, sum);
			}
		}
		
		return maxSum;
	}
};
```

---

### Better Approach
Betterment in brute force solution → Still Brute Force

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxSubArray(vector<int>& nums) {
		int n = nums.size();
		int maxSum = INT_MIN;
		
		for (int i = 0; i < n; i++) {
			int sum = 0;
			for (int j = i; j < n; j++) {
				sum += nums[j];
			    maxSum = max(maxSum, sum);
			}
		}
		
		return maxSum;
	}
};
```

---

### Optimal Approach
By using `Kadane's Algorithm`

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int maxSubArray(vector<int> &arr) {
        int maxSum = INT_MIN;
        int sum = 0;
        
        for (int num : arr) {
            sum += num;
            maxSum = max(maxSum, sum);
            
            if(sum < 0) {
                sum = 0;
            }
        }
        
        return maxSum;
    }
};
```

>[!tip]
if the question asked to print /  return the subarray then

**Time Complexity**: O(n) + O(k)
**Space Complexity**: O(k)

```cpp
class Solution {
  public:
    vector<int> maxSubArray(vector<int> &arr) {
        int maxSum = INT_MIN;
        int sum = 0, n = nums.size();
        int ansStart = -1, ansEnd = -1;
        int start = -1, end = -1;
        
        for (int i = 0; i < n; i++) {
	        if (sum == 0) {
		        start = i;
		    }
		    
		    sum += nums[i];
		    
		    if (sum > maxSum) {
			    maxSum = sum;
			    ansStart = start;
			    ansEnd = i;
		    }
		    
		    if (sum < 0) {
			    sum = 0;
		    }
        }
        
        vector<int> result;  
		for (int i = ansStart; i <= ansEnd; i++) {  
			result.push_back(arr[i]);  
		}
  
		return result; 
    }
};
```

---
