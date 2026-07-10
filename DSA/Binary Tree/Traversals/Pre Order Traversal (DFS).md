**Problem**: Given the `root` of a binary tree, return _the preorder traversal of its nodes' values_.

**Note**: A **preorder traversal** first visits the node, then visits the left child (including its entire subtree), and finally visits the right child (including its entire subtree).

**Example**:
**Input**: `root = [1,null,2,3]`
**Output**: `[1,2,3]`

![[Traversal.png]]

[Visit Leetcode](https://leetcode.com/problems/binary-tree-preorder-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/preorder-traversal/1)

---

### By using Recursion - [[Recursion]]

**Time Complexity**: O(n)
**Space Complexity**: O(n) → Recursive Stack Space

```cpp
class Solution {
  private:
	void preOrder(TreeNode *node, vector<int>& ans) {
		if (node == nullptr) {
			return;
		}
		
		ans.push_back(node->val);
		preOrder(node->left, ans);
		preOrder(node->right, ans);
	}
	
  public:
	vector<int> preOrderTraversal(TreeNode *root) {
		vector<int> ans;
		preOrder(root, ans);
		return ans;
	}
};
```

---

### By using a Stack - [[DSA/STL/Stack|Stack]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<int> preOrderTraversal(TreeNode *root) {
		vector<int> ans;
	
		if (root == nullptr) {
			return ans;
		}
		
		stack<TreeNode *> st;
		st.push(root);
		
		while (!st.empty()) {
			TreeNode *node = st.top();
			st.pop();
			
			ans.push_back(node->val);
			
			if (node->right != nullptr) st.push(node->right);
			if (node->left != nullptr) st.push(node->left);
		}
		
		return ans;
	}
};
```

> [!tip]
> We are using O(n) space for returning the answer

---
### By Using Moris Traversal

**Time Complexity**: O(2n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	vector<int> preorderTraversal(TreeNode* root) {
		vector<int> preOrder;
		TreeNode* current = root;
		
		while (current != nullptr) {
			if (current->left == nullptr) {
				preOrder.push_back(current->val);
				current = current->right;
			} else {
				TreeNode* previous = current->left;
				
				while (previous->right != nullptr && previous->right != current) {
					previous = previous->right;
				}
				
				if (previous->right == nullptr) {
					preOrder.push_back(current->val);
					previous->right = current;
					current = current->left;
				} else {
					previous->right = nullptr;
					current = current->right;
				}
			}
		}
		return preOrder;
	}
};
```

---