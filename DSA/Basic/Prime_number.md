**Problem**: Given an integer `n`, return if it is a prime or not.

Brute Force Approach

- Idea
  Count the number of divisor, and if it is equals to 2 then retuen true else return false
  Time complexity: O(n)
  Space Complexity: O(1)

```cpp
bool isPrime(int n) {
	if (n <= 1) {
		return false;
	}
	
	int divisor = 0;
	for (int i = 2; i < n; i++) {
		if (n % i == 0) {
			return false;
		}
	}
	
	return true;
}
```

Optimal Approach

- Idea
  Count the number of divisor, and if it is equals to 2 then retuen true else return false

Time complexity: O(√n)
Space Complexity: O(1)

````cpp
bool isPrime(int n) {
    if (n <= 1) {
	    return false;
	}

    if (n == 2) {
	    return true;
    }

    if (n % 2 == 0) {
	    return false;
	}

    for (int i = 3; i * i <= n; i += 2) {
        if (n % i == 0) {
            return false;
        }
    }

    return true;
}```
````

