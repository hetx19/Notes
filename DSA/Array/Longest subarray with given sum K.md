**Problem**: Given an array `arr` of size `n` and an integer `k`, find the length of the longest sub-array that sums to k. If no such sub-array exists, return 0.

**Example**:
**Input**: `arr = [10, 5, 2, 7, 1, 9]`, k = 15  
**Output**: 4
**Explanation**: The longest sub-array with a sum equal to 15 is `[5, 2, 7, 1]`, which has a length of 4. This sub-array starts at index 1 and ends at index 4, and the sum of its elements (5 + 2 + 7 + 1) equals 15. Therefore, the length of this sub-array is 4.

[Visit GFG](https://www.geeksforgeeks.org/problems/longest-sub-array-with-sum-k0809/1)

---

### Brute Force Approach

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int longestSubarrayWithSumK(vector<int> &arr, long long K) {
		int length = 0;
		int n = arr.size();
		
		for (int i = 0; i < n; i++) {
			for (int j = i; j < n; j++) {
				long long sum = 0;
				for (int k = i; k <= j; k++) {
					sum += arr[k];
				}
				
				if (sum == K) {
					length = max(length, j - i + 1);
				}
			}
		}
		
		return length;
	}
};
```

---

### Slight improvement but still brute force

Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int longestSubarrayWithSumK(vector<int> &arr, long long K) {
		int length = 0;
		int n = arr.size();
		
		for (int i = 0; i < n; i++) {
			long long sum = 0;
			for (int j = i; j < n; j++) {
				sum += arr[j];
			}
				
			if (sum == K) {
				length = max(length, j - i + 1);
			}
		}
		
		return length;
	}
};
```

---

### Better Approach (Optimal Approach if array contains negative numbers)

**Time Complexity**: O(n logn)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int longestSubarrayWithSumK(vector<int> &arr, long long K) {
		int n = arr.size();
		map<long long, int> preSumMap;
		long long sum = 0;
		
		int maxLength = 0;
		
		for (int i = 0; i < n; i++) {
			sum += arr[i];
			
			if (sum == K) {
				maxLength = max(maxLength, i + 1);
			}
			
			long long rem = sum - K;
			
			if (preSumMap.find(rem) != preSumMap.end()) {
				int length = i - preSumMap[rem];
				maxLength = max(maxLength, length);
			}
			
			if (preSumMap.find(sum) == preSumMap.end()) {
				preSumMap[sum] = i;
			}
		}
		
		return maxLength;
	}
};
```

---

### Optimal Solution (If array has only positive)
By using 2 pointer approach

**Time Complexity**: O(n + n) → O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int longestSubarrayWithSumK(vector<int> &arr, long long K) {
		int left = 0, right = 0, maxLength = 0, n = arr.size();
		long long sum = 0;
		
		while (right < n) {
			while (left <= right && sum > K) {
				sum -= arr[left++];
			}
			
			if (sum == K) {
				maxLength = max(maxLength, right - left + 1);
			}
			
			right++;
			
			if (right < n) {
				sum += arr[right];
			}
		}
		
		return maxLength;
	}
};
```