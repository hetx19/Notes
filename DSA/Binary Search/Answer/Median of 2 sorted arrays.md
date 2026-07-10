**Problem**: Given two sorted arrays `nums1` and `nums2` of size `m` and `n` respectively, return **the median** of the two sorted arrays.

The overall run time complexity should be `O(log (m+n))`.

**Example**:

**Input**: `nums1 = [1,3], nums2 = [2]`
**Output**: 2.00000
**Explanation**: `merged array = [1,2,3] and median is 2.`

[Visit Leetcode](https://leetcode.com/problems/median-of-two-sorted-arrays/)
[Visit GFG](https://www.geeksforgeeks.org/problems/median-of-2-sorted-arrays-of-different-sizes/1)

---
### Brute Force Solution
By using Merge Sort - [[Merge Sort]]

**Time Complexity**: O(m + n)
**Space Complexity**: O(m + n)

```cpp
class Solution {
  private:
	vector<int> merge(vector<int>& nums1, vector<int>& nums2) {
		vector<int> temp;
		int n = nums1.size(), m = nums2.size();
		
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
		
		return temp;
	}
	
  public:
	double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
		int n = nums1.size(), m = nums2.size();
		int total = m + n;
		
		vector<int> ans = merge(nums1, nums2);
		
		if (total & 1) {
			return (ans[total / 2]);
		}
		 
		return (ans[(total / 2) - 1] + ans[total / 2]) / 2.0;
	}
};
```

---

### Better Approach
By finding index1 and index2

**Time Complexity**: O(m + n)
**Space Complexity**: O(1)

```cpp
class Solution {
public:
    double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
        int n = nums1.size(), m = nums2.size();
        int total = n + m;

        int left = 0, right = 0;

        if (total & 1) {
            int target = total / 2;
            int element = 0;

            for (int index = 0; index <= target; index++) {
                if (left < n && (right >= m || nums1[left] <= nums2[right])) {
                    element = nums1[left++];
                } else {
                    element = nums2[right++];
                }
            }

            return element;
        }

        int target1 = total / 2 - 1;
        int target2 = total / 2;

        int first = 0, second = 0;

        for (int index = 0; index <= target2; index++) {
            int element;

            if (left < n && (right >= m || nums1[left] <= nums2[right])) {
                element = nums1[left++];
            } else {
                element = nums2[right++];
            }

            if (index == target1) {
                first = element;
            }

            if (index == target2) {
                second = element;
            }
        }

        return (first + second) / 2.0;
    }
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: O(log (min(m, n)))
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
		int n = nums1.size(), m = nums2.size();
		
		if(n > m) {
			return findMedianSortedArrays(nums2, nums1);
		}
		
		int low = 0, high = n;
		int total = n + m;
		int left = (total + 1) / 2;
		
		while(low <= high) {
			int mid1 = low + ((high - low) / 2);
			int mid2 = left - mid1;
			
			int l1 = (mid1 > 0) ? nums1[mid1 - 1] : INT_MIN;
			int l2 = (mid2 > 0) ? nums2[mid2 - 1] : INT_MIN;
			int r1 = (mid1 < n) ? nums1[mid1] : INT_MAX;
			int r2 = (mid2 < m) ? nums2[mid2] : INT_MAX;
			
			if (l1 <= r2 && l2 <= r1) {
				if(n & 1) {
					return max(l1, l2);
				}
				
				return ((double)(max(l1, l2) + min(r1, r2))) / 2.0;
			} else if (l1 > r2) {
				high = mid1 - 1;
			} else {
				low = mid1 + 1;
			}
		}
		
		return 0;
	}
};
```

---
