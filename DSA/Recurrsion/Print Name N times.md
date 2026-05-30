**Problem**: Write a recursive program to print name n times

Time complexity: O(n)
Space complexity: O(n) → Recursive Stack Space

i → starts from 0

```cpp
class Solution {
	void printName(string name, int i, int n) {
		if (i == n) {
			return;
		}

		cout << name << "\n";
		printName(name, i + 1, n);
	}
};
```

Call it like

```cpp
printName("Het", 0, 5);
```
