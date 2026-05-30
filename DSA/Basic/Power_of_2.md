**Problem**: Given a number `n`, check if it can be represent in a for 2<sup>x</sup>. 0 <= x < 31.
[Vist_Problem](https://leetcode.com/problems/power-of-two/)

Brute force:

- Idea: Check every power of 2 from `2⁰` to `2³⁰` and see if any equals `n`.

Time Complexity: O(1)
Space Complexity: O(1)

```cpp
class Solution {
  public:
	bool isPowerOfTwo(int n) {
		for(int i = 0; i <= 30; i++) {
			int ans = pow(2,i);
			if (ans == n) {
				return true;
			}
		}
		return false;
	}
};
```

Optimal Approach:
Idea: A power of two has **exactly one bit set** in its binary representation. (Bit Manipulation)

Time Complexity: O(1)
Space Complexity: O(1)

```cpp
class Solution {
  public:
	bool isPowerOfTwo(int n) {
		return (n > 0 && ((n & (n - 1)) == 0);
	}
};
```

Bit Manipulation: [[Bit Manipulation]]

