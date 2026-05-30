**Problem**: Given an array `nums` of size `n`, return _the majority element_.

The majority element is the element that appears more than `⌊n / 2⌋` times. You may assume that the majority element always exists in the array.

**Example**:
**Input**: `nums = [3,2,3]`
**Output**: 3

[Visit Leetcode](https://leetcode.com/problems/majority-element/)
[Visit GFG](https://www.geeksforgeeks.org/problems/majority-element-1587115620/1)

---

### Brute Force
By using Linear Search Algorithm - [[Linear Search]]

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int linearSearch(vector<int>& nums, int target) {
		int cnt = 0;
		
		for (int &num : nums) {
			if (num == target) {
				cnt++;
			}
		}
		
		return cnt;
	}
	
  public:
	int majorityElement(vector<int>& nums) {
		int n = nums.size();
		
		for (int &num : nums) {
			int cnt = linearSearch(nums, num);
			
			if (cnt > n / 2) {
				return num;
			}
		}
		
		return -1;
	}
};
```

---

### Better Approach
By using concept of hashing - [[Hashing]]

**Time Complexity**: O(2 * n) → unordered_map  |  map → O(n logn)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int majorityElement(vector<int>& nums) {
		int n = nums.size();
		unordered_map<int, int> mpp;
		
		for (int &num : nums) {
			mpp[num]++;
		}
		
		for (auto &it : mpp) {
			if (it.second > n / 2) {
				return it.first;
			}
		}
		
		return -1;
	}
};
```

---
### Optimal Solution
By using Moore's voting algorithm

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int majorityElement(vector<int>& nums) {
		int element = 0, cnt = 0;
		
		for (int &num : nums) {
			if (cnt == 0) {
				cnt = 1;
				element = num;
			} else if (num == element) {
				cnt++;
			} else {
				cnt--;
			}
		}
		
		return element;
	}
};
```

> [!warning]
> If Question doesn't specify that majority element always exists in the array. Then check the count of the element (count)
> if count > n / 2 then return the element else return -1


---
