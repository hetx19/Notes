**Problem**: Given an integer array `nums`, find a subarray that has the largest product, and return _the product_.

The test cases are generated so that the answer will fit in a **32-bit** integer.

**Note** that the product of an array with a single element is the value of that element.

**Example**:

**Input**: `nums = [2,3,-2,4]`
**Output**: 6
**Explanation**: `[2,3]` has the largest product 6.

[Visit Leetcode](https://leetcode.com/problems/maximum-product-subarray/)
[Visit GFG](https://www.geeksforgeeks.org/problems/maximum-product-subarray3604/1)

---

### Brute Force
By generating all the subarrays

**Time Complexity**: O(n<sup>3</sup>)  
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProduct(vector<int>& nums) {
		int n = nums.size();
		int maxProduct = INT_MIN;
		
		for (int i = 0; i < n; i++) {
			for (int j = i; j < n; j++) {
				int product = 1;
				for (int k = i; k <= j; k++) {
					product *= nums[k];
				}
				maxProduct = max(maxProduct, product);
			}
		}
		
		return maxProduct;
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
	int maxProduct(vector<int>& nums) {
		int n = nums.size();
		int maxProduct = INT_MIN;
		
		for (int i = 0; i < n; i++) {
			int product = 1;
			for (int j = i; j < n; j++) {
				product *= nums[j];
				maxProduct = max(maxProduct, product);
			}
		}
		
		return maxProduct;
	}
};
```

---

### Optimal Approach
By Observation

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProduct(vector<int>& nums) {
		int ans = INT_MIN, prefix = 1, suffix = 1;
		int n = nums.size();
		
		for(int i = 0; i < n; i++) {
			if(prefix == 0) {
				prefix = 1;
			}
			
			if(suffix == 0) {
				suffix = 1;
			}
			
			prefix = prefix * nums[i];
			suffix = suffix * nums[n - i - 1];
			
			ans = max(ans, max(prefix, suffix));
		}
		
		return ans;
	}
};
```

### Alternative Approach
By using `Kadane's Algorithm`

**Time Complexity**: O(n)  
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int maxProduct(vector<int>& nums) {
		int ans = nums[0], maxProduct = nums[0], minProduct = nums[0];
		int n = nums.size();
		
		for(int i = 0; i < n; i++) {
			int current = nums[i];
			
			if (current < 0) {
				swap(maxProduct, minProduct);
			}
			
			maxProduct = max(current, maxProduct * current);
			minProduct = min(current, minProduct * current);
			ans = max(ans, maxProduct);
		}
		
		return ans;
	}
};
```

---