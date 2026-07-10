**Problem**: You are given 2 numbers **n and m,** the task is to find **n√m** (nth root of m). If the root is not integer then return **-1**.

**Example**:

**Input**: n = 3, m = 8
**Output**: 2
**Explanation**: 2<sup>3</sup> = 8

[Visit GFG](https://www.geeksforgeeks.org/problems/find-nth-root-of-m5843/1)

---

### Brute Force
By doing Linear Search - [[Linear Search]]
Only Applicable for n > 0

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int nthRoot(int n, int m) {
	    for (int i = 0; i <= m; i++) {
		    int power = pow(i, n);
		    
		    if (power == m) {
			    return i;
		    } else if (power > m) {
			    break;
			}
	    }
	    
	    return -1;
    }
};
```

---

### Optimal Solution
By doing Binary Search - [[Binary Search]]
Only Applicable for n > 0

**Time Complexity**: O(log m)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
    int nthRoot(int n, int m) {
	    int low = 1, high = m;
	    
	    while (low <= high) {
		    int mid = low + ((high - low) / 2);
		    int ans = 1;
		    
		    fot (int i = 0; i < n; i++) {
			    ans *= mid;
			    
			    if (ans > m) {
				    break;
			    }
		    }
		    
		    if (ans == m) {
			    return mid;
		    } else if (ans < m) {
			    low = mid + 1;
		    } else {
			    high = mid - 1;
		    }
	    }
	    
	    return -1;
    }
};
```

---
