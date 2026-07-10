**Problem**: Koko loves to eat bananas. There are `n` piles of bananas, the `ith` pile has `piles[i]` bananas. The guards have gone and will come back in `h` hours.

Koko can decide her bananas-per-hour eating speed of `k`. Each hour, she chooses some pile of bananas and eats `k` bananas from that pile. If the pile has less than `k` bananas, she eats all of them instead and will not eat any more bananas during this hour.

Koko likes to eat slowly but still wants to finish eating all the bananas before the guards return.

Return _the minimum integer_ `k` _such that she can eat all the bananas within_ `h` _hours_.

**Example**:
**Input**: `piles = [3,6,7,11], h = 8`
**Output**: 4

[Visit Leetcode](https://leetcode.com/problems/koko-eating-bananas/)
[Visit GFG](https://www.geeksforgeeks.org/problems/koko-eating-bananas/1)

---
### Brute Force
Minimum Speed = 1 banana/sec, Maximum Speed = max_pile/sec
By checking Every possible speed. Linear Search - [[Linear Search]]

**Time Complexity**: O(n * max(piles)) → O(n<sup>2</sup>)
**Time Complexity**: O(1)

```cpp
class Solution {
  private:
	int calculateTime(vector<int>& piles, int speed) {
		int totalHour = 0;
		
		for (int &pile : piles) {
			totalHours += (pile * speed - 1) / speed;
		}
		
		return totalHours;
	}
	
  public:
	int minEatingSpeed(vector<int>& piles, int h) {
		int maxPile = *maxElement(piles.begin(), piles.end());
		
		for (int i = 1; i <= maxPile; i++) {
			int hours = calculateTime(piles, i);
			
			if (hours <= h) {
				return i;
			}
		}
		
		return maxPile;
	}
};
```

---

### Optimal Solution
Search Space is Sorted then - Binary Search between `[1, max_pile]` - [[Binary Search]]

**Time Complexity**: O(n * log (max(piles))) → O(nlog n)
**Time Complexity**: O(1)

```cpp
class Solution {
  private:
	int calculateTime(vector<int>& piles, int speed) {
		int totalHour = 0;
		
		for (int &pile : piles) {
			totalHours += (pile * speed - 1) / speed;
		}
		
		return totalHours;
	}
	
  public:
	int minEatingSpeed(vector<int>& piles, int h) {
		int maxPile = *max(piles.begin(), piles.end());
		int ans = maxPile;
		int low = 1, high = maxPile;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			
			int hours = calculateTime(piles, mid);
			
			if (hours <= h) {
				ans = mid;
				high = mid - 1;
			} else {
				low = mid + 1;
			}
		}
		
		return ans;
	}
};
```