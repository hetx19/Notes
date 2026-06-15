**Problem**: Given an integer array of size `n`, find all elements that appear more than `⌊ n/3 ⌋` times.

**Example**:

**Input**: nums = `[3,2,3]`
**Output**: `[3]`

[Visit Leetcode](https://leetcode.com/problems/majority-element-ii/)
[Visit GFG](https://www.geeksforgeeks.org/problems/majority-vote/1)

---

### Brute Force
By counting frequency of every element

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)  → At max we will be storing 2 elements

```cpp
class Solution {
  private:
	int countFrequency(vector<int>& nums, int target) {
		int count = 0;
		
		for (int num : nums) {
			if (num == target) {
				count++;
			}
		}
		
		return count;
	}
	
  public:
	vector<int> majorityElement(vector<int>& nums) {
		int n = nums.size();
		vector<int> ans;
		
		for (int num : nums) {
			if (ans.size() == 0 || ans[0] != num) {
				int count = countFrequency(nums, num);
				
				if (count > n / 3) {
					ans.push_back(num);
				}
			}
			
			if (ans.size() == 2) {
				break;
			}
		}
		
		return ans;
	}
};
```

---

### Better Approach
By using concept of Hashing - [[Hashing]]

**Time Complexity**: O(2n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<int> majorityElement(vector<int>& nums) {
		int n = nums.size();
		int mini = (n / 3) + 1;
		unordered_map<int, int> mpp;
		
		vector<int> ans;
		
		for (int num : nums) {
			mpp[num]++;
			if (mpp[num] == mini) {
				ans.push_back(num);
			}
		}
		
		return ans;
	}
};
```

>[!tip]
>If we want ans in sorted order than do sorting. But Time complexity will remain same

---

### Optimal Solution
By Boyer's Moore's Voting Algorithm

**Time Complexity**: O(2n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	vector<int> majorityElement(vector<int>& nums) {
		int n = nums.size();
		int mini = (n / 3) + 1;
		
		vector<int> ans;
		
		int count1 = 0, count2 = 0;
		int element1 = INT_MIN, element2 = INT_MIN;
		
		for (int num : nums) {
			if (count1 == 0 && num != element2) {
				element1 = num;
				count1++;
			} else if (count2 == 0 && num != element1) {
				element2 = num;
				count2++;
			} else if (num == element1) {
				count1++;
			} else if (num == element2) {
				count2++;
			} else {
				count1--;
				count2--;
			}
		}
		
		count1 = count2 = 0;
		for (int num : nums) {
			if (num == element1) count1++;
			else if (num == element2) count2++;
		}
		
		if (count1 >= mini) ans.push_back(element1);
		if (count2 >= mini) ans.push_back(element2);
		
		return ans;
	}
};
```

---
