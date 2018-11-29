# DFS

Question: Print Longest Path in the tree

```c++
void Tree::printDeepestPath2(TreeNode *root, stack<TreeNode*> &path,
		stack<TreeNode*> &result) {

	if (root == NULL)
		return;
	path.push(root);
	if (root->left == NULL && root->right == NULL) {
		if (path.size() > result.size()) {
			result = path;
		}
	}

	if (root->left)
		printDeepestPath2(root->left, path, result);
	if (root->right)
		printDeepestPath2(root->right, path, result);

	path.pop();
}
```



Question: [Check if it is balanced BST](https://leetcode.com/problems/balanced-binary-tree/description/)

```c++
/*
 *
 * check if it is a valid banlanced BST.
 */

int dfsHeight (TreeNode *root) {
        if (root == NULL) return 0;
        
        int leftHeight = dfsHeight (root -> left);
        if (leftHeight == -1) return -1;
        int rightHeight = dfsHeight (root -> right);
        if (rightHeight == -1) return -1;
        
        if (abs(leftHeight - rightHeight) > 1)  return -1;
        return max (leftHeight, rightHeight) + 1;
    }
bool isBalanced(TreeNode *root) {
        return dfsHeight (root) != -1;
    }


```

Question: Delete the whole BST