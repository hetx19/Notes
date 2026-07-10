**Problem**: Given the `root` of a binary tree, return _the **maximum width** of the given tree_.

The **maximum width** of a tree is the maximum **width** among all levels.

The **width** of one level is defined as the length between the end-nodes (the leftmost and rightmost non-null nodes), where the null nodes between the end-nodes that would be present in a complete binary tree extending down to that level are also counted into the length calculation.

It is **guaranteed** that the answer will in the range of a **32-bit** signed integer.

**Example**:
![[Maximum width of Binary Tree.png|265]]
**Input**: `root = [1,3,2,5,3,null,9]`
**Output**: 4
**Explanation**: The maximum width exists in the third level with length 4 (5,3,null,9).

[Visit Leetcode](https://leetcode.com/problems/maximum-width-of-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/maximum-width-of-tree/1)

---
### Optimal Solution
By using a BFS traversal

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	int widthOfBinaryTree(TreeNode* root) {
		if (root == nullptr) {
			return 0;
		}
		
		long long maxWidth = 0;
		queue<pair<TreeNode*, long long>> q;
		q.push({root, 1});
		
		while (!q.empty()) {
			int size = q.size();
			long long left = q.front().second;
			long long first = 0, last = 0;
			
			for (int i = 0; i < size; i++) {
				auto [node, idx] = q.front();
				q.pop();
				
				long long current = idx - left;
				
				if (i == 0) {
					first = current;
				} else if (i == size - 1) {
					last = current;
				}
				
				if (node->left != nullptr) {
					q.push({node->left, 2 * current + 1});
				}
				
				if (node->right != nullptr) {
					q.push({node->right, 2 * (current + 1)});
				}
			}
			
			maxWidth = max(maxWidth, last - first + 1);
		}
	
		return maxWidth;
	}
};
```

---