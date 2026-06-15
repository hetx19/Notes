**Problem**: Given an array **`arr[]`** containing integers and an integer **`k`**, your task is to find the length of the longest subarray where the sum of its elements is equal to the given value **`k`**. If there is no subarray with sum equal to **`k`**, return **`0`**.

**Example**:

**Input**: `arr[] = [10, 5, 2, 7, 1, -10], k = 15`
**Output**: `6`
**Explanation**: Subarrays with sum = 15 are `[5, 2, 7, 1]`, `[10, 5]` and `[10, 5, 2, 7, 1, -10]`. The length of the longest subarray with a sum of 15 is 6.

[Visit GFG](https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1)

---

### Brute Force
By generating all Subarray

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int longestSubarray(vector<int>& arr, int k) {
	    int n = arr.size();
	    int maxLength = 0;
	    
	    for (int start = 0; start < n; start++) {
		    int sum = 0;
		    for (int end = start; end < n; end++) {
			    sum += arr[end];
			    
			    if (sum == k) {
				    int length = end - start + 1;
				    maxLength = max(maxLength, length);
			    }
		    }
	    }
	    
	    return maxLength;
    }
};
```

---

### Better Solution
By using concept of hashing - [[Hashing]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& arr, int k) {
        int n = arr.size();
        int maxLength = 0;
        long long sum = 0;
        unordered_map<long long, int> mpp;

        for (int i = 0; i < n; i++) {
            sum += arr[i];

            if (sum == k) {
                maxLength = i + 1;
            } else if (mpp.find(sum - k) != mpp.end()) {
                maxLength = max(maxLength, i - mpp[sum - k]);
            }

            if (mpp.find(sum) == mpp.end()) {
                mpp[sum] = i;
            }
        }

        return maxLength;
    }
};
```

---