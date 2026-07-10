**Problem**: You are given an array with unique elements of **stalls[]**, which denote the positions of **stalls**. You are also given an integer **k** which denotes the number of aggressive cows. The task is to assign **stalls** to **k** cows such that the **minimum distance** between any two of them is the **maximum** possible.

**Example**:
**Input**: `stalls[] = [1, 2, 4, 8, 9], k = 3`
**Output**: 3
**Explanation**: 
`The first cow can be placed at stalls[0]`,
`the second cow can be placed at stalls[2],`
`the third cow can be placed at stalls[3].`
The minimum distance between cows in this case is 3, which is the largest among all possible ways.

[Visit GFG](https://www.geeksforgeeks.org/problems/aggressive-cows/1)

---

### Brute Force Solution
By using Linear Search function - [[Linear Search]]
 
**Time Complexity**: O(n logn) + O(n * (max - min)) → O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	bool canWePlace(vector<int>& stalls, int distance, int cows) {
		int n = stalls.size(), countCows = 1;
		int last = stalls[0];
		
		
		for (int i = 1; i < n; i++) {
			if (stalls[i] - last >= distance) {
				countCows++;
				last = stalls[i];
			}
			
			if (countCows >= cows) {
				return true;
			}
		}
		
		return false;
	}
	
  public:
	int aggressiveCows(vector<int>& stalls, int k) {
		sort(stalls.begin(), stalls.end());
		int n = stalls.size();
		int limit = stalls[n - 1] - stalls[0];
		
		for (int i = 0; i <= limit; i++) {
			if (!canWePlace(stalls, i, k)) {
				return (i - 1);
			}
		}
		
		return limit;
	}
};
```

---
### Optimal Solution
By using Binary Search function - [[Binary Search]]

**Time Complexity**: O(n logn) + O(n * log(max - min)) → O(n log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	bool canWePlace(vector<int> &stalls, int distance, int cows) {
		int n = stalls.size(), countCows = 1;
		int last = stalls[0];
		
		
		for (int i = 1; i < n; i++) {
			if (stalls[i] - last >= distance) {
				countCows++;
				last = stalls[i];
			}
			
			if (countCows >= cows) {
				return true;
			}
		}
		
		return false;
	}
	
  public:
	int aggressiveCows(vector<int>& stalls, int k) {
		sort(stalls.begin(), stalls.end());
		int n = stalls.size();
		int low = 1, high = stalls[n - 1] - stalls[0];
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (canWePlace(stalls, mid, k)) {
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return high;
	}
};
```

---