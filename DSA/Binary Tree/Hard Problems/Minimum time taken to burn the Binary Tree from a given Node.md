**Problem**: Given the **root** of a binary tree and a **target** node, determine the **minimum time** required to burn the entire tree if the **target** node is set on fire. In one second, the fire spreads from a node to its left child, right child, and parent.

**Note:** The tree contains unique values.

**Example**:
**Input**: `root = [1, 2, 3, 4, 5, 6, 7], target = 2`
![[Minimum time taken to burn the Binary Tree from a given Node.png|381]]

[Visit GFG](https://www.geeksforgeeks.org/problems/burning-tree/1)

---
### Optimal Solution

**Time Complexity**: O(n) + O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	int findMaxDistance(unordered_map<TreeNode *, TreeNode *>& mapParent, TreeNode *target) {
		queue<TreeNode *> q;
		q.push(target);
		map<TreeNode *, bool> visited;
		visited[target] = true;
		
		int maxi = 0;
		
		while (!q.empty()) {
			int size = q.size();
			bool flag = false;
			
			for (int i = 0; i < size; i++) {
				TreeNode *node = q.front();
				q.pop();
				
				if (node->left != nullptr && !visited[node->left]) {
					flag = true;
					visited[node->left] = true;
					q.push(node->left);
				}
				
				if (node->right != nullptr && !visited[node->right]) {
					flag = true;
					visited[node->right] = true;
					q.push(node->right);
				}
				
				if (mapParent[node] != nullptr && !visited[mapParent[node]]) {
					flag = true;
					visited[mapParent[node]] = true;
					q.push(mapParent[node]);
				}
			}
			
			maxi += (flag) ? 1 : 0;
		}
		
		return maxi;
	}
	
	TreeNode *bfsToMapParents(TreeNode *root, unordered_map<TreeNode *, TreeNode *>& mapParent, int start) {
		queue<TreeNode *> q;
		q.push(root);
		TreeNode *ans;
		
		while (!q.empty()) {
			TreeNode *node = q.front();
			q.pop();
			
			if (node->val == start) {
				ans = node;
			}
			
			if (node->left != nullptr) {
				mapParent[node->left] = node;
				q.push(node->left);
			}
			
			if (node->right != nullptr) {
				mapParent[node->right] = node;
				q.push(node->right);
			}
		}
		
		return ans;
	}
	
  public:
	int minTime(TreeNode* root, int target) {
		unordered_map<TreeNode *, TreeNode *> mapParent;
		
		TreeNode *targetNode = bfsToMapParents(root, mapParent, target);
		int maxi = findMaxDistance(mapParent, targetNode);
		
		return maxi;
    }
};
```

---