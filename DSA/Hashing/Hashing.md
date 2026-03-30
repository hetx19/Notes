## What is hashing and why it is required??

Hashing is a technique used to store and retrieve data efficiently using a special function called a hash function.

A hash function converts a key (like a number or string) into an index of an array, called a hash table.

Instead of searching linearly O(n), hashing allows direct access to the data in O(1) average time.

#### Basic Concept

```code
Key → Hash Function → Index → Hash Table
```

Example: Given an array `arr`, return the frequency of every element in the array.

#### Brute force Approach

Idea: Two nested for loops

Time Complexity: O(n<sup>2</sup>)
Space Complexity: O(M) : m → Number of unique Elements
O(m) → space is used for returning the answer

```cpp
int countFreq(const vector<int>& arr, int number) {
	int count = 0;

	for (int i = 0; i < arr.size(); i++) {
		if (arr[i] == number) {
			count++;
		}
	}

	return count;
}

vector<pair<int, int>> frequencies(vector<int>& arr) {
	vector<pair<int, int>> ans;

	for (int i = 0; i < arr.size(); i++) {
		bool alreadyCalculated = false;

		for (int j = 0; j < i; j++) {
			if (arr[j] == arr[i]) {
				alreadyCalculated = true;
				break;
			}
		}

		if (alreadyCalculated == true) {
			continue;
		}

		ans.push_back({arr[i], countFreq(arr, arr[i])});
	}

	return ans;
}
```

#### Better Approach

Idea: By using concept of hashing (hash Array)

Time Complexity: O(n) + O(maxElement)
Space Complexity: O(2 \* maxElement)
O(maxElement) → space is used for returning the answer
O(maxElement) → space is used for pre Calculation

```cpp
vector<pair<int, int>> frequencies(vector<int>& arr) {
	int n = arr.size();
	int maxElement = *max_element(arr.begin(), arr.end());

	// Hash Array
	vector<int> hash(maxElement + 1, 0);
	vector<pair<int, int>> ans;

	for (int i = 0; i < n; i++) {
		hash[arr[i]]++;
	}

	for (int i = 0; i <= maxElement; i++) {
		if (hash[i] > 0) {
			ans.push_back({i, hash[i]});
		}
	}

	return ans;
}
```

## Important Points

- Limitation of Hash Array
  In the above example, we used a **hash array** to store frequencies.
  1. In the above example we use concept of hash array, but it is not always a good approach because arrays have fixed size. If the maximum element is very large, creating such a large array is not memory efficient..
  2. If the maximum element is very large (e.g., 10⁹), creating a hash array of size `10⁹ + 1` is not feasible.
  3. It wastes memory when the input range is large but the number of elements is small.

  Therefore, using a hash array is practical only when:
  - The elements are non-negative.
  - The range of values is small.

#### The better way is to use a map / unorderedMap.

Map - [[Map]]
Unordered map - [[Unordered Map]]
Syntax:

```cpp
unordered_map<data_type_of_key, data_type_of_value> name;
map<data_type_of_key, data_type_of_value> name;
```

#### How to use map in above example?

By using a map data structure

For unordered_map:
Time complexity: O(n) → Best and average case
Time complexity: O(n<sup>2</sup>) → Worst-case → If there is a hash collision

For map:
Time complexity: O(n log n)

Space Complexity: O(2 \* m) m → Number of unique elements
O(m) → space is used for returning the answer
O(m) → space is used for pre Calculation

```cpp
vector<pair<int, int>> frequencies(vector<int>& arr) {
	int n = arr.size();

	// MAP
	unordered_map<int, int> hash;
	vector<pair<int, int>> ans;

	for (int i = 0; i < n; i++) {
		hash[arr[i]]++;
	}

	for (auto it : hash) {
		ans.push_back({it.first, it.second});
	}

	return ans;
}
```

When working with **characters**, hashing becomes much simpler and more memory-efficient compared to integers.

### Why Character Hashing is Easy

- The total number of possible characters is **limited and small**.
- In standard **ASCII**, there are only **256 possible characters** (values from 0 to 255).
- Since this range is fixed and small, we can safely use a **hash array of size 256**.
- If it is given that the character are only lowercase or uppercase then we can reduce the array size to 26

