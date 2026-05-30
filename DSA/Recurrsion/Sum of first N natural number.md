**Problem**: You are given an natural number `n`. return the sum of first `n` natural number.

#### Brute Force:

Idea: Use the Recursion

Time complexity: O(n)
Space complexity: O(n) → Recursion Stack Space

###### Parameterize Way

```cpp
class Solution {
  public:
	int sumOfN(int i, int sum) {
	    if (i == 0) {
	        return sum;
	    }

	    return sumOfN(i - 1, sum + i);
	}
};
```

Call this like

```cpp
int ans = sumOfN(n, 0);
```

###### Formal Way

```cpp
class Solution {
  public:
	int sumOfN(int n) {
	    if (n == 0) {
	        return 0;
	    }

	    return n + sumOfN(n - 1);
	}
};
```

Call this like

```cpp
int ans = sumOfN(n);
```

---

#### Better Approach:

Idea: By using a simple for loop

Time complexity: O(n)
Space complexity: O(1)

```cpp
class Solution {
  public:
	int sumOfN(int n) {
		int sum = 0;

		for (int i = 1; i <= n; i++) {
			sum += i;
		}

		return sum;
	}
};
```

---

#### Optimal Approach:

Idea: By using Basic maths sum = n(n + 1) / 2

Time complexity: O(1)
Space complexity: O(1)

```cpp
class Solution {
  public:
	int sumOfN(int n) {
		return (n * (n + 1) / 2);
	}
};
```

