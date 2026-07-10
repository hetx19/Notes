**Problem**: Given a binary tree, the task is to print all the nodes of the binary tree in *Pre-order*, *in-order* and *post-order*, iteratively using only one stack traversal.

**Example**:
**Input**:
![[All DFS Traversal.png|175]]

**Output**:
Preorder Traversal: 1 2 3  
Inorder Traversal: 2 1 3  
Postorder Traversal: 2 3 1

---

### By using Recursion - [[Recursion]]

**Time Complexity**: O(3n) → O(n)
**Space Complexity**: O(3n) + O(n) → O(n)

```cpp
class Solution {
  private:
	void traversal(TreeNode *node, vector<int> &preOrder, vector<int> &inOrder, vector<int> &postOrder) {
		if (root == nullptr) {
			return;
		}
		
		preOrder.push_back(node->val);
		traversal(node->left, preOrder, inOrder, postOrder);
		inOrder.push_back(node->val);
		traversal(node->right, preOrder, inOrder, postOrder);
		postOrder.push_back(node->val);
	}
	
  public:
	vector<vector<int>> allTraversal(TreeNode *root) {
		vector<int> preOrder, inOrder, postOrder;
		traversal(root, preOrder, inOrder, postOrder);
		return {preOrder, inOrder, postOrder};
	}
};
```

> [!tip]
> We are using O(3n) space for returning the ans

---
### By using a Stack - [[DSA/STL/Stack|Stack]]

**Time Complexity**: O(3n) → O(n)
**Space Complexity**: O(3n) + O(n) → O(n)

```cpp
class Solution {
  public:
	vector<vector<int>> allTraversal(TreeNode *root) {
		vector<int> preOrder, inOrder, postOrder;
		
		if (root == nullptr) {
			return {preOrder, inOrder, postOrder};
		}
		
		stack<pair<TreeNode *, int>> st;
		st.push({root, 1});
		
		while (!st.empty()) {
			auto it = st.top();
			st.pop();
			
			if (it.second == 1) {
				preOrder.push_back(it.first->val);
				it.second++;
				st.push(it);
				
				if (it.first->left != nullptr) {
					st.push_back({it.first->left, 1});
				}
			} else if (it.second == 2) {
				inOrder.push_back(it.first->val);
				it.second++;
				st.push(it);
				
				if (it.first->right != nullptr) {
					st.push_back({it.first->right, 1});
				}
			} else {
				postOrder.push_back(it.first->val);
			}
		}
		
		return {preOrder, inOrder, postOrder};
	}
};
```

> [!tip]
> We are using O(3n) space for returning the ans

---