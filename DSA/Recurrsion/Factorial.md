**Problem**: You are given a number `n`. return the factorial of `n`.

#### Brute Force:

Idea: Use the Recursion

Time complexity: O(n)
Space complexity: O(n) → Recursion Stack Space

###### Parameterize Way

```cpp
class Solution {
  public:
	long long factorialN(int i, long long factorial) {
	    if (i <= 1) {
	        return factorial;
	    }

	    return factorialN(i - 1, 1LL * factorial * i);
	}
};
```

Call this like

```cpp
long long ans = factorialN(n, 1);
```

###### Formal Way

```cpp
class Solution {
  public:
	long long factorialN(int n) {
	    if (n == 0 || n == 1) {
	        return 1;
	    }

	    return 1LL * n + factorialN(n - 1);
	}
};
```

Call this like

```cpp
long long ans = factorialN(n);
```

---

#### Optimal Approach:

Idea: By using a simple for loop

Time complexity: O(n)
Space complexity: O(1)

```cpp
class Solution {
  public:
	long long factorialN(int n) {
		long long factorial = 1;

		for (int i = 1; i <= n; i++) {
			factorial *= i;
		}

		return factorial;
	}
};
```

