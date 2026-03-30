**Problem**: Given an integer, return the reverse of the number.

- [Vist Problem](https://leetcode.com/problems/reverse-integer/description/)

Optimal Approach:
Extract digits one by one using `% 10` and build the reversed number by multiplying the current result by `10` and adding the extracted digit until the number becomes `0`.

```cpp
int reverse_number(int n) {
	int ans = 0;
	
	while (n != 0) {
		int digit = (n % 10);
		if (ans > INT_MAX/10 || ans < INT_MIN/10) {
			return 0;
		} else {
			ans = (ans * 10) + digit;
		}
		n = n / 10;
	}
	
	return ans;
}
```

