**Problem**: Given an integer `n`, return if it is a palindrome or not.

- [Vist Problem](https://leetcode.com/problems/palindrome-number/description/)

Optimal Approach

- Idea
  Find the reverse of the number `n` and check if it is equal to the original number

Time complexity: O(n)
Space Complexity: O(1)

```cpp
class Solution {
  public:
	bool isPalindrome(int n) {
		if (n < 0) {
			return fasle;
		}
	
		int temp = n;
		int rev = 0;
	
		while (temp != 0) {
			int digit = temp % 10;
			if (rev > INT_MAX / 10) {
				return false;
			} else {
				rev = (rev * 10) + digit;
			}
			temp = temp / 10;
		}
	
		return (n == rev);
	}
};
```

