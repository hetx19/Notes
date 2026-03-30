**Problem**: Given an integer `n`, print or return all of its divisors.

Brute Force:

- Idea
  Check every number from `1` to `n` and print the number if it divides `n`.

Time Complexity: O(n)
Space Complexity: O(1) // For Printing
Space Complexity: O(Number of divisors) // For Storing

```cpp
void printDivisor(int n) {
	for (int i = 1; i <= n; i++) {
		if (n % i == 0) {
			cout << i << " ";
		}
	}
	cout << "\n";
}
```

Optimal Approach:

- Idea
  If `i` divides `n`, then `n / i` also divides `n`.

Time Complexity: O(√n)
Space Complexity: O(1) // For Printing
Space Complexity: O(Number of divisors) // For Storing

```cpp
void printDivisor(int n) {
	for (int i = 1; i * i <= n; i++) {
		if (n % i == 0) {
			cout << i << " ";
			if ((n / i) != i) {
				cout << n / i << " ";
			}
		}
	}
	cout << "\n";
}
```

##### For storing the divisor the first approach is the optimal

Time Complexity: O(n)
Space Complexity: O(Number of Divisior)

###### Why this is optimal??

The √n approach does **not** guarantee sorted order.

