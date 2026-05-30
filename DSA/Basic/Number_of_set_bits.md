**Problem**: You are given integer `n`. return number of `1` in its binary representation of `n`.

Brute Force:

- Idea: Check every bit of `n` and count how many bits are set to `1`.

Time Complexity: O(log n) (number of bits in `n`)
Space Complexity: O(1)

```cpp
class Solution {
  public:
	int hammingWeight(int n) {
		int count = 0;
	
		while (n > 0) {
			if (n & 1) {
				count++;
			}
			n >>= 1;
		}
	
		return count;
	}
};
```

Optimal Approach: (Brian Kernighan’s Algorithm)

- Idea: Each operation `n = n & (n - 1)` removes the lowest set bit, so count how many times we can do this until `n` becomes `0`.

Time Complexity: O(k) where `k` is the number of set bits
Space Complexity: O(1)

```cpp
class Solution {
  public:
	int hammingWeight(int n) {
		int count = 0;
	
		while (n > 0) {
			count++;
			n = n & (n - 1);
		}
	
		return count;
	}
};
```

**_Important Note_**
To calculate the number of `1` bits in the binary representation of `n`, C++ provides a built-in function:

_Syntax_

```cpp
__builtin_popcount(unsigned int n);
```

Time Complexity: O(1)

