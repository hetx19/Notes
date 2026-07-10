**Problem**: Given the `root` of a binary tree, calculate the **vertical order traversal** of the binary tree.

For each node at position `(row, col)`, its left and right children will be at positions `(row + 1, col - 1)` and `(row + 1, col + 1)` respectively. The root of the tree is at `(0, 0)`.

The **vertical order traversal** of a binary tree is a list of top-to-bottom orderings for each column index starting from the leftmost column and ending on the rightmost column. There may be multiple nodes in the same row and same column. In such a case, sort these nodes by their values.

Return _the **vertical order traversal** of the binary tree_.

**Example**:
![[Vertical Order Traversal.png|311]]

**Input**: `root = [3,9,20,null,null,15,7]`
**Output**: `[[9],[3,15],[20],[7]]`
**Explanation**:
Column -1: Only node 9 is in this column.
Column 0: Nodes 3 and 15 are in this column in that order from top to bottom.
Column 1: Only node 20 is in this column.
Column 2: Only node 7 is in this column.

[Visit Leetcode](https://leetcode.com/problems/vertical-order-traversal-of-a-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/print-a-binary-tree-in-vertical-order/1)

---
### Optimal Solution
By using co-ordinate

**Time Complexity**: O(n (log n)<sup>3</sup>)
**Space Complexity**: O(n + n / 2)

```cpp
class Solution {
  public:
	vector<vector<int>> verticalTraversal(TreeNode* root) {
		map<int, map<int, multiset<int>>> nodes;
		queue<pair<TreeNode* , pair<int, int>>> todo;
		todo.push({root, {0, 0}});
		
		while(!todo.empty()) {
			auto p = todo.front();
			todo.pop();
			
			TreeNode* temp = p.first;
			int x = p.second.first;
			int y = p.second.second;
			
			nodes[x][y].insert(temp->val);
			
			if(temp->left != nullptr) {
				todo.push({temp->left, {x - 1, y + 1}});
			}
			
			if(temp->right != nullptr) {
				todo.push({temp->right, {x + 1, y + 1}});
			}
		}
		
		vector<vector<int>> ans;
		
		for (auto p: nodes) {
			vector<int> col;
			for (auto q: p.second) {
				col.insert(col.end(), q.second.begin(), q.second.end());
			}
			ans.push_back(col);
		}
		return ans;
	}
};
```

---