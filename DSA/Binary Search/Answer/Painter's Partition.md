**Problem**: Given an array **arr[]** where each element denotes the length of a board, and an integer **k** representing the number of **painters** available. Each painter takes **1** unit of time to paint **1 unit length** of a board.

Determine the **minimum** amount of time required to paint all the boards, under the constraint that each painter can paint only a **contiguous** sequence of boards (no skipping or splitting allowed).

**Example**:
**Input**: `arr[] = [5, 10, 30, 20, 15], k = 3`
**Output**: 35
**Explanation**: `The optimal allocation of boards among 3 painters is - Painter 1 → [5, 10] → time = 15`
- `Painter 2 → [30] → time = 30`
- `Painter 3 → [20, 15] → time = 35`
Job will be done when all painters finish i.e. at time = max(15, 30, 35) = 35

[Visit GFG](https://www.geeksforgeeks.org/problems/split-array-largest-sum--141634/1)

---
### Brute Force Solution
By using Linear Search function - [[Linear Search]]

**Time Complexity**: O(n * (sum - max + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution { 
  private:
	int countPainters(vector<int>& boards, int time) {
		int painters = 1, boardPainter = 0;
		
		for (int &board : boards) {
			if (boardPainter + board <= time) {
				boardPainter += board;
			} else {
				painters++;
				boardPainter = board;
			}
		}
		
		return painters;
	}
	
  public:
	int minTime(vector<int>& arr, int k) {
		int low = *max_element(arr.begin(), arr.end());
		int high = accumulate(arr.begin(), arr.end(), 0);
		
		for (int i = low; i <= high; i++) {
			if (countPainters(arr, i) <= k) {
				return i;
			}
		}
		
		return low;
	}
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: **Time Complexity**: O(n * log(sum - max + 1)) → O(n logn)
**Space Complexity**: O(1)

```cpp
class Solution { 
  private:
	int countPainters(vector<int>& boards, int time) {
		int painters = 1, boardPainter = 0;
		
		for (int &board : boards) {
			if (boardPainter + board <= time) {
				boardPainter += board;
			} else {
				painters++;
				boardPainter = board;
			}
		}
		
		return painters;
	}
	
  public:
	int minTime(vector<int>& arr, int k) {
		int low = *max_element(arr.begin(), arr.end());
		int high = accumulate(arr.begin(), arr.end(), 0);
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int painters = countPainters(arr, mid);
			
			if (painters > k) {
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
