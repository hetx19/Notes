**Problem**: Given the `root` of a binary tree, return _the postorder traversal of its nodes' values_.

**Note**: A **postorder traversal** first visits the left child (including its entire subtree), then visits the right child (including its entire subtree), and finally visits the node itself.

**Example**:
**Input**: `root = [1,null,2,3]`
**Output**: `[3,2,1]`

![[Traversal.png]]

[Visit Leetcode](https://leetcode.com/problems/binary-tree-postorder-traversal/)
[Visit GFG](https://www.geeksforgeeks.org/problems/postorder-traversal/1)

---

### By using Recursion - [[Recursion]]

**Time Complexity**: O(n)
**Space Complexity**: O(n) → Recursive Stack Space

```cpp
class Solution {
  private:
	void postOrder(TreeNode *node, vector<int>& ans) {
		if (node == nullptr) {
			return;
		}
		
		postOrder(node->left, ans);
		postOrder(node->right, ans);
		ans.push_back(node->val);
	}
	
  public:
	vector<int> postOrderTraversal(TreeNode *root) {
		vector<int> ans;
		postOrder(root, ans);
		return ans;
	}
};
```

---

### By using a Stack - [[DSA/STL/Stack|Stack]]
By Using 2 Stack

**Time Complexity**: O(n)
**Space Complexity**: O(2n)

```cpp
class Solution {
  public:
	vector<int> postOrderTraversal(TreeNode *root) {
		vector<int> ans;
		
		if (root == nullptr) return ans;
		
		stack<TreeNode *> st1, st2;
		st1.push(root);
		
		while (!st1.empty()) {
			TreeNode *node = st1.top();
			st1.pop();
			st2.push(node); 
			
			if (node->left != nullptr) st1.push(node->left);
			if (node->right != nullptr) st1.push(node->right);
		}
		
		while (!st2.empty()) {
			ans.push_back(st2.top()->val);
			st2.pop();
		}
		
		return ans;
	}
};
```

By using Single Stack

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  public:
	vector<int> postOrderTraversal(TreeNode *root) {
		vector<int> ans;
		
		stack<TreeNode *> st;
		TreeNode *curr = root;
		
		while (curr != nullptr || !st.empty()) {
			if (curr != nullptr) {
				ans.push_back(curr->val);
				st.push(curr);
				curr = curr->right;
			} else {
				curr = st.top();
				st.pop();
				curr = curr->left;
			}
		}
		
		reverse(ans.begin(), ans.end());
		return ans;
	}
};
```

> [!tip]
> We are using O(n) space for returning the answer

---