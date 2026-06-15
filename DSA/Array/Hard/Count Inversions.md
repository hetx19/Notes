**Problem**: Given an array of integers **arr[]**. You have to find the **Inversion Count** of the array. **Note :** Inversion count is the number of pairs of elements (i, j) such that i < j and `arr[i]` > `arr[j]`.

**Input**: `arr[] = [2, 4, 1, 3, 5]`
**Output**: 3
**Explanation**: The sequence 2, 4, 1, 3, 5 has three inversions (2, 1), (4, 1), (4, 3).

[Visit GFG](https://www.geeksforgeeks.org/problems/inversion-of-array-1587115620/1)

---

### Brute Force
By generating all the pairs

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int inversionCount(vector<int> &arr) {
	    int n = arr.size();
	    int count = 0;
	    
	    for (int i = 0; i < n; i++) {
		    for (int j = i + 1; j < n; j++) {
			    if (arr[i] > arr[j]) {
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
	int merge(vector<int>& arr, int low, int mid, int high) {
		vector<int> temp;
		int left = low, right = mid + 1;
		int count = 0;
		
		while (left <= mid && right <= high) {
			if (arr[left] <= arr[right]) {
				temp.push_back(arr[left++]);
			} else {
				temp.push_back(arr[right++]);
				count += (mid - left + 1);
			}
		}
		
		while (left <= mid) {
			temp.push_back(arr[left++]);
		}
		
		while (right <= high) {
			temp.push_back(arr[right++]);
		}
		
		for (int i = low; i <= high; i++) {
			arr[i] = temp[i - low];
		}
		
		return count;
	}
	
	int mergeSort(vector<int>& arr, int low, int high) {
		int count = 0;
		
		if (low >= high) {
			return count;
		}
		
		int mid = low + ((high - low) / 2);
		count += mergeSort(arr, low, mid);
		count += mergeSort(arr, mid + 1, high);
		count += merge(arr, low, mid, high);
		
		return count;
	}
	
  public:
    int inversionCount(vector<int> &arr) {
	    return mergeSort(arr, 0, arr.size() - 1);
    }
};
```