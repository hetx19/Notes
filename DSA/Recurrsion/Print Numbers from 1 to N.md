**Problem**: Write a recursive program to print numbers from `1` to `n`

Time complexity: O(n)
Space complexity: O(n) → Recursive Stack Space

### Approach 1: Normal Recursion

```cpp
class Solution {
  public:
	void printNumber(int i, int n) {
		if (i > n) {
			return;
		}

		cout << i << "\n";
		printNumber(i + 1, n);
	}
};
```

Call it like

```cpp
printNumber(1, 5);
```

### Approach 2: By using Backtrack

Problem: Write a recursive program to print numbers from `1` to `n`. but you cannot increment any variable.

Time complexity: O(n)
Space complexity: O(n) → Recursive Stack Space

```cpp
class Solution {
  public:
	void printNumber(int n) {
		if (n == 0) {
			return;
		}
		printNumber(n - 1);
		cout << n << "\n";
	}
};
```

Call it like

```cpp
printNumber(5);
```

---

Problem: Write a recursive program to print numbers from `n` to `1`

Time complexity: O(n)
Space complexity: O(n) → Recursive Stack Space

### Approach 1: Normal Recursion

```cpp
class Solution {
  public:
	void printNumber(int n) {
		if (n == 0) {
			return;
		}

		cout << n << "\n";
		printNumber(n - 1);
	}
};
```

Call it like

```cpp
printNumber(5);
```

### Approach 2: By using Backtrack

Problem: Write a recursive program to print numbers from `1` to `n`. but you cannot decrement any variable.

Time complexity: O(n)
Space complexity: O(n) → Recursive Stack Space

```cpp
class Solution {
  public:
	void printNumber(int i, int n) {
		if (i > n) {
			return;
		}
		printNumber(i + 1, n);
		cout << i << "\n";
	}
};
```

Call it like

```cpp
printNumber(1, 5);
```
