**Problem**: Given an unsorted array of integers `nums`, return _the length of the longest consecutive elements sequence._

You must write an algorithm that runs in `O(n)` time.

**Example**:
**Input:** `nums = [100,4,200,1,3,2]`
**Output:** 4
**Explanation:** The longest consecutive elements sequence is `[1, 2, 3, 4]`. Therefore its length is 4.

[Visit Leetcode](https://leetcode.com/problems/longest-consecutive-sequence/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/longest-consecutive-subsequence2449/1)

---

### Brute Force Approach
By checking for next number of every single element in the array

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	bool linearSearch(vector<int>& arr, int target) {
		for (int &num : arr) {
			if (num == target) {
				return true;
			}
		}
		
		return false;
	}
	
  public:
	int longestConsecutive(vector<int>& nums) {
		int longest = 0;
		
		for (int &num : nums) {
			int x = num;
			int counter = 1;
			
			while (linearSearch(nums, x + 1)) {
				x = x + 1;
				counter++;
			}
			
			longest = max(counter, longest);
		}
		
		return longest;
	}
};
```

---

### Better Approach

**Time Complexity**: O(n logn + n) → O(n logn)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	int longestConsecutive(vector<int>& nums) {
		int longest = 0, lastSmaller = INT_MIN, countCurrent = 0;
		
		sort(nums.begin(), nums.end());
		
		for (int &num : nums) {
			if (num - 1 == lastSmaller) {
				countCurrent++;
			} else if (num != lastSmaller) {
				countCurrent = 1;
			}
			
			lastSmaller = num;
			longest = max(longest, countCurrent);
		}
		
		return longest;
	}
};
```

---

### Optimal Solution

**Time Complexity**: O(3 * n)
**Space Complexity**: O(n)

```cpp
class Solution { 
  public:
	int longestConsecutive(vector<int>& nums) {		
		int longest = 0;
		unordered_set<int> st;
		
		for (int &num : nums) {
			st.insert(num);
		}
		
		for (auto &it : st) {
			if (st.count(it - 1) == 0) {
				int count = 1;
				auto x = it;
				
				while (st.count(x + 1) > 0) {
					x++;
					count++;
				}
				
				longest = max(longest, count);
			}
		}
		
		return longest;
	}
};
```

---