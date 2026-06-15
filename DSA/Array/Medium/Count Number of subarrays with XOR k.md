**Problem**: Given an array of integers **arr[]** and a number **k**, count the number of subarrays having **XOR** of their elements as **k**.

**Note:** It is guranteed that the total count will fit within a 32-bit integer.

**Example**:

**Input**: `arr[] = [4, 2, 2, 6, 4], k = 6`
**Output**: 4  
**Explanation**: The subarrays having XOR of their elements as 6 are `[4, 2], [4, 2, 2, 6, 4], [2, 2, 6], and [6]`. Hence, the answer is 4.

[Visit GFG](https://www.geeksforgeeks.org/problems/count-subarray-with-given-xor/1)

---

### Brute Force
By Generating all subarrays

**Time Complexity**: O(n<sup>3</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int subarrayXor(vector<int>& arr, int k) {
		int count = 0, n = arr.size();
		
		for (int i = 0; i < n; i++) {
			for (int j = i; j < n; j++) {
				int XOR = 0;
				for (int k = i; k <= j; k++) {
					XOR ^= arr[k];
				}
				
				if (XOR == k) {
					count++;
				}
			}
		}
		
		return count;
	}
};
```

---

### Better Solution
Betterment in Brute

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int subarrayXor(vector<int>& arr, int k) {
		int count = 0, n = arr.size();
		
		for (int i = 0; i < n; i++) {
			int XOR = 0;
			for (int j = i; j < n; j++) {
				XOR ^= arr[j];
				
				if (XOR == k) {
					count++;
				}
			}
		}
		
		return count;
	}
};
```

---

### Optimal Solution
By using concept of hashing - [[Hashing]]

**Time Complexity**: O(n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	int subarrayXor(vector<int>& arr, int k) {
		int n = arr.size(), count = 0, preXOR = 0;
		unordered_map<int, int> mpp;
		
		mpp[perXOR]++;
		
		for (int i = 0; i < n; i++) {
			preXOR ^= arr[i];
			int y = preXOR ^ k;
			
			count += mpp[y];
			
			mpp[preXOR] += 1;
		}
		
		return count;
	}
};
```

---