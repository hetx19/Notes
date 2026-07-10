**Problem**: A **path** in a binary tree is a sequence of nodes where each pair of adjacent nodes in the sequence has an edge connecting them. A node can only appear in the sequence **at most once**. Note that the path does not need to pass through the root.

The **path sum** of a path is the sum of the node's values in the path.

Given the `root` of a binary tree, return _the maximum **path sum** of any **non-empty** path_.

**Example**:
![[Maximum path sum.png|305]]
**Input**: `root = [1,2,3]`
**Output**: 6
**Explanation**: The optimal path is 2 -> 1 -> 3 with a path sum of 2 + 1 + 3 = 6.

[Visit Leetcode](https://leetcode.com/problems/binary-tree-maximum-path-sum/)
[Visit GFG](https://www.geeksforgeeks.org/problems/maximum-path-sum-from-any-node/1)

---
### Optimal Solution
By Simple DFS Traversal

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	int dfs(TreeNode *node, int &maxSum) {
		if (node == nullptr) {
			return 0;
		}
		
		int left = max(0, dfs(node->left, maxSum));
		int right = max(0, dfs(node->right, maxSum));
		
		maxSum = max(maxSum, left + right);
		
		return node->val + max(left, right);
	}
	
  public:
	int maxPathSum(TreeNode *root) {
		int maxSum = INT_MIN;
		dfs(root, maxSum);
		return maxSum;
	}
};
```

---