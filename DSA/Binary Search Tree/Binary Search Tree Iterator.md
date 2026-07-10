**Problem**: Implement the `BSTIterator` class that represents an iterator over the **[in-order traversal](https://en.wikipedia.org/wiki/Tree_traversal#In-order_\(LNR\))** of a binary search tree (BST):

- `BSTIterator(TreeNode root)` Initializes an object of the `BSTIterator` class. The `root` of the BST is given as part of the constructor. The pointer should be initialized to a non-existent number smaller than any element in the BST.
- `boolean hasNext()` Returns `true` if there exists a number in the traversal to the right of the pointer, otherwise returns `false`.
- `int next()` Moves the pointer to the right, then returns the number at the pointer.

Notice that by initializing the pointer to a non-existent smallest number, the first call to `next()` will return the smallest element in the BST.

You may assume that `next()` calls will always be valid. That is, there will be at least a next number in the in-order traversal when `next()` is called.

**Example**:
![[Binary Search Tree Iterator.png]]
**Input**: `["BSTIterator", "next", "next", "hasNext", "next", "hasNext", "next", "hasNext", "next", "hasNext"]`
`[[[7, 3, 15, null, null, 9, 20]], [], [], [], [], [], [], [], [], []]`
**Output**: `[null, 3, 7, true, 9, true, 15, true, 20, false]`

**Explanation**:
`BSTIterator bSTIterator = new BSTIterator([7, 3, 15, null, null, 9, 20]);`
`BSTIterator.next();    // return 3`
`BSTIterator.next();    // return 7`
`BSTIterator.hasNext(); // return True`
`BSTIterator.next();    // return 9`
`BSTIterator.hasNext(); // return True`
`BSTIterator.next();    // return 15`
`BSTIterator.hasNext(); // return True`
`BSTIterator.next();    // return 20`
`BSTIterator.hasNext(); // return False`

[Visit Leetcode](https://leetcode.com/problems/binary-search-tree-iterator/)

---
### Optimal Solution

**Time Complexity:** Constructor **O(h)**, `next()` **O(1)** amortized, `hasNext()` **O(1)**
**Space Complexity:** **O(h)**

```cpp
class BSTIterator {
  private:
	stack<TreeNode *> st;
	
	void pushAll(TreeNode* node) {
		for (; node != NULL; st.push(node), node = node->left);
	}
	
  public:
	BSTIterator(TreeNode* root) {
		pushAll(root);
	}
	
	int next() {
		TreeNode* topNode = st.top();
		st.pop();
		pushAll(topNode->right);
		return topNode->val;
	}

	bool hasNext() {
		return !st.empty();
	}
};
```

---