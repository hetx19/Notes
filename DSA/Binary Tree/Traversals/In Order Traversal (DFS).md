**Problem**: Given the `root` of a binary tree, return _the inorder traversal of its nodes' values_.

**Note:** An **inorder traversal** first visits the left child (including its entire subtree), then visits the node, and finally visits the right child (including its entire subtree).

**Example**:
**Input**: `root = [1,null,2,3]`
**Output**: `[1,3, 2]`

![[Traversal.png]]

[Visit Leetcode](https://leetcode.com/problems/binary-tree-inorder-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/inorder-traversal/1)

---

### By using Recursion - [[Recursion]]

**Time Complexity**: O(n)
**Space Complexity**: O(n) → Recursive Stack Space

```cpp
class Solution {
  private:
	void inOrder(TreeNode *node, vector<int>& ans) {
		if (node == nullptr) {
			return;
		}
		
		inOrder(node->left, ans);
		ans.push_back(node->val);
		inOrder(node->right, ans);
	}
	
  public:
	vector<int> inOrderTraversal(TreeNode *root) {
		vector<int> ans;
		inOrder(root, ans);
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
	vector<int> inOrderTraversal(TreeNode *root) {
		vector<int> ans;
		
		stack<TreeNode *> st;
		TreeNode *node = root;
		
		while (true) {
			if (node != nullptr) {
				st.push(node);
				node = node->left;
			} else {
				if (st.empty()) {
					break;
				} else {
					node = st.top();
					st.pop();
					
					ans.push_back(node->val);
					node = node->right;
				}
			}
		}
		
		return ans;
	}
};
```

> [!tip]
> We are using O(n) space for returning the answer

---
### By using Moris Traversal

**Time Complexity**: O(2n)
**Space Complexity**: O(1)

```cpp
class Solution {
  public:
	vector<int> inorderTraversal(TreeNode* root) {
		vector<int> ans;
		TreeNode* current = root;
		
		while (current != nullptr) {
			if (current->left == nullptr) {
				ans.push_back(current->val);
				current = current->right;
			} else {
				TreeNode* perv = current->left;
				while (perv->right != nullptr && perv->right != current) {
					perv = perv->right;
				}
				
				if (perv->right == nullptr) {
					perv->right = current;
					current = current->left;
				} else {
					perv->right = nullptr;
					ans.push_back(current->val);
					current = current->right;
				}
			}
		}
		return ans;
	}
};
```

---