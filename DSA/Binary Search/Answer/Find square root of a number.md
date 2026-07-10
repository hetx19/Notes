**Problem**: Given a positive integer **n,** find the **square root** of n. If **n** is not a perfect square, then return the **floor value**.

**Floor value** of any number is the greatest Integer which is less than or equal to that number.

**Example**:
Bu
**Input**: n = 4
**Output**: 2
**Explanation**: Since, 4 is a perfect square, so its square root is 2.

[Visit GFG](https://www.geeksforgeeks.org/problems/square-root/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int sqRoot(int n) {
		int ans = 0;
		
		for (int i = 2; i < n; i++) {
			int sqValue = i * i;
			
			if (sqValue <= n) {
				ans = i;
			} else {
				break;
			}
		}
		
		return ans;
	}
}
```

---

### Optimal Solution
By doing Binary Search - [[Binary Search]]

**Time Complexity**: O(log n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int sqRoot(int n) {
		if (n == 0 || n == 1) {
			return n;
		}
	
		int ans = 0;
		int low = 1, high = n;
		
		while (low <= high) {
			int mid = low + ((high - low) / 2);
			int sqValue = mid * mid;
			
			if (sqValue <= x) {
				ans = mid;
				low = mid + 1;
			} else {
				high = mid - 1;
			}
		}
		
		return ans;
	}
}
```

---
