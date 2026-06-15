**Problem**: Given an integer array nums, return all the triplets `[nums[i], nums[j], nums[k]]` such that `i != j`, `i != k`, and `j != k`, and `nums[i] + nums[j] + nums[k] == 0`.

Notice that the solution set must not contain duplicate triplets.

**Example**:

**Input**: `nums = [-1,0,1,2,-1,-4]`
**Output**: `[[-1,-1,2],[-1,0,1]]`
**Explanation**: 
`nums[0] + nums[1] + nums[2] = (-1) + 0 + 1 = 0.`
`nums[1] + nums[2] + nums[4] = 0 + 1 + (-1) = 0.`
`nums[0] + nums[3] + nums[4] = (-1) + 2 + (-1) = 0.`
`The distinct triplets are [-1,0,1] and [-1,-1,2]. Notice that the order of the output and the order of the triplets

[Visit Leetcode]()
[Visit GFG]()

---

### Brute Force
By generating every triplet

**Time Complexity**: O(n<sup>3</sup> logM) → M is number of unique triplets
**Space Complexity**: O(2 * M) → vector + Set

```cpp
class Solution { 
  public:
	vector<vector<int>> threeSum(vector<int>& nums, int target) {
		int n = nums.size();
		set<vector<int>> st;
		
		for (int i = 0; i < n; i++) {
			for (int j = i + 1; j < n; j++) {
				for (int k = j + 1; k < n; k++) {
					int sum = nums[i] + nums[j] + nums[k];
					
					if (sum == target) { // for this question target = 0
						vector<int> temp = {nums[i], nums[j], nums[k]};
						sort(temp.begin(), temp.end()); // Constant time (3 elements only)
						st.insert(temp);
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

**Time Complexity**: O(n<sup>2</sup> logM) → M is number of unique triplets
**Space Complexity**: O(2 * M) + O(N) → vector + Set

```cpp
class Solution { 
  public:
	vector<vector<int>> threeSum(vector<int>& nums, int target) {
		int n = nums.size();
		set<vector<int>> st;
		
		for (int i = 0; i < n; i++) {
			set<int> hashSet;
			
			for (int j = i + 1; j < n; j++) {
				int sum = nums[i] + nums[j];
				int third = target - sum;
				
				if (hashSet.find(third) != hashSet.end()) {
					vector<int> temp = {nums[i], nums[j], third};
					sort(temp.begin(), temp.end());
					
					st.insert(temp);
				}
				hashSet.insert(nums[j]);
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

**Time Complexity**: O(n<sup>2</sup> + n logn)
**Space Complexity**: O(M) → M is number of unique triplets

```cpp
class Solution { 
  public:
	vector<vector<int>> threeSum(vector<int>& nums, int target) {
		int n = nums.size();
		sort(nums.begin(), nums.end());
		vector<vector<int>> ans;
		
		for (int i = 0; i < n; i++) {
			if (i > 0 && nums[i] == nums[i - 1]) {
				continue;
			}
			int j = i + 1, k = n - 1;
			
			while (j < k) {
				int sum = nums[i] + nums[j];
				sum += nums[k];
				
				if (sum < target) {
					j++;
				} else if (sum > target) {
					k--;
				} else {
					vector<int> temp = {nums[i], nums[j], nums[k], nums[l]};
					ans.push_back(temp);
					j++;
					k--;
				}
				
				while (j < k && nums[j] == nums[j - 1]) {
					j++;
				}
			
				while (j < k && nums[k] == nums[k + 1]) {
					k--;
				}
			}
		}
		
		return ans;
	}
};
```

---