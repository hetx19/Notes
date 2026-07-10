**Problem**: Given the roots of two binary trees `p` and `q`, write a function to check if they are the same or not.

Two binary trees are considered the same if they are structurally identical, and the nodes have the same value.

**Example**:
![[Check if two trees are identical or not.png]]
**Input**: `p = [1,2,3], q = [1,2,3]`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/same-tree/)
[Visit GFG](https://www.geeksforgeeks.org/problems/determine-if-two-trees-are-identical/1)

---
### Optimal Solution
By Checking each and every node

**Time Complexity**: O(n)
**Space Complexity:**: O(h)

```cpp
class Solution {
  public:
	bool isSameTree(TreeNode *p, TreeNode *q) {
		if (p == nullptr && q == nullptr) {
			return true;
		}
		
		if (p == nullptr || q == nullptr) {
			return false;
		}
		
		return (p->val == q->val && isSameTree(p->left, q-> left) && isSameTree(p->right, q->right));
	}
};
```

---