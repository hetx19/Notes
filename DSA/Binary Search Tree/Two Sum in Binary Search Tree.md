**Problem**: Given the `root` of a binary search tree and an integer `k`, return `true` _if there exist two elements in the BST such that their sum is equal to_ `k`, _or_ `false` _otherwise_.

**Example**:
![[Two Sum in Binary Search Tree.png|302]]
**Input**: `root = [5,3,6,2,4,null,7], k = 9`
**Output**: true

[Visit Leetcode](https://leetcode.com/problems/two-sum-iv-input-is-a-bst/)
[Visit GFG](https://www.geeksforgeeks.org/problems/find-a-pair-with-given-target-in-bst/1)

---
### Brute Force Solution
By storing inorder traversal + 2 pointer - [[Two Sum]]

**Time Complexity**: O(2n)
**Space Complexity**: O(n) + Recursive Stack Space

```cpp
class Solution {
  private:
	bool twoSum(vector<int>& nums, int target) {
		int left = 0, right = nums.size() - 1;
		
		while (left < right) {
			int sum = nums[left] + nums[right];
			
			if (sum == target) {
				return true;
			} else if (sum < target) {
				left++;
			} else {
				right--;
			}
		}
		
		return false;
	}
	
	void inorderTraversal(TreeNode* node, vector<int>& inorder) {
		if (node == nullptr) {
			return;
		}
		
		inorderTraversal(node->left, inorder);
		inorder.push_back(node->val);
		inorderTraversal(node->right, inorder);
	}
	
  public:
	bool findTarget(TreeNode* root, int k) {
		vector<int> inorder;
		inorderTraversal(root, inorder);
		
		return twoSum(inorder, k);
	}
};
```

---
### Optimal Solution
By using BST Iterator - [[Binary Search Tree Iterator]]

**Time Complexity**: O(n)
**Space Complexity**: O(2h)

```cpp
class BSTIterator {
  private:
	stack<TreeNode *> st;
	bool reverse = true;
	
	void pushAll(TreeNode* node) {
		for (; node != NULL;) {
			st.push(node);
		
			if (reverse == true) {
				node = node->right;
			} else {
				node = node->left;
			}
		}
	}
	
  public:
	BSTIterator(TreeNode* root, bool isReverse) {
		reverse = isReverse;
		pushAll(root);
	}

	int next() {
		TreeNode* topNode = st.top();
		st.pop();

		if (reverse == false) {
			pushAll(topNode->right);
		} else {
			pushAll(topNode->left);
		}

		return topNode->val;
	}

	bool hasNext() {
		return !st.empty();
	}
};

class Solution {
  public:
	bool findTarget(TreeNode* root, int k) {
		if (root == nullptr) {
			return false;
		}

		BSTIterator left(root, false);
		BSTIterator right(root, true);
		
		int i = left.next(), j = right.next();
		
		while (i < j) {
			if (i + j == k) {
				return true;
			} else if (i + j > k) {
				j = right.next();
			} else {
				i = left.next();
			}
		}
		
		return false;
	}
};
```

---