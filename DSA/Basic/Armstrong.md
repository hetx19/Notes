**Problem**: Given an integer, check weather the number is an _Armstrong Number_

Optimal Approach:

- Idea:
  Extract each digit of the number, raise it to the power of the total number of digits, sum the results, and check if the sum equals the original number.

Time Complexity: O(log n)
Space Complexity: O(1)

```cpp
bool isArmStrong(int n) {
	if (n < 0) {
		return false;
	}
	
	if (n == 0) {
		return true;
	}
	
	int len = log10(n) + 1;
	int temp = n;
	int number = 0;
	while (temp != 0) {
		int digit = (temp % 10);
		number += pow(digit, len);
		temp /= 10;
	}
	
	return (n == number);
}
```
