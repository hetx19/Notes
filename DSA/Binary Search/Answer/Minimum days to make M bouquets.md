**Problem**: You are given an integer array `bloomDay`, an integer `m` and an integer `k`.

You want to make `m` bouquets. To make a bouquet, you need to use `k` **adjacent flowers** from the garden.

The garden consists of `n` flowers, the `ith` flower will bloom in the `bloomDay[i]` and then can be used in **exactly one** bouquet.

Return _the minimum number of days you need to wait to be able to make_ `m` _bouquets from the garden_. If it is impossible to make m bouquets return `-1`.

**Example**:

**Input**: `bloomDay = [1,10,3,10,2], m = 3, k = 1`
**Output**: 3
**Explanation**: Let us see what happened in the first three days. x means flower bloomed and _ means flower did not bloom in the garden.
We need 3 bouquets each should contain 1 flower.
`After day 1: [x, _, _, _, _]   // we can only make one bouquet.`
`After day 2: [x, _, _, _, x]   // we can only make two bouquets.`
`After day 3: [x, _, x, _, x]   // we can make 3 bouquets. The answer is 3.`

[Visit Leetcode](https://leetcode.com/problems/minimum-number-of-days-to-make-m-bouquets/)
[Visit GFG](https://www.geeksforgeeks.org/problems/minimum-days-to-make-m-bouquets/1)

---
### Brute Force
Minimum days = Minimum Bloom Day, Maximum days = Maximum Bloom Day
By checking Every possible day. Linear Search - [[Linear Search]]

**Time Complexity**: O(n * (max - min + 1) → O(n<sup>2</sup>)
**Time Complexity**: O(1)

```cpp
class Solution {
  private:
	bool isPossible(vector<int>& bloomDay, int m, int k, int day) {
		int count = 0, numberOfBouquets = 0;
		
		for (int &bDay : bloomDay) {
			if (bDay <= day) {
				count++;
			} else {
				numberOfBouquets += (count / k);
				count = 0;
			}
		}
		
		numberOfBouquets += (count / k);
		
		return numberOfBouquets >= m;
	}
	
  public:
	int minDays(vector<int>& bloomDay, int m, int k) {
		int n = bloomDay.size();
		int totalFlowersRequired = m * k;
		
		if (totalFlowersRequired > n) return -1;
		
		int mini = INT_MAX, maxi = INT_MIN;
		
		for (int i = 0; i < n; i++) {
			mini = min(mini, bloomDay[i]);
			maxi = max(maxi, bloomDay[i]);
		}
		
		for (int i = mini; i <= maxi; i++) {
			if (isPossible(bloomDay, m, k, i)) {
				return i;
			}
		}
		
		return -1;
	}
};
```

---

### Optimal Solution
Search Space is Sorted then - Binary Search between `[Min Day, Max Day]` - [[Binary Search]]

**Time Complexity**: O(n * log (max - min + 1))
**Time Complexity**: O(1)

```cpp
class Solution {
  private:
	bool isPossible(vector<int>& bloomDay, int m, int k, int day) {
		int count = 0, numberOfBouquets = 0;
		
		for (int &bDay : bloomDay) {
			if (bDay <= day) {
				count++;
			} else {
				numberOfBouquets += (count / k);
				count = 0;
			}
		}
		
		numberOfBouquets += (count / k);
		
		return numberOfBouquets >= m;
	}
	
  public:
	int minDays(vector<int>& bloomDay, int m, int k) {
		int n = bloomDay.size();
		int totalFlowersRequired = m * k;
		
		if (totalFlowersRequired > n) return -1;
		
		for (int i = 0; i < n; i++) {
			mini = min(mini, bloomDay[i]);
			maxi = max(maxi, bloomDay[i]);
		}
		
		int low = mini, high = maxi;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			if (isPossible(bloomDay, m, k, mid)) {
				high = mid - 1;
			} else {
				low = mid + 1;
			}
		}
		
		return low;
	}
};
```