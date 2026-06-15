**Problem**: Given an integer array `nums`, return _the number of **reverse pairs** in the array_.

A **reverse pair** is a pair `(i, j)` where:

- `0 <= i < j < nums.length` and
- `nums[i] > 2 * nums[j]`.

**Example**:

**Input**: `nums = [1,3,2,3,1]`
**Output**: 2
**Explanation**: The reverse pairs are:
`(1, 4) --> nums[1] = 3, nums[4] = 1, 3 > 2 * 1`
`(3, 4) --> nums[3] = 3, nums[4] = 1, 3 > 2 * 1`

[Visit Leetcode](https://leetcode.com/problems/reverse-pairs/)
[Visit GFG](https://www.geeksforgeeks.org/problems/count-reverse-pairs/1)

---

### Brute Force
By generating all the pairs

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int reversePairs(vector<int> &nums) {
	    int n = nums.size();
	    int count = 0;
	    
	    for (int i = 0; i < n; i++) {
		    for (int j = i + 1; j < n; j++) {
			    if (nums[i] > (2 * nums[j])) {
				    count++;
				}
		    }
	    }
	    
	    return count;
    }
};
```

---

### Optimal Solution
By using merge sort - [[Merge Sort]]

**Time Complexity**: O(n logn)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int countPairs(vector<int>& nums, int low, int mid, int high) {
		int count = 0;
		int right = mid + 1;
		
		for (int i = low; i <= mid; i++) {
			while (right <= high && nums[i] > 2 * nums[right]) {
				right++;			
			}
			
			count += (right - mid - 1);
		}
		
		return count;
	}
	
	void merge(vector<int>& nums, int low, int mid, int high) {
		vector<int> temp;
		int left = low, right = mid + 1;
		
		while (left <= mid && right <= high) {
			if (nums[left] <= nums[right]) {
				temp.push_back(nums[left++]);
			} else {
				temp.push_back(nums[right++]);
			}
		}
		
		while (left <= mid) {
			temp.push_back(nums[left++]);
		}
		
		while (right <= high) {
			temp.push_back(nums[right++]);
		}
		
		for (int i = low; i <= high; i++) {
			nums[i] = temp[i - low];
		}
	}
	
	int mergeSort(vector<int>& nums, int low, int high) {
		int count = 0;
		
		if (low >= high) {
			return count;
		}
		
		int mid = low + ((high - low) / 2);
		count += mergeSort(nums, low, mid);
		count += mergeSort(nums, mid + 1, high);
		count += countPairs(nums, low, mid, high);
		merge(nums, low, mid, high);
		
		return count;
	}
	
  public:
    int reversePairs(vector<int> &nums) {
	    return mergeSort(nums, 0, nums.size() - 1);
    }
};
```