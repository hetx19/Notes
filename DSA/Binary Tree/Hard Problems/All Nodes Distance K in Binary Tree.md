**Problem**: Given the `root` of a binary tree, the value of a target node `target`, and an integer `k`, return _an array of the values of all nodes that have a distance_ `k` _from the target node._

You can return the answer in **any order**.

**Example**:
![[All Nodes Distance K in Binary Tree.png|327]]
**Input**: `root = [3,5,1,6,2,0,8,null,null,7,4], target = 5, k = 2`
**Output**: `[7,4,1]`
**Explanation**: The nodes that are a distance 2 from the target node (with value 5) have values 7, 4, and 1.

[Visit Leetcode](https://leetcode.com/problems/all-nodes-distance-k-in-binary-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/nodes-at-given-distance-in-binary-tree/1)

---
### Optimal Solution
By using a hashMap for backward traversal - [[Hashing]]

**Time Complexity**: O(n)
**Space Complexity**: O(n)

```cpp
class Solution {
  private:
	void markParent(TreeNode* root, unordered_map<TreeNode *, TreeNode *> &parentTrack, TreeNode* target) {
		queue<TreeNode *> q;
		q.push(root);
	
		while (!q.empty()) {
			TreeNode *currentNode = q.front();
			q.pop();
		
			if (currentNode->left) {
				parentTrack[currentNode->left] = currentNode;
				q.push(currentNode->left);
			}
		
			if (currentNode->right) {
				parentTrack[currentNode->right] = currentNode;
				q.push(currentNode->right);
			}
		}
	}
	
  public:
	vector<int> distanceK(TreeNode* root, TreeNode* target, int k) {
		unordered_map<TreeNode *, TreeNode *> parentTrack;
		markParent(root, parentTrack, target);
		
		unordered_map<TreeNode *, bool> isVisted;
		queue<TreeNode *> q;
		q.push(target);
		isVisted[target] = true;
		
		int currentLevel = 0;
		
		while (!q.empty()) {
			int size = q.size();
			
			if (currentLevel++ == k) {
				break;
			}
			
			for (int i = 0; i < size; i++) {
				TreeNode* currentNode = q.front();
				q.pop();
				
				if (currentNode->left != nullptr && !isVisted[currentNode->left]) {
					q.push(currentNode->left);
					isVisted[currentNode->left] = true;
				}
				
				if (currentNode->right != nullptr && !isVisted[currentNode->right]) {
					q.push(currentNode->right);
					isVisted[currentNode->right] = true;
				}
				
				if (parentTrack[currentNode] != nullptr && !isVisted[parentTrack[currentNode]]) {
					q.push(parentTrack[currentNode]);
					isVisted[parentTrack[currentNode]] = true;
				}
			}
		}
		
		vector<int> ans;
		while (!q.empty()) {
			TreeNode *currentNode = q.front();
			q.pop();
			ans.push_back(currentNode->val);
		}
		
		return ans;
	}
};
```