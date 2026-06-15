**Problem**: You are given an array **`arr`** of positive integers. Your task is to find all the leaders in the array. An element is considered a leader if it is greater than or equal to all elements to its right. The rightmost element is always a leader.

**Example**:
**Input**: `arr = [16, 17, 4, 3, 5, 2]`
**Output**: `[17, 5, 2]`
**Explanation**: Note that there is nothing greater on the right side of 17, 5 and, 2.

[Visit GFG](https://www.geeksforgeeks.org/problems/leaders-in-an-array-1587115620/1)

---

### Brute Force Approach

**Time Complexity**: O(n<sup>2</sup>)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
    vector<int> leaders(vector<int>& arr) {
	    vector<int> ans;
	    int n = arr.size();
	    
	    for (int i = 0; i < n; i++) {
		    bool isLeader = true;
		    
		    for (int j = i + 1; j < n; j++) {
			    if (arr[j] > arr[i]) {
				    isLeader = false;
				    break;
			    }
		    }
		    
		    if (isLeader) {
			    ans.push_back(arr[i]);
		    }
	    }
	    
	    return ans;
    }
};
```

---

### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
public:
    vector<int> leaders(vector<int>& arr) {
        int n = arr.size(), maxi = INT_MIN;
        vector<int> ans;
        
        for (int i = n - 1; i >= 0; i--) {
            if (arr[i] >= maxi) {
                ans.push_back(arr[i]);
                maxi = arr[i];
            }
        }
        
        reverse(ans.begin(), ans.end());
        return ans;
    }
};
```

---