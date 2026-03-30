**Problem**: Given an integer array `nums` sorted in **non-decreasing order**, remove the duplicates in-place such that each unique element appears only **once**. The **relative order** of the elements should be kept the **same**.
Consider the number of *unique elements* in `nums` to be `k`​​​​​​​. After removing duplicates, return the number of unique elements `k`.

The first `k` elements of `nums` should contain the unique numbers in **sorted order**. The remaining elements beyond index `k - 1` can be ignored.

**Example**
**Input:** nums = `[1,1,2]`
**Output:** 2, nums = `[1,2,_]`
**Explanation:** Your function should return k = 2, with the first two elements of nums being 1 and 2 respectively.
It does not matter what you leave beyond the returned k (hence they are underscores).

[Vist_leetcode](https://leetcode.com/problems/remove-duplicates-from-sorted-array/description/)

---

### Brute force Approach:

**Idea**:

- Store all the unique elements
- Then assign the all the unique values from the beginning

**Time Complexity:** O(n log n) + O(n) → O(n log n)
**Space Complexity** O(k) → k is number of unique elements

```cpp
class Solution {
public:
	int removeDuplicates(vector<int> &nums) {
		int n = nums.size();
		set<int> st;

		for (auto num : nums) {
			st.insert(num);
		}

		int idx = 0;
		for (auto it : st) {
			nums[idx++] = it;
		}

		return idx;
	}
};
```

---

### Optimal Approach (2 pointer Approach)

**Idea**:

- Use **two pointers**
- Maintain a pointer for **unique elements**

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
class Solution {
public:
	int removeDuplicates(vector<int> &nums) {
		if (nums.empty()) return 0;

		int i = 0, n = nums.size();

		for (int j = 1; j < n; j++) {
			if (nums[i] != nums[j]) {
				nums[i + 1] = nums[j];
				i++;
			}
		}

		return (i + 1);
	}
};
```
