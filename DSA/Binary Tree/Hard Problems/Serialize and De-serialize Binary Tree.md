**Problem**: Serialization is the process of converting a data structure or object into a sequence of bits so that it can be stored in a file or memory buffer, or transmitted across a network connection link to be reconstructed later in the same or another computer environment.

Design an algorithm to serialize and deserialize a binary tree. There is no restriction on how your serialization/deserialization algorithm should work. You just need to ensure that a binary tree can be serialized to a string and this string can be deserialized to the original tree structure.

**Clarification:** The input/output format is the same as [how LeetCode serializes a binary tree](https://support.leetcode.com/hc/en-us/articles/32442719377939-How-to-create-test-cases-on-LeetCode#h_01J5EGREAW3NAEJ14XC07GRW1A). You do not necessarily need to follow this format, so please be creative and come up with different approaches yourself.

**Example**:
![[Serialize and De-serialize Binary Tree.png|269]]
**Input**: `root = [1,2,3,null,null,4,5]`
**Output**: `[1,2,3,null,null,4,5]`

[Visit Leetcode](https://leetcode.com/problems/serialize-and-deserialize-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/serialize-and-deserialize-a-binary-tree/1)

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Codec {
  public:
	// Encodes a tree to a single string.
	string serialize(TreeNode* root) {
		if (root == nullptr) {
			  return "";
		}
	
		string s;
		queue<TreeNode *> q;
		q.push(root);
	
		while (!q.empty()) {
			TreeNode *node = q.front();
			q.pop();
		
			if (node == nullptr) {
				s.append("nullptr,");
			} else {
				s.append(to_string(node->val) + ',');
			}
		
			if (node != nullptr) {
				q.push(node->left);
				q.push(node->right);
			}
		}
	
		return s;
	}
  
	// Decodes your encoded data to tree.
	TreeNode* deserialize(string data) {
		if (data.size() == 0) {
			return nullptr;
		}
		
		stringstream s(data);
		string str;
		getline(s, str, ',');
		
		TreeNode *root = new TreeNode(stoi(str));
		queue<TreeNode *> q;
		q.push(root);
		
		while (!q.empty()) {
			TreeNode *node = q.front(); q.pop(); getline(s, str, ',');
			
			if (str == "nullptr") {
				node->left = nullptr;
			} else {
				TreeNode *leftNode = new TreeNode(stoi(str));
				node->left = leftNode;
				q.push(leftNode);
			}
			
			getline(s, str, ',');
			
			if (str == "nullptr") {
				node->right = nullptr;
			} else {
				TreeNode *rightNode = new TreeNode(stoi(str));
				node->right = rightNode;
				q.push(rightNode);
			}
		}
		
		return root;
	}
};
```

---