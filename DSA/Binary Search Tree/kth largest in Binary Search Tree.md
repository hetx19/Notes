**Problem**: Given the `root` of a binary search tree, and an integer `k`, return _the_ `kth` _largest value (**1-indexed**) of all the values of the nodes in the tree_.

**Example**:
![[kth smallest in Binary Search Tree.png|195]]
**Input**: `root = [3,1,4,null,2], k = 1`
**Output**: 4

[Visit GFG](https://www.geeksforgeeks.org/problems/kth-largest-element-in-bst/1)

---
### Brute force Solution
By using recursion - [[Recursion]]

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  private:
	int kthLargestElement(TreeNode* root, int &count, int k) {
	    if (root == nullptr) {
	        return -1;
	    }
        
	    int right = kthLargestElement(root->right, count, k);
	    
	    if (right != -1) {
	        return right;
	    }
	    
	    count++;
    
	    if (count == k) {
	        return root->data;
	    }
        
	    int left = kthLargestElement(root->left, count, k);
	    
	    return left;
	}
	
  public:
	int kthLargest(TreeNode* root, int k) {
	    int count = 0;
	    return kthLargestElement(root, count, k);
	}
};
```

---
### Alternative Solution

**Time Complexity**: O(n)
**Space Complexity**: O(h)

```cpp
class Solution {
  public:
	int kthLargest(TreeNode* root, int k) {
	    stack<TreeNode*> st;
	    TreeNode* current = root;
	    int count = 0;
    
	    while (current != nullptr || !st.empty()) {
	        while (current != nullptr) {
	            st.push(current);
	            current = current->right;
	        }
        
	        current = st.top();
	        st.pop();
        
	        count++;
        
	        if (count == k) {
	            return current->data;
	        }
        
	        current = current->left;
	    }
    
	    return -1;
	}
};
```

---