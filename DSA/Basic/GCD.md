**Problem**: Given two integers `a` and `b`, find their greatest common divisor.

Brute Force:

- Idea
  Check every number from `1` to `min(a, b)`, if the number divides both update the gcd.
- Imp: Check every number from `min(a, b)` to `1`, if the number divides both update the gcd and break the loop. The time complexity will not change as we alway looks for worst-case

Time Complexity: O(min(a, b))
Space Complexity: O(1)

```cpp
class Solution {
  public:
	int gcd(int a, int b) {
		int ans = 1;
	
		for (int i = 1; i <= min(a, b); i++) {
			if (((a % i) == 0) && ((b % i) == 0)) {
				ans = i;
			}
		}
	
		return ans;
	}
};
```

Optimal Approach:

- Idea
  Euclidean Algorithm:
  - gcd(a, b) = gcd(a / b, b). if (a > b)
  - gcd(a, b) = gcd(a, b / a). if (b > a)
  - gcd(a, b) = gcd(a % b, b). if (a > b)
  - gcd(a, b) = gcd(a, b % a). if (b > a)

Time Complexity: O(log (min(a, b)))
Space Complexity: O(1)

```cpp
class Solution {
  public:
	int gcd(int a, int b) {
		while (a > 0 && b > 0) {
			if (a > b) {
				a = a % b;
			} else {
				b = b % a;
			}
		}
	
		return (a == 0) ? b : a;
	}
};
```

