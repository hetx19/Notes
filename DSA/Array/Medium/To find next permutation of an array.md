**Problem**: A **permutation** of an array of integers is an arrangement of its members into a sequence or linear order.

- For example, for `arr = [1,2,3]`, the following are all the permutations of `arr`: `[1,2,3], [1,3,2], [2, 1, 3], [2, 3, 1], [3,1,2], [3,2,1]`.

The **next permutation** of an array of integers is the next lexicographically greater permutation of its integer. More formally, if all the permutations of the array are sorted in one container according to their lexicographical order, then the **next permutation** of that array is the permutation that follows it in the sorted container. If such arrangement is not possible, the array must be rearranged as the lowest possible order (i.e., sorted in ascending order).

- For example, the next permutation of `arr = [1,2,3]` is `[1,3,2]`.
- Similarly, the next permutation of `arr = [2,3,1]` is `[3,1,2]`.
- While the next permutation of `arr = [3,2,1]` is `[1,2,3]` because `[3,2,1]` does not have a lexicographical larger rearrangement.

Given an array of integers `nums`, _find the next permutation of_ `nums`.

The replacement must be **[in place](http://en.wikipedia.org/wiki/In-place_algorithm)** and use only constant extra memory.

**Example**:
**Input**: `nums = [1,2,3]`
**Output**: `[1,3,2]`

[Visit Leetcode](https://leetcode.com/problems/next-permutation/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/next-permutation5226/1)

---

### Brute Force Approach (Avoid To code in interview)
By generating all the permutations

**Time Complexity**: O(n * n!)
**Space Complexity**: O(n * n!)

```cpp
class Solution {
  private:
	void permute(int index, vector<int>& arr, set<vector<int>>& ans) {
		if (index == arr.size()) {
			ans.insert(arr);
			return;
		}

		for (int i = index; i < arr.size(); i++) {
			swap(arr[index], arr[i]);
			permute(index + 1, arr, ans);
			swap(arr[index], arr[i]);
		}
	}

  public:
	void nextPermutation(vector<int>& nums) {
		set<vector<int>> ans;
		permute(0, nums, ans);
		vector<vector<int>> permutations(ans.begin(), ans.end());
	
		for (int i = 0; i < permutations.size(); i++) {
			if (permutations[i] == nums) {
				if (i == permutations.size() - 1) {
					nums = permutations[0];
				} else {
					nums = permutations[i + 1];
				}
			}
		}
	}
};
```

---

### Optimal Solution 

**Time Complexity**: O(3 * n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	void nextPermutation(vector<int>& nums) {
		int n = nums.size(), index = -1;
		
		for (int i = n - 2; i >= 0; i--) {
			if (nums[i] < nums[i + 1]) {
				index = i;
				break;
			}
		}
		
		if (index == -1) {
			reverse(nums.begin(), nums.end());
			return;
		}
		
		for (int i = n - 1; i > index; i--) {
			if (nums[i] > nums[index]) {
				swap(nums[i], nums[index]);
				break;
			}
		}
		
		reverse(nums.begin() + index + 1, nums.end());
	}
};
```