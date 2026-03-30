**Problem**: Given an integer, return the number of digits in the number.

Brute Force Approach:

- Idea:
  Repeatedly divide the number by 10 until it becomes 0.  
  Count how many times you perform the division — that count is the number of digits.

Time Complexity: O(log n) // Base 10
Space Complexity: O(1)

```cpp
int countDigits(int n) {
	if (n == 0) {
		return 1;
	}
	
	n = abs(n);
	
	int temp = n;
	int digits = 0;
	
	while (temp != 0) {
		digits++;
		temp = temp / 10;
	}
	
	return digits;
}
```

Optimal Approach:

- Idea:
  By simple Math: no_of_digit = (log<sub>10</sub> (number) + 1)

Time Complexity: O(1)
Space Complexity: O(1)

```cpp
int countDigits(int n) {
	if (n == 0) {
		return 1;
	}
	
	n = abs(n);
	
	int digits = log10(n) + 1;
	
	return digits;
}
```

