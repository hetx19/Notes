**Problem**: Given two sorted arrays **a[]** and **b[]** and an element **k**, the task is to find the element that would be at the **k<sup>th</sup>** position of the combined sorted array.

**Example**:

**Input**: `a[] = [2, 3, 6, 7, 9], b[] = [1, 4, 8, 10], k = 5`
**Output:** 6
**Explanation:** `The final combined sorted array would be [1, 2, 3, 4, 6, 7, 8, 9, 10]. The 5th element of this array is 6.`

[Visit GFG](https://www.geeksforgeeks.org/problems/k-th-element-of-two-sorted-array1317/1)

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
	int kthElement(vector<int> &a, vector<int> &b, int k) {
		vector<int> ans = merge(a, b);
		return ans[k - 1];
	}
};
```

---

### Better Approach
By using variables like element and counter

**Time Complexity**: O(m + n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
    int kthElement(vector<int> &a, vector<int> &b, int k) {
        int n = a.size(), m = b.size();

        int left = 0, right = 0;
        int count = 0;
        int element = 0;

        while (left < n && right < m) {
            if (a[left] <= b[right]) {
                element = a[left++];
            } else {
                element = b[right++];
            }

            count++;
            
            if (count == k) {
                return element;
            }
        }

        while (left < n) {
            element = a[left++];
            
            count++;
            
            if (count == k) {
                return element;
            }
        }

        while (right < m) {
            element = b[right++];
            
            count++;
            
            if (count == k) {
                return element;
            }
        }

        return -1;
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
	int kthElement(vector<int> &a, vector<int> &b, int k) {
		int n = a.size(), m = b.size();
		
		if(n > m) {
			return kthElement(b, a, k);
		}
		
		int left = k;
		int low = max(0, k - m), high = min(k, n);
		
		while(low <= high) {
			int mid1 = low + ((high - low) / 2);
			int mid2 = left - mid1;
			
			int l1 = (mid1 > 0) ? a[mid1 - 1] : INT_MIN;
			int l2 = (mid2 > 0) ? b[mid2 - 1] : INT_MIN;
			int r1 = (mid1 < n) ? a[mid1] : INT_MAX;
			int r2 = (mid2 < m) ? b[mid2] : INT_MAX;
			
			if (l1 <= r2 && l2 <= r1) {
				return max(l1, l2);
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
