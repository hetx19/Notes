**Problem**: Given a **root** of a Binary Tree, return its **boundary traversal** in the following order:

1. **Left Boundary:** Nodes from the root to the leftmost non-leaf node, preferring the left child over the right and excluding leaves.
2. **Leaf Nodes:** All leaf nodes from left to right, covering every leaf in the tree.
3. **Reverse Right Boundary:** Nodes from the root to the rightmost non-leaf node, preferring the right child over the left, excluding leaves, and added in reverse order.

**Note:** The root is included once, leaves are added separately to avoid repetition, and the right boundary follows traversal preference not the path from the rightmost leaf.

**Example**:
**Input**: `root = [1, 2, 3, 4, 5, 6, 7, N, N, 8, 9, N, N, N, N]`
**Output**: `[1, 2, 4, 8, 9, 6, 7, 3]`
**Explanation:**
![[Boundary Traversal.png|294]]

[Visit GFG](https://www.geeksforgeeks.org/problems/boundary-traversal-of-binary-tree/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(n) + O(h)

```cpp
class Solution {
  private:
	bool isLeafNode(TreeNode *node) {
		return (!node->left && !node->right);
	}
	
	void addLeftBoundary(TreeNode *node, vector<int>& ans) {
		TreeNode *current = node->left;
		
		while (current != nullptr) {
			if (!isLeafNode(current)) {
				ans.push_back(current->data);
			}
			
			if (current->left != nullptr) {
				current = current->left;
			} else {
				current = current->right;
			}
		}
	}
	
	void addRightBoundary(TreeNode *node, vector<int>& ans) {
		TreeNode *current = node->right;
		vector<int> temp;
		
		while (current != nullptr) {
			if (!isLeafNode(current)) {
				temp.push_back(current->data);
			}
			
			if (current->right != nullptr) {
				current = current->right;
			} else {
				current = current->left;
			}
		}
		
		for (int i = temp.size() - 1; i >= 0; i--) {
			ans.push_back(temp[i]);
		}
	}
	
	void addLeaves(TreeNode *node, vector<int>& ans) {
		if (isLeafNode(node)) {
			ans.push_back(node->val);
			return;
		}
		
		if (node->left != nullptr) {
			addLeaves(node->left, ans);
		}
		
		if (node->right != nullptr) {
			addLeaves(node->right, ans);
		}
	}
	
  public:
	vector<int> boundaryTraversal(TreeNode *root) {   
		vector<int> ans;
		
		if (root == nullptr) {
			return ans;
		}
		
		if (!isLeafNode(root)) {
			ans.push_back(root->val);
		}
		
		addLeftBoundary(root, ans);
		addLeaves(root, ans);
		addRightBoundary(root, ans);
		
		return ans;
    }
};
```

---