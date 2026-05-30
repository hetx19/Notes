**Problem**: Given an array `nums` with `n` objects colored red, white, or blue, sort them **[in-place](https://en.wikipedia.org/wiki/In-place_algorithm)** so that objects of the same color are adjacent, with the colors in the order red, white, and blue.

We will use the integers `0`, `1`, and `2` to represent the color red, white, and blue, respectively.

You must solve this problem without using the library's sort function.

**Example**:
**Input**: `nums = [2,0,2,1,1,0]`
**Output**: `[0,0,1,1,2,2]`

[Visit Leetcode](https://leetcode.com/problems/sort-colors/description/)
[Visit GFG](https://www.geeksforgeeks.org/problems/sort-an-array-of-0s-1s-and-2s4231/1)

---

### Brute Force 
By using any in-place sorting algorithm (Eg: Quick Sort - [[Quick Sort]])

**Time Complexity**: O(n logn)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int partition(vector<int>& arr, int low, int high) {
		int pivot = arr[low], i = low, j = high;

		while (i < j) {
			while (arr[j] > pivot && j >= low + 1) {
				j--;
			}

			while (arr[i] <= pivot && i <= high - 1) {
				i++;
			}

			if (i < j) {
				swap(arr[i], arr[j]);
			}
		}

		swap(arr[low], arr[j]);
		return j;
	}
	
	void quickSort(vector<int>& arr, int low, int high) {
		if (low >= high) {
			return;
		}

		int p_index = partition(arr, low, high);
		quickSort(arr, low, p_index - 1);
		quickSort(arr, p_index + 1, high);
	}
	
  public:
	void sortColors(vector<int>& nums) {
		quickSort(nums, 0, nums.size() - 1);
	}
};
```

---

### Better Solution
By counting number of 0's, 1's and 2's

**Time Complexity**: O(2 * n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	void sortColors(vector<int>& nums) {
		int count0 = 0, count1 = 0, count2 = 0;
		
		for (int num : nums) {
			if (num == 0) {
				count0++;
			} else if (num == 1) {
				count1++;
			} else {
				count2++;
			}
		}
		
		for (int i = 0; i < count0; i++) {
			nums[i] = 0;
		}
		
		for (int i = count0; i < count0 + count1; i++) {
			nums[i] = 0;
		}
		
		for (int i = count0 + count1; i < count0 + count1 + count2; i++) {
			nums[i] = 0;
		}
	}
};
```

---

### Optimal Solution 
By Dutch National Flag Algorithm (3 pointer approach)

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	void sortColors(vector<int>& nums) {
		int low = 0, mid = 0, high = nums.size() - 1;
		
		while (mid <= high) {
			if (nums[mid] == 0) {
				swap(nums[low++], nums[mid++]);
			} else if (nums[mid] == 1) {
				mid++;
			} else {
				swap(nums[mid], nums[high--]);
			}
		}
	}
};
```