**Problem**: Given an array `nums` containing `n` distinct numbers in the range `[0, n]`, return _the only number in the range that is missing from the array._

**Example**:
**Input**: `nums = [3,0,1]`
**Output**: 2

**Explanation**
`n = 3` since there are 3 numbers, so all numbers are in the range `[0,3]`. 2 is the missing number in the range since it does not appear in `nums`.

[Visit Leetcode](https://leetcode.com/problems/missing-number/)
[Visit_GFG](https://www.geeksforgeeks.org/problems/missing-number-in-array1416/1)

---

### Brute Force Approach
By simple linear search

**Time complexity**: O(n<sup>2</sup>)
**Space complexity**: O(1)

```cpp
class Solution {
  private:
	bool linearSearch(vector<int> &nums, int element) {
		for (int num : nums) {
			if (num == element) {
				return true;
			}
		}
		
		return false;
	}
	
  public:
	int missingNumber(vector<int> &nums) {
		int n = nums.size();
		
		for (int i = 0; i <= n; i++) {
			if (!linearSearch(nums, i)) {
				return i;
			}
		}
		
		return -1;
	}
};
```

---

### Better Approach
By using hashing concept - [[Hashing]]

**Time complexity**: O(n + n) → O(n)
**Space complexity**: O(n)

```cpp
class Solution {
  public:
	int missingNumber(vector<int> &nums) {
		int n = nums.size();
		unordered_map<int, bool> mpp;
		
		for (auto &it : nums) {
			mpp[it] = true;
		}
		
		for (int i = 0; i <= n; i++) {
			if (mpp.find(i) == mpp.end()) {
				return i;
			}
		}
		
		return -1;
	}
};
```

Or 

Since we only need presence/absence
```cpp
class Solution {
  public:
    int missingNumber(vector<int>& nums) {
        int n = nums.size();

        unordered_set<int> st;

        for (int num : nums) {
            st.insert(num);
        }

        for (int i = 0; i <= n; i++) {
            if (st.find(i) == st.end()) {
                return i;
            }
        }

        return -1;
    }
};
```

---

### Optimal Approach
There are two optimal approach

1). By using Sum

**Time complexity**: O(n)
**Space complexity**: O(1)

```cpp
class Solution {
  public:
    int missingNumber(vector<int>& nums) {
	    int n = nums.size();
	    
	    int sum = (n * (n + 1)) / 2;
	    
	    for (int num : nums) {
		    sum -= num;
	    }
	    
	    return sum;
    }
};
```

2). By using XOR

**Time complexity**: O(n)
**Space complexity**: O(1)

```cpp
class Solution {
  public:
	int missingNumber(vector<int>& nums) {
		int n = nums.size();
		int xorNums = 0, xorRange = 0;
		
		for(int i = 0; i < n; i++) {
			xorNums = xorNums^(nums[i]);
			xorRange = xorRange^(i);
		}
		
		return xorNums^xorRange^n; 
	}
};
```