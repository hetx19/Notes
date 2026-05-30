**Problem**: Check whether a string is a palindrome using recursion.

Time Complexity: O(n)  
Space Complexity: O(n) → recursive stack

#### Parameterized / With Indices

```cpp
class Solution {
  public:
	bool isPalindrome(string &s, int i, int j) {
	    if (i >= j) {
		    return true;
		}

	    if (s[i] != s[j]) {
		    return false;
		}

	    return isPalindrome(s, i + 1, j - 1);
	}
};
```

Call:

```cpp
bool ans = isPalindrome(str, 0, str.length() - 1);
```

---

Time Complexity: O(n)  
Space Complexity: O(n) → recursive stack

#### Functional / Backtracking Style

```cpp
class Solution {
  public:
	bool isPalindrome(string &s, int i = 0) {
	    int n = s.size();
	    if (i >= n / 2) {
		    return true;
		}

	    return (s[i] == s[n - i - 1]) && isPalindrome(s, i + 1);
	}
};
```

Call:

```cpp
bool ans = isPalindrome(str);
```

---

Time Complexity: O(n)  
Space Complexity: O(1)

#### Iterative / Optimal

```cpp
class Solution {
  public:
	bool isPalindrome(string &s) {
	    int i = 0, j = s.length() - 1;
	    while (i < j) {
	        if (s[i] != s[j]) return false;
	        i++;
	        j--;
	    }
	    return true;
	}
};
```
