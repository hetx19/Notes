**Problem**: Given the `root` of a binary tree, flatten the tree into a "linked list":

- The "linked list" should use the same `TreeNode` class where the `right` child pointer points to the next node in the list and the `left` child pointer is always `null`.
- The "linked list" should be in the same order as a [**pre-order** **traversal**](https://en.wikipedia.org/wiki/Tree_traversal#Pre-order,_NLR) of the binary tree.

**Example**:
![[Flatten Binary Tree to Linked List.png|347]]
**Input**: `root = [1,2,5,3,4,null,6]`
**Output**: `[1,null,2,null,3,null,4,null,5,null,6]`

[Visit Leetcode](https://leetcode.com/problems/flatten-binary-tree-to-linked-list/)
[Visit GFG](https://www.geeksforgeeks.org/problems/flatten-binary-tree-to-linked-list/1)

---
### Brute Force Solution
By using Recursion - [[Recursion]]

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution { 
  private:
	TreeNode* prev = nullptr;
	
  public:
	void flatten(TreeNode* root) {
		if (root == nullptr) {
			return;
		}
		
		flatten(root->right);
		flatten(root->left);
		root->right = prev;
		root->left = nullptr;
		prev = root;
	}
};
```

---
### Better Approach
By using Stack - [[DSA/STL/Stack|Stack]]

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution { 	
  public:
	void flatten(TreeNode* root) {
		if (root == nullptr) {
			return;
		}
		
		stack<TreeNode *> st;
		st.push(root);
		
		while (!st.empty()) {
			TreeNode *current = st.top();
			st.pop();
			
			if (current->right != nullptr) {
				st.push(current->right);
			}
			
			if (current->left != nullptr) {
				st.push(current->left);
			}
			
			if (!st.empty()) {
				current->right = st.top();
			}
			
			current->left = nullptr;
		}
	}
};
```

---
### Optimal Solution

**Time Complexity**: O(2n)
**Space Complexity**: O(1)

```cpp
class Solution { 
  public:
	void flatten(TreeNode* root) {
		TreeNode *current = root;
		
		while (current != nullptr) {
			if (current->left != nullptr) {
				TreeNode *prev = current->left;
				
				while (prev->right != nullptr) {
					prev = prev->right;
				}
				
				prev->right = current->right;
				current->right = current->left;
				current->left = nullptr;
			}
			
			current = current->right;
		}
	}
};
```

---
