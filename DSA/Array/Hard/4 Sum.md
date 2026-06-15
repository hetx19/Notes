**Problem**: Given an array `nums` of `n` integers, return _an array of all the **unique** quadruplets_ `[nums[a], nums[b], nums[c], nums[d]]` such that:

- `0 <= a, b, c, d < n`
- `a`, `b`, `c`, and `d` are **distinct**.
- `nums[a] + nums[b] + nums[c] + nums[d] == target`

You may return the answer in **any order**.

**Example**:
**Input**: `nums = [1,0,-1,0,-2,2], target = 0`
**Output:** `[[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]`

[Visit Leetcode](https://leetcode.com/problems/4sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-all-four-sum-numbers1732/1)

---

### Brute Force
By generating every Quadruples

**Time Complexity**: O(n<sup>4</sup> logM) → M is number of unique Quadruples
**Space Complexity**: O(2 * M) → vector + Set

```cpp
class Solution { 
  public:
	vector<vector<int>> fourSum(vector<int>& nums, int target) {
		int n = nums.size();
		set<vector<int>> st;
		
		for (int i = 0; i < n; i++) {
			for (int j = i + 1; j < n; j++) {
				for (int k = j + 1; k < n; k++) {
					for (int l = k + 1; l < n; l++) {
						int sum = nums[i] + nums[j];
						sum += nums[k];
						sum += nums[l];
					
						if (sum == target) {
							vector<int> temp = {nums[i], nums[j], nums[k], nums[l]};
							sort(temp.begin(), temp.end());
							st.insert(temp);
						}
					}
				}
			}
		}
		
		vector<vector<int>> ans(st.begin(), st.end());
		return ans;
	}
};
```

---

### Better Approach
By using hashSet - [[Hashing]]

**Time Complexity**: O(n<sup>3</sup> logM) → M is number of unique Quadruples
**Space Complexity**: O(2 * M) + O(N) → vector + Set

```cpp
class Solution { 
  public:
	vector<vector<int>> fourSum(vector<int>& nums, int target) {
		int n = nums.size();
		set<vector<int>> st;
		
		for (int i = 0; i < n; i++) {
			for (int j = i + 1; j < n; j++) {
				set<int> hashSet;
				for (int k = j + 1; k < n; k++) {
					int sum = nums[i] + nums[j];
					sum += nums[k];
					int fourth = target - sum;
				
					if (hashSet.find(fourth) != hashSet.end()) {
						vector<int> temp = {nums[i], nums[j], nums[k], fourth};
						sort(temp.begin(), temp.end());
					
						st.insert(temp);
					}
					hashSet.insert(nums[k]);
				}
			}
		}
		
		vector<vector<int>> ans(st.begin(), st.end());
		return ans;
	}
};
```

---

### Optimal Approach
Removing extra space by sorting - [[Sorting]]

**Time Complexity**: O(n<sup>3</sup> + n logn)
**Space Complexity**: O(M) → M is number of unique Quadruples

```cpp
class Solution { 
  public:
	vector<vector<int>> fourSum(vector<int>& nums, int target) {
		int n = nums.size();
		sort(nums.begin(), nums.end());
		vector<vector<int>> ans;
		
		for (int i = 0; i < n; i++) {
			if (i > 0 && nums[i] == nums[i - 1]) {
				continue;
			}
			
			for (int j = i + 1; j < n; j++) {
				if (j != i + 1 && nums[j] == nums[j - 1]) {
					continue;
				}
				
				int k = j + 1, l = n - 1;
				
				while (k < l) {
					int sum = nums[i] + nums[j];
					sum += nums[k];
					sum += nums[l];
				
					if (sum < target) {
						k++;
					} else if (sum > target) {
						l--;
					} else {
						vector<int> temp = {nums[i], nums[j], nums[k], nums[l]};
						ans.push_back(temp);
						k++;
						l--;
					}
				}
				
				while (k < l && nums[k] == nums[k - 1]) {
					k++;
				}
			
				while (k < l && nums[l] == nums[l + 1]) {
					l--;
				}
			}
		}
		
		return ans;
	}
};
```