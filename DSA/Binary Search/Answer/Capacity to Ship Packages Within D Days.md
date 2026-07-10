**Problem**: A conveyor belt has packages that must be shipped from one port to another within `days` days.

The `ith` package on the conveyor belt has a weight of `weights[i]`. Each day, we load the ship with packages on the conveyor belt (in the order given by `weights`). We may not load more weight than the maximum weight capacity of the ship.

Return the least weight capacity of the ship that will result in all the packages on the conveyor belt being shipped within `days` days.

**Example**:
**Input**: `weights = [1,2,3,4,5,6,7,8,9,10], days = 5`
**Output**: 15
**Explanation**: A ship capacity of 15 is the minimum to ship all the packages in 5 days like this:
- 1st day: 1, 2, 3, 4, 5
- 2nd day: 6, 7
- 3rd day: 8
- 4th day: 9
- 5th day: 10

Note that the cargo must be shipped in the order given, so using a ship of capacity 14 and splitting the packages into parts like (2, 3, 4, 5), (1, 6, 7), (8), (9), (10) is not allowed.

[Visit Leetcode](https://leetcode.com/problems/capacity-to-ship-packages-within-d-days/)
[Visit GFG](https://www.geeksforgeeks.org/problems/capacity-to-ship-packages-within-d-days/1)

---
### Brute Force
By using linear Search - [[Linear Search]]

**Time Complexity**: O(n (sum - max + 1)) → O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int calculateDays(vector<int>& weights, int capacity) {
		int days = 1, load = 0;
		
		for (int &weight : weights) {
			if (load + weight > capacity) {
				days++;
				load = weight;
			} else {
				load += weight;
			}
		}
		
		return days;
	}
	
  public:
	int shipWithinDays(vector<int>& weights, int days) {
		int maxi = INT_MIN, sum = 0;
		
		for (int &weight : weights) {
			sum += weight;
			maxi = max(maxi, weight);
		}
		
		for (int i = maxi; i <= sum; i++) {
			int daysRequired = calculateDays(weights, i);
			
			if (daysRequired <= days) {
				return i;
			}
		}
		
		return -1;
	}
};
```

---
### Optimal Solution
By using Binary Search Function - [[Binary Search]]

**Time Complexity**: O(n * log (max - sum + 1)) → O(n log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  private:
	int calculateDays(vector<int>& weights, int capacity) {
		int days = 1, load = 0;
		
		for (int &weight : weights) {
			if (load + weight > capacity) {
				days++;
				load = weight;
			} else {
				load += weight;
			}
		}
		
		return days;
	}
	
  public:
	int shipWithinDays(vector<int>& weights, int days) {
		int maxi = INT_MIN, sum = 0;
		
		for (int &weight : weights) {
			sum += weight;
			maxi = max(maxi, weight);
		}
		
		int low = maxi, high = sum;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int daysRequired = calculateDays(weights, mid);
			
			if (daysRequired <= days) {
				high = mid - 1;
			} else {
				low = mid + 1;
			}
		}
		
		return low;
	}
};
```

---