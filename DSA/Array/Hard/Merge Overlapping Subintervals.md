**Problem**: Given an array of `intervals` where `intervals[i] = [starti, endi]`, merge all overlapping intervals, and return _an array of the non-overlapping intervals that cover all the intervals in the input_.

**Example**:

**Input**: `intervals = [[1,3],[2,6],[8,10],[15,18]]`
**Output**: `[[1,6],[8,10],[15,18]]`
**Explanation**: `Since intervals [1,3] and [2,6] overlap, merge them into [1,6].`

[Visit Leetcode](https://leetcode.com/problems/merge-intervals/)
[Visit GFG](https://www.geeksforgeeks.org/problems/overlapping-intervals--170633/1)

---

### Brute force 
By doing Sorting- [[Sorting]]

**Time Complexity**: O(n logn) + O(2n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<vector<int>> merge(vector<vector<int>>& intervals) {
		int n = intervals.size();
		sort(intervals.begin(), intervals.end());
		vector<vector<int>> ans;
		
		for (int i = 0; i < n; i++) {
			int start = intervals[i][0];
			int end = intervals[i][1];
			
			if (!ans.empty() && end <= ans.back()[1]) {
				continue;
			}
			
			for (int j = i + 1; j < n; j++) {
				if (intervals[j][0] <= end) {
					end = max(intervals[j][1], end);
				} else {
					break;
				}
			}
			
			ans.push_back({start, end});
		}
		
		return ans;
	}
};
```

---

### Optimal Solution

**Time Complexity**: O(n logn + n)
 **Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<vector<int>> merge(vector<vector<int>>& intervals) {
		sort(intervals.begin(), intervals.end());
		vector<vector<int>> ans;
		
		for (auto &interval : intervals) {
			if (ans.empty() || interval[0] > ans.back()[1]) {
				ans.push_back(interval);
			} else {
				ans.back()[1] = max(interval[1], ans.back()[1]);
			}
		}
		
		return ans;
	}
};
```

---
