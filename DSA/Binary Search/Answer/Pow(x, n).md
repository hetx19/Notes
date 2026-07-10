**Problem**: Implement `pow(x, n)`, which calculates `x` raised to the power `n` (i.e., x<sup>n</sup>).

**Example**:

**Input**: x = 2.00000, n = 10
**Output**: 1024.00000

[Visit Leetcode](https://leetcode.com/problems/powx-n/)

---

### Optimal Solution
Power - [[Find Nth root of a number]]

**Time Complexity**: O(log m)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	double myPow(double x, int n) {
		if(n < 0) {
			x = 1 / x;
		}
		
		int pow = labs(n);
		long double ans = 1;
		
		while(pow != 0) {
			if((pow & 1) != 0) {
				ans *= x;
			}
			x *= x;
			pow = pow >> 1;
		}
		
		return ans;
	}
};
```