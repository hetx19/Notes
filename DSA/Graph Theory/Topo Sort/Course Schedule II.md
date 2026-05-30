**Problem**: There are a total of `numCourses` courses you have to take, labeled from `0` to `numCourses - 1`. You are given an array `prerequisites` where `prerequisites[i] = [ai, bi]` indicates that you **must** take course `bi` first if you want to take course `ai`.

- For example, the pair `[0, 1]`, indicates that to take course `0` you have to first take course `1`.
  Return *the ordering of courses you should take to finish all courses*. If there are many valid answers, return **any** of them. If it is impossible to finish all courses, return **an empty array**.

**Example**:
**Input:** numCourses = 2, prerequisites = `[[1,0]]`
**Output:** `[0,1]`
**Explanation:** There are a total of 2 courses to take. To take course 1 you should have finished course 0. So the correct course order is `[0,1]`.

[Vist LeetCode](https://leetcode.com/problems/course-schedule-ii/description/)
[Vist_GFG](https://www.geeksforgeeks.org/problems/prerequisite-tasks/1)

---

### By Topological Sort

**Time Complexity:** O(V + E)
**Space Complexity:** O(V) + O(V) + O(V) → O(V)
V → numCourses

```cpp
class Solution {
  public:
	vector<int> canFinish(int numCourses, vector<vector<int>>& prerequisites) {
		vector<vector<int>> adj(numCourses);

		for (auto &pre : prerequisites) {
			adj[pre[1]].push_back(pre[0]);
		}

		vector<int> indegree(numCourses, 0);

		for (int i = 0; i < numCourses; i++) {
			for (int it : adj[i]) {
				indegree[it]++;
			}
		}

		queue<int> q;
		for (int i = 0; i < numCourses; i++) {
			if (indegree[i] == 0) {
				q.push(i);
			}
		}

		vector<int> ans;

		while (!q.empty()) {
			int node = q.front();
			q.pop();

			ans.push_back(node);

			for (int it : adj[node]) {
				indegree[it]--;
				if (indegree[it] == 0) {
					q.push(it);
				}
			}
		}

		if (ans.size() == numCourses) {
			return ans;
		}

		return {};
	}
};
```
