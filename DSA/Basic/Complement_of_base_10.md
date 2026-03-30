**Problem**: The **complement** of an integer is the integer you get when you flip all the `0`'s to `1`'s and all the `1`'s to `0`'s in its binary representation.

For Example: 5 -> `101` complement of `101` -> `010` -> `2`
[Vist_Problem](https://leetcode.com/problems/complement-of-base-10-integer/)

Optimal Approach:

- Idea:
  Use bitwise not operator with a mask

Time Complexity: O(log n)
Space Complexity: O(1)

```cpp
int bitwiseComplement(int n) {
	if (n == 0) {
		return 1;
	}
	
	int temp = n;
	int mask = 0;
	
	while (temp != 0) {
		mask = (mask << 1) | 1;
		temp = temp >> 1;
	}
	
	return ((~n)&mask);
}
```
