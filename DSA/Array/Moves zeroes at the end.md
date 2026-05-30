**Problem**: Given an integer array `nums`, move all `0`'s to the end of it while maintaining the relative order of the non-zero elements.

**Note** that you must do this in-place without making a copy of the array.

**Example**:

**Input:** nums = `[0,1,0,3,12]`
**Output:** `[1,3,12,0,0]`

[Vist_leetcode](https://leetcode.com/problems/move-zeroes/description/)

---

### Brute force Approach:

**Idea**:

- Collect all **non-zero** elements in a temporary array.
- Copy non-zero elements back to `nums`.
- Fill remaining positions with `0`s.

**Time Complexity:** O(n) + O(n) → O(n)
**Space Complexity** O(k) → k is number of non zero elements

```cpp
class Solution {
  public:
	void moveZeroes(vector<int> &nums) {
		int n = nums.size();
		vector<int> temp;

		for (int num : nums) {
			if (num != 0) temp.push_back(num);
		}

		int m = temp.size();
		for (int i = 0; i < m; i++) {
			nums[i] = temp[i];
		}

        for (int i = m; i < n; i++) {
            nums[i] = 0;
        }
	}
};
```

---

### Optimal Approach (2 pointer Approach)

**Idea**:

- Maintain a pointer `lastNonZero` to track where the next **non-zero** should go.
- Traverse the array and **swap** non-zero elements with `lastNonZero`.
- This preserves relative order and works fully in-place.

**Time Complexity:** O(n)
**Space Complexity** O(1)

```cpp
class Solution {
  public:
	void moveZeroes(vector<int> &nums) {
		int j = -1, n = nums.size();

		for (int i = 0; i < n; i++) {
			if (nums[i] == 0) {
				j = i;
				break;
			}
		}

		for (int i = j + 1; i < n; i++) {
			if (nums[i] != 0) {
				swap(nums[i], nums[j]);
				j++;
			}
		}
	}
};
```
