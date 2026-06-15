**Problem**: You are given two integer arrays `nums1` and `nums2`, sorted in **non-decreasing order**, and two integers `m` and `n`, representing the number of elements in `nums1` and `nums2` respectively.

**Merge** `nums1` and `nums2` into a single array sorted in **non-decreasing order**.

The final sorted array should not be returned by the function, but instead be _stored inside the array_ `nums1`. To accommodate this, `nums1` has a length of `m + n`, where the first `m` elements denote the elements that should be merged, and the last `n` elements are set to `0` and should be ignored. `nums2` has a length of `n`.

**Example**:

**Input**: `nums1 = [1,2,3,0,0,0], m = 3, nums2 = [2,5,6], n = 3`
**Output**: `[1,2,2,3,5,6]`
**Explanation:** The arrays we are merging are `[1,2,3]` and `[2,5,6]`.
The result of the merge is `[1,2,2,3,5,6]` with the underlined elements coming from nums1.

[Visit Leetcode](https://leetcode.com/problems/merge-sorted-array/)
[Visit GFG](https://www.geeksforgeeks.org/problems/merge-two-sorted-arrays-1587115620/1)

---

### Brute Force 
By using merge function of merge sort - [[Merge Sort]]

**Time Complexity**: O(m + n)
**Space Complexity**: O(m + n)

```cpp
class Solution {
  public:
	void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
		vector<int> temp;
		
		int left = 0, right = 0;
		
		while (left < n && right < m) {
			if (nums1[left] <= nums2[right]) {
				temp.push_back(nums1[left++]);
			} else {
				temp.push_back(nums2[right++]);
			}
		}
		
		while (left < n) {
			temp.push_back(nums1[left++]);
		}
		
		while (right < m) {
			temp.push_back(nums2[right++]);
		}
		
		for (int i = 0; i < n + m; i++) {
			nums1[i] = temp[i];
		}
	}
};
```

---

### Optimal Solution
By using 2 pointer approach

**Time Complexity**: O(min(m, n)) + O(m logm) + O(n log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
		int left = n - 1, right = 0;
		
		while (left >= 0 && right < m) {
			if (nums1[left] > nums2[right]) {
				swap(nums1[left--], nums2[right++]);
			} else {
				break;
			}
		}
		
		sort(nums1.begin(), nums1.end());
		sort(nums2.begin(), nums2.end());
		
		for (int i = 0; i < n; i++) {
			nums1[i + m] = nums2[i];
		}
	}
};
```

### Alternative Approach
By using Gap Method / Shell Sorting

```cpp
class Solution {
  private:
	void swapIfGreater(vector<int>& nums1, vector<int>& nums2, int idx1, int idx2) {
		if (nums1[idx1] > nums2[idx2]) {
			swap(nums1[idx1], nums2[idx2]);
		}
	}
	
  public:
	void merge(vector<int>& nums1, int m, vector<int>& nums2, int n) {
		int length = n + m;
		int gap = (length / 2) + (length % 2);
		
		while (gap > 0) {
			int left = 0, right = left + gap;
			
			while (right < length) {
                if (left < m && right >= m) {
                    swapIfGreater(nums1, nums2, left, right - m);
                } else if (left >= m) {
                    swapIfGreater(nums2, nums2, left - m, right - m);
                } else {
                    swapIfGreater(nums1, nums1, left, right);
                }

                left++;
                right++;
            }
		
			if (gap == 1) {
				return;
			}
		
			gap = (gap / 2) + (gap % 2);
		}
		
		for (int i = 0; i < n; i++) {
			nums1[i + m] = nums2[i];
		}
	}
};
```

---