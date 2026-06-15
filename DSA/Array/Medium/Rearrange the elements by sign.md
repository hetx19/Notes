**Problem**: You are given a **0-indexed** integer array `nums` of **even** length consisting of an **equal** number of positive and negative integers.

You should return the array of nums such that the array follows the given conditions:
1. Every **consecutive pair** of integers have **opposite signs**.
2. For all integers with the same sign, the **order** in which they were present in `nums` is **preserved**.
3. The rearranged array begins with a positive integer.

Return _the modified array after rearranging the elements to satisfy the aforementioned conditions_.

**Example**:
**Input**: `nums = [3,1,-2,-5,2,-4]`
**Output**: `[3,-2,1,-5,2,-4]`
**Explanation**:
The positive integers in nums are `[3,1,2]`. The negative integers are `[-2,-5,-4]`.
The only possible way to rearrange them such that they satisfy all conditions is `[3,-2,1,-5,2,-4]`.
Other ways such as `[1,-2,2,-5,3,-4]`, `[3,1,2,-2,-5,-4]`, `[-2,3,-5,1,-4,2]` are incorrect because they do not satisfy one or more conditions.

---

### Brute Force Approach (Optimal if number of positive is not equal to number of negative)
By creating 2 different array for storing positive and negative integers


**Time Complexity**: O(2 * n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<int> rearrangeArray(vector<int>& nums) {
		int n = nums.size();
		vector<int> positive, negative;
		
		for (int &num : nums) {
			if (num > 0) {
				positive.push_back(num);
			} else {
				negative.push_back(num);
			}
		}
		
		for (int i = 0; i < (n / 2); i++) {
			nums[i * 2] = positive[i];
			nums[(i * 2) + 1] = negative[i];
		}
		
		return nums;
	}
};
```

### If number of positive in not equal to number of negative

```cpp
class Solution {
  public:
	vector<int> rearrangeArray(vector<int>& nums) {
		vector<int> positive, negative;
		
		for (int &num : nums) {
			if (num > 0) {
				positive.push_back(num);
			} else {
				negative.push_back(num);
			}
		}
		
		if (positive.size() > negative.size()) {
			for (int i = 0; i < negative.size(); i++) {
				nums[i * 2] = positive[i];
				nums[(i * 2) + 1] = negative[i];
			}
			
			int index = negative.size() * 2;
			
			for (int i = negative.size(); i < positive.size(); i++) {
				nums[index++] = positive[i];
			}
		} else {
			for (int i = 0; i < positive.size(); i++) {
				nums[i * 2] = positive[i];
				nums[(i * 2) + 1] = negative[i];
			}
			
			int index = positive.size() * 2;
			
			for (int i = positive.size(); i < negative.size(); i++) {
				nums[index++] = negative[i];
			}
		}
		
		return nums;
	}
};
```

---

### Optimal Approach (if number of positive equals to number of negative)

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	vector<int> rearrangeArray(vector<int>& nums) {
		int n = nums.size();
		vector<int> ans(n);
		
		int pIndex = 0, nIndex = 1;
		
		for (int &num : nums) {
			if (num > 0) {
				ans[pIndex] = num;
				pIndex += 2;
			} else {
				ans[nIndex] = num;
				nIndex += 2;
			}
		}
		
		return ans;
	}
};
```

---