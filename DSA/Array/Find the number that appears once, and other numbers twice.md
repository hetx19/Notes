**Problem**: Given a **non-empty** array of integers `nums`, every element appears _twice_ except for one. Find that single one.

You must implement a solution with a linear runtime complexity and use only constant extra space.

**Example**:
**Input:** `nums = [2, 2, 1]`
**Output:** 1

[Visit Leetcode](https://leetcode.com/problems/single-number/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-unique-number/1)

---

### Brute Force Approach
By doing linear search

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int countOccurence(vector<int> &nums, int element) {
		int cnt = 0;
		
		for (int num : nums) {
			if (num == element) {
				cnt++;
			}
		}
		
		return cnt;
	}
	
  public:
	int singleNumber(vector<int> &nums) {
		for (int num : nums) {
			if (countOccurence(nums, num) == 1) {
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

**Time Complexity**: O(n + n) → O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int singleNumber(vector<int> &nums) {
		unordered_map<int, int> mpp;
		
		for (int num : nums) {
			mpp[num]++;
		}
		
		for (auto &it : mpp) {
			if (it.second == 1) {
				return it.first;
			}
		}
		
		return -1;
	}
};
```

---
### Optimal Approach
By using XOR operation

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int singleNumber(vector<int> &nums) {
		int XOR1 = 0;
		
		for (int num : nums) {
			XOR1 ^= num;
		}
		
		return XOR1;
	}
};
```
