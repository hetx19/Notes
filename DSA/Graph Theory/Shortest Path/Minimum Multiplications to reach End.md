**Problem**: Given **start**, **end** and an array **arr** of **n** numbers. At each step, **start** is multiplied with any number in the array and then mod operation with **100000** is done to get the new start.

Your task is to find the minimum steps in which **end** can be achieved starting from **start**. If it is not possible to reach **end**, then return **-1**.

[Visit GFG](https://www.geeksforgeeks.org/problems/minimum-multiplications-to-reach-end/1)

**Example**:

**Input:**
arr[] = {2, 5, 7}
start = 3, end = 30
**Output:**
2
**Explanation:**
```
Step 1: 3*2 = 6 % 100000 = 6 
Step 2: 6*5 = 30 % 100000 = 30
```

---

### By Dijkstra's Algorithm

**Time Complexity**: O(100000 * n) → This is a valid **tight upper bound**. (Highly dependent on Input Array)
**Space Complexity**: O(100000)

```cpp
class Solution {
  public:
	int minimumMultiplications(vector<int> &arr, int start, int end) {
		const int mod = 100000;
        vector<int> distance(100000, INT_MAX);
        distance[start] = 0;
        queue<pair<int, int>> q;
        q.push({0, start});
        
        while (!q.empty()) {
	        int node = q.front().second;
	        int steps = q.front().first;
	        q.pop();
	        
	        if (node == end) {
		        return steps;
	        }
	        
	        for (int it : arr) {
		        int num = (it * node) % mod;
		        
		        if (steps + 1 < distance[num]) {
			        distance[num] = steps + 1;
			        q.push({distance[num], num});
		        }
	        }
        }
        
        return -1;
    }
};
```