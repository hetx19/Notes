**Problem**: Given an array **arr[]** of integers, where each element **arr[i]** represents the number of pages in the i-th book. You also have an integer **k** representing the number of students. The task is to allocate books to each student such that:

- Each student receives atleast one book.
- Each student is assigned a contiguous sequence of books.
- No book is assigned to more than one student.
- All books must be allocated.

The objective is to **minimize the maximum number of pages** assigned to any student. In other words, out of all possible allocations, find the arrangement where the student who receives the most pages still has the **smallest possible maximum**.

**Note:** If it is not possible to allocate books to all students, return **-1**.

**Example**:
**Input**: `arr[] = [12, 34, 67, 90], k = 2`
**Output**: 113
**Explanation**: Allocation can be done in following ways:  
-  `[12] and [34, 67, 90] Maximum Pages = 191`
-  `[12, 34] and [67, 90] Maximum Pages = 157`
-  `[12, 34, 67] and [90] Maximum Pages = 113.`
The third combination has the minimum pages assigned to a student which is 113.

[Visit GFG](https://www.geeksforgeeks.org/problems/allocate-minimum-number-of-pages0937/1)

---

### Brute Force Solution
By using Linear Search function - [[Linear Search]]

**Time Complexity**: O(n * (sum - max + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int countStudents(vector<int>& arr, int pages) {
		int student = 1, StdPages = 0;
		
		for (int num : arr) {
			if (num + StdPages > pages) {
				student++;
				StdPages = num;
			} else {
				StdPages += num;
			}
		}
		
		return student;
	}
	
  public:
	int findPages(vector<int>& arr, int k) {
	    int n = arr.size();
	    
	    if (k > n) return -1;
	    
        int low = *max_element(arr.begin(), arr.end());
        int high = accumulate(arr.begin(), arr.end(), 0);
        
        for (int i = low; i <= high; i++) {
	        if (countStudents(arr, i) <= k) {
		        return i;
	        }
        }
        
        return low;
    }
};
```

---
### Optimal Solution
By using Binary Search function - [[Binary Search]]

**Time Complexity**: O(n * log(sum - max + 1)) → O(n log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int countStudents(vector<int>& arr, int pages) {
		int student = 1, StdPages = 0;
		
		for (int num : arr) {
			if (num + StdPages > pages) {
				student++;
				StdPages = num;
			} else {
				StdPages += num;
			}
		}
		
		return student;
	}
	
  public:
	int findPages(vector<int>& arr, int k) {
		int n = arr.size();
		
		if (k > n) return -1;
		
        int low = *max_element(arr.begin(), arr.end());
        int high = accumulate(arr.begin(), arr.end(), 0);
        
        while (low <= high) {
	        int mid = low + ((high - low) / 2);
	        
	        int students = countStudents(arr, mid);
	        
	        if (students > k) {
		        low = mid + 1;
	        } else {
		        high = mid - 1;
	        }
        }
        
        return low;
    }
};
```

---