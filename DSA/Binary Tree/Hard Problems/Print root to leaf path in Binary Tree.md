**Problem**: Given a binary tree with distinct nodes(no two nodes have the same data values). The problem is to print the path from root to a given node ****x****. If node ****x**** is not present then print "No Path".

**Example**: 
**Input**:
```
      1  
    /   \  
   2     3  
  /  \   / \  
 4    5  6  7
 target = 5  
```

 **Output**: `[1,2,5]

---
### Optimal Solution

**Time Complexity**: O(n)
**Space Complexity**: O(h) + Recursive Stack Space

```cpp
class Solution {
  private:
	bool getPath(TreeNode *node, vector<int>& ans, int target) {
		if (node == nullptr) {
			return false;
		}
		
		ans.push_back(node->val);
		
		if (node->val == target) {
			return true;
		}
		
		if (getPath(node->left, ans, target) || getPath(node->right, ans, target)) {
			return true;
		}
		
		ans.pop_back();
		return false;
	}
	
  public:
	vector<int> printPath(TreeNode *root, int target) {
		vector<int> ans;
		
		if (root == nullptr) {
			return;
		}
		
		getPath(root, ans, target);
		return ans;
	}
};
```

---