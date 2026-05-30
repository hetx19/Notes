**Problem**: Return the `n`-th Fibonacci number (`0, 1, 1, 2, 3, 5, ...`)
a<sub>n</sub> = a<sub>n - 1</sub> + a<sub>n - 2</sub>

#### Functional / Basic Recursion (Brute Force)

Time Complexity: O(2<sup>n</sup>)
Space Complexity: O(n) → recursion stack

```cpp
class Solution {
  public:
	int fibonacci(int n) {
	    if (n == 0) {
		    return 0;
		}

	    if (n == 1) {
		    return 1;
		}

	    return fibonacci(n - 1) + fibonacci(n - 2);
	}
};
```

Call:

```cpp
int ans = fibonacci(n);
```

---

#### Parameterized Recursion

Time Complexity: O(n)
Space Complexity: O(n) → recursion stack

```cpp
class Solution {
  public:
	int fibonacci(int n, int a = 0, int b = 1) {
	    if (n == 0) {
		    return a;
		}

	    return fibonacci(n - 1, b, a + b);
	}
};
```

Call:

```cpp
int ans = fibonacci(n);
```

---

#### Iterative / Optimal

Time Complexity: O(n)
Space Complexity: O(1)

```cpp
class Solution {
  public:
	int fibonacci(int n) {
	    if (n == 0) {
		    return 0;
		}

	    int a = 0, b = 1;

	    for (int i = 2; i <= n; i++) {
	        int c = a + b;
	        a = b;
	        b = c;
	    }

	    return b;
	}
};
```
