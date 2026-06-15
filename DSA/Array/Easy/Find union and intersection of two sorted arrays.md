**Problem**: Given two sorted arrays, **arr1** and **arr2** of size **n** and **m**. Find the union and intersection of two sorted arrays.
The union of two arrays can be defined as the common and distinct elements in the two arrays.  
**NOTE**: Elements in the union should be in ascending order.

**Example**:
**Input**: n = 5, m = 5, `arr1[] = {1,2,3,4,5}`, `arr2[] = {2,3,4,4,5}`
**Output**: {1,2,3,4,5} 
**Explanation**: 
Common Elements in arr1 an arr2 are: {2, 3, 4, 5}.
Distinct Elements in arr1 are : 1
Distinct Elements in arr2 are : No distinct elements.
Union of arr1 and arr2 is {1, 2, 3, 4, 5}
Intersection of arr1 and arr2 is {2, 3, 4, 5}

---

### Brute force Approach
By using a set data structure - [[Set]]

1). Union
**Time Complexity**: O((n + m) log(n + m))
**Space Complexity**: O(2 * (m + n)) → O(m + n)

```cpp
class Solution {
  public:
	vector<int> findUnion(int n, int m, vector<int> &arr1, vector<int> &arr2) {
		set<int> st;
		
		for (int i = 0; i < n; i++) {
			st.insert(arr1[i]);
		}
		
		for (int i = 0; i < m; i++) {
			st.insert(arr2[i]);
		}
		
		vector<int> unionArr;
		
		for (auto &it : st) {
			unionArr.push_back(it);
		}
		
		return unionArr;
	}
};
```

2). Intersection
**Time Complexity**: O(nm)
**Space Complexity**: O(min(m, n)) → O(n)

```cpp
class Solution {
  public:
	vector<int> findIntersection(int n, int m, vector<int> &arr1, vector<int> &arr2) {
		if (n > m) {
			return findIntersection(m, n, arr2, arr1);
		}
		
		vector<int> intersectionArr;
		vector<bool> visited(m, false);
		
		for (int i = 0; i < n; i++) {
			for (int j = 0; j < m; j++) {
				if (arr1[i] == arr2[j] && !visited[j]) {
					intersectionArr.push_back(arr1[i]);
					visited[j] = true;
				}
				
				if (arr1[i] < arr2[j]) {
					break;
				}
			}
		}
		
		return intersectionArr;
	}
};
```

---

### Optimal Approach
By using two pointer approach

1). Union

**Time Complexity**: O(n + m)
**Space Complexity**: O(m + n)

```cpp
class Solution {
  public:
	vector<int> FindUnion(int n, int m, vector<int> &arr1, vector<int> &arr2) {
		vector<int> unionArray;
		int i = 0, j = 0;
		
		while (i < n && j < m) {
			if (arr1[i] <= arr2[j]) {
				if (unionArray.size() == 0 || unionArray.back() != arr1[i]) {
					unionArray.push_back(arr1[i]);
				}
				i++;
			} else {
				if (unionArray.size() == 0 || unionArray.back() != arr2[j]) {
					unionArray.push_back(arr2[j]);
				}
				j++;
			}
		}
		
		while (i < n) {
			if (unionArray.size() == 0 || unionArray.back() != arr1[i]) {
				unionArray.push_back(arr1[i]);
			}
			i++;
		}
		
		while (j < m) {
			if (unionArray.size() == 0 || unionArray.back() != arr2[j]) {
				unionArray.push_back(arr2[j]);
			}
			j++;
		}
		
		return unionArray;
	}
};
```

2). Intersection

**Time Complexity**: O(n + m)
**Space Complexity**: O(min(m, n)) → O(n)

```cpp
class Solution {
  public:
	vector<int> findIntersection(int n, int m, vector<int> &arr1, vector<int> &arr2) {
		vector<int> intersectionArray;
		int i = 0, j = 0;
		
		while (i < n && j < m) {
			if (arr1[i] < arr2[j]) {
				i++;
			} else if (arr1[i] > arr2[j]) {
				j++;
			} else {
				if (intersectionArray.empty() || intersectionArray.back() != arr1[i])
					intersectionArray.push_back(arr1[i]);
				}
				i++;
				j++;
			}
		}
		
		return intersectionArray;
	}
};
```