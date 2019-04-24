# Binary Tree Related Questions



## a list of questions on leetcode
https://articles.leetcode.com/category/binary-tree/

**Question: Saving a Binary Search Tree to a File**

se preorder traversal

read file into binary search tree

```c++
void readBSTHelper(int min, int max, int &insertVal,
                   BinaryTree *&p, ifstream &fin) {
  if (insertVal > min && insertVal < max) {
    int val = insertVal;
    p = new BinaryTree(val);
    if (fin >> insertVal) {
      readBSTHelper(min, val, insertVal, p->left, fin);
      readBSTHelper(val, max, insertVal, p->right, fin);
    }
  }
}

void readBST(BinaryTree *&root, ifstream &fin) {
  int val;
  fin >> val;
  readBSTHelper(INT_MIN, INT_MAX, val, root, fin);
}

```

**Question: Saving a Binary Tree to a file**

```c++
void writeBinaryTree(BinaryTree *p, ostream &out) {
  if (!p) {
    out << "# ";
  } else {
    out << p->data << " ";
    writeBinaryTree(p->left, out);
    writeBinaryTree(p->right, out);
  }
}
```

**Read from file to Binary tree**

```c++
void readBinaryTree(BinaryTree *&p, ifstream &fin) {
  int token;
  bool isNumber;
  if (!readNextToken(token, fin, isNumber)) 
    return;
  if (isNumber) {
    p = new BinaryTree(token);
    readBinaryTree(p->left, fin);
    readBinaryTree(p->right, fin);
  }
}
```

**Question: Serialization/Deserialization of a Binary Tree**
use Preorder Travel with sentinel. 

```c++
void writeBinaryTree(BinaryTree *p, ostream &out) {
  if (!p) {
    out << "# ";
  } else {
    out << p->data << " ";
    writeBinaryTree(p->left, out);
    writeBinaryTree(p->right, out);
  }
}
```
```c++
void readBinaryTree(BinaryTree *&p, ifstream &fin) {
  int token;
  bool isNumber;
  if (!readNextToken(token, fin, isNumber)) 
    return;
  if (isNumber) {
    p = new BinaryTree(token);
    readBinaryTree(p->left, fin);
    readBinaryTree(p->right, fin);
  }
}
```

## IIPrinting Level and Edge of Binary Tree

**Question: Print Edge of Binary tree**

Thought and solution: [solution](https://articles.leetcode.com/print-edge-nodes-boundary-of-binary/)

Bacially divide to two side left and right and do DFS seperately with print indicator. 


```c++
/**
 * Definition for a binary tree node.
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    vector<int> boundaryOfBinaryTree(TreeNode* root) {
          vector<int> res;
          if (!root) return res;
          res.push_back(root->val);
          printLeftEdges(root->left, true, res);
          printRightEdges(root->right, true, res);
        return res;
    }
    
    void printLeftEdges(TreeNode* p, bool print, vector<int> & res){
        if(p == nullptr) return ;
        if(print || (!p->left && !p->right))
            res.push_back(p->val);
        printLeftEdges(p->left, print,res);
        printLeftEdges(p->right, print&& (p->left ? false: true ), res);
    }
    
    void printRightEdges(TreeNode * p, bool print, vector<int> & res){
        if(p == nullptr) return ;
        printRightEdges(p->left, print && (p->right? false:true), res);
        printRightEdges(p->right,print, res);
        if(print || (!p->left && !p->right))
            res.push_back(p->val);
    }
};
```
Question: Binary Tree Zigzag Level Order Traversal https://leetcode.com/problems/binary-tree-zigzag-level-order-traversal/

// use two stacks

```c++
void printLevelOrderZigZag(TreeNode *root) {

	stack<TreeNode*> currentLevel, nextLevel;
	bool leftToRight = true;
	currentLevel.push(root);
	while (!currentLevel.empty()) {
		TreeNode *currNode = currentLevel.top();
		currentLevel.pop();
		if (currNode) {
			cout << currNode->val << " ";
			if (leftToRight) {
				nextLevel.push(currNode->left);
				nextLevel.push(currNode->right);
			} else {
				nextLevel.push(currNode->right);
				nextLevel.push(currNode->left);
			}
		}
		if (currentLevel.empty()) {
			cout << endl;
			leftToRight = !leftToRight;
			swap(currentLevel, nextLevel);
		}
	}
}

void swap(stack<TreeNode*> &stack1, stack<TreeNode*> &stack2) {
	stack<TreeNode*> tempStack = stack1;
	stack1 = stack2;
	stack2 = tempStack;
}
```



## III DFS tree

**Question : print the deepestPath of BST**
Pattern:
use backtracking

```
0. if head == null, return
1. result.push_back(head)
2. check if the head is leaf node, then do something
3. check if the head left node, go to left 
4. check if the heard right node, go to right, 
5. result.pop_back(head);
```

**Question :  dfs for the tree **
```c++
void Tree::dfstree(vector<TreeNode*> result, TreeNode* head) {

	if (head == NULL)
		return;
	result.push_back(head);
	// if find the leaf TreeNode, stop there and print out the path.
	if (head->left == NULL && head->right == NULL) {
		for (size_t i = 0; i < result.size(); i++) {
			cout << result[i]->val << " ";
		}
		cout << endl;
	}
	// if there is left TreeNode, go to the left TreeNode.
	if (head->left)
		dfstree(result, head->left);
	// if there is right TreeNode, go to the right TreeNode.
	if (head->right)
		dfstree(result, head->right);
	// pop out the last TreeNode and go back to the upper level.
	result.pop_back();
}
```
**Question: Print The DeepestPaht of tree**
```c++
void printDeepestPath(TreeNode *root, stack<TreeNode*> &path,
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
		printDeepestPath(root->left, path, result);
	if (root->right)
		printDeepestPath(root->right, path, result);

	path.pop();
}
```

**Question 129: sum Root to leaf numbers  **
```c++
int sumNumbers(TreeNode* root) {
    int sum = 0;
    int temp = 0;
    dfs(root, temp, sum);
    return sum;
}

void dfs(TreeNode* root, int temp, int& sum){
    if(root == NULL) return;
    temp = temp*10+ root->val;
    if(root->left == NULL && root->right == NULL)
        sum += temp;
    if(root->left != NULL)
        dfs(root->left, temp, sum);
    if(root->right != NULL)
        dfs(root->right, temp, sum);
}
```
** Question 124: Binary Tree Maximum Path Sum II **
There are several options: 
- if left path is bigger than 0, including the left path, otherwise not including
- if right path is bigger than 0, including the right path, otherwise not including
- The return part only return either left or right sub-path plus current node value, 

```c++

int maxPathSum(TreeNode* root) {
    int result=INT_MIN;
    help(root, result);
    return result;
}
/*** return the max-value-ended-at-root-node ***/
int help(TreeNode* root, int& result){
    if(!root)    return 0;
    int left=max(0, help(root->left, result));
    int right=max(0, help(root->right, result));
    /*** key parts : embedding the max-value-find in the recursion process ***/
    result=max(result, left+right+root->val);
    return max(left, right)+root->val;
}
```

## IV Traveral Related

Question: [Inorder Successor in BST](https://leetcode.com/problems/inorder-successor-in-bst/) 

```c++

TreeNode * Tree::findNextInorderNode(TreeNode* root, TreeNode * n) {
    // 1. if n has right node, then right nodes leftMost is the result.
    if( n->right != NULL )
        return minValue(n->right);
    // 2. if not then find the parent.
    TreeNode *succ = NULL;
    // Start from root and search for successor down the tree
    while (root != NULL)
    {
        if (n->val < root->val)
        {
            succ = root;
            root = root->left;
        }
        else if (n->val > root->val)
            root = root->right;
        else
           break;
    }
    return succ;
}

TreeNode * Tree::leftMostNode(TreeNode * node) {
    TreeNode* current = node;
    /* loop down to find the leftmost leaf */
    while (current->left != NULL) {
      current = current->left;
    }
    return current;
}

```



**Larget BST in Binary Tree**

https://articles.leetcode.com/largest-binary-search-tree-bst-in_22/

```c++
// Find the largest BST in a binary tree.
// This code does not delete dynamically-allocated nodes,
// so memory will be leaked upon exit.
// The min and max values are passed top-down to check if
// including a node satisfies the current BST constraint.
// The child nodes are passed bottom-up to be assigned 
// to its parent.
// Returns the total number of nodes the child holds.
int findLargestBST(BinaryTree *p, int min, int max, int &maxNodes, 
                   BinaryTree *& largestBST, BinaryTree *& child) {
  if (!p) return 0;
  if (min < p->data && p->data < max) {
    int leftNodes = findLargestBST(p->left, min, p->data, maxNodes, largestBST, child);
    BinaryTree *leftChild = (leftNodes == 0) ? NULL : child;
    int rightNodes = findLargestBST(p->right, p->data, max, maxNodes, largestBST, child);
    BinaryTree *rightChild = (rightNodes == 0) ? NULL : child;
    // create a copy of the current node and 
    // assign its left and right child.
    BinaryTree *parent = new BinaryTree(p->data);
    parent->left = leftChild;
    parent->right = rightChild;
    // pass the parent as the child to the above tree.
    child = parent;
    int totalNodes = leftNodes + rightNodes + 1;
    if (totalNodes > maxNodes) {
      maxNodes = totalNodes;
      largestBST = parent;
    }
    return totalNodes;
  } else {
    // include this node breaks the BST constraint,
    // so treat this node as an entirely new tree and 
    // check if a larger BST exist in this tree
    findLargestBST(p, INT_MIN, INT_MAX, maxNodes, largestBST, child);
    // must return 0 to exclude this node
    return 0;
  }
}

BinaryTree* findLargestBST(BinaryTree *root) {
  BinaryTree *largestBST = NULL;
  BinaryTree *child;
  int maxNodes = INT_MIN;
  findLargestBST(root, INT_MIN, INT_MAX, maxNodes, largestBST, child);
  return largestBST;
}
```



**Question: Largest subtree BST in a Binary Tree**

```c++

// Find the largest BST subtree in a binary tree.
// If the subtree is a BST, return total number of nodes.
// If the subtree is not a BST, -1 is returned.
int findLargestBSTSubtree(BinaryTree *p, int &min, int &max, 
                   int &maxNodes, BinaryTree *& largestBST) {
  if (!p) return 0;
  bool isBST = true;
  int leftNodes = findLargestBSTSubtree(p->left, min, max, maxNodes, largestBST);
  int currMin = (leftNodes == 0) ? p->data : min;
  if (leftNodes == -1 || 
     (leftNodes != 0 && p->data <= max))
    isBST = false;
  int rightNodes = findLargestBSTSubtree(p->right, min, max, maxNodes, largestBST);
  int currMax = (rightNodes == 0) ? p->data : max;
  if (rightNodes == -1 || 
     (rightNodes != 0 && p->data >= min))
    isBST = false;
  if (isBST) {
    min = currMin;
    max = currMax;
    int totalNodes = leftNodes + rightNodes + 1;
    if (totalNodes > maxNodes) {
      maxNodes = totalNodes;
      largestBST = p;
    }
    return totalNodes;
  } else {
    return -1;   // This subtree is not a BST
  }
}
 
BinaryTree* findLargestBSTSubtree(BinaryTree *root) {
  BinaryTree *largestBST = NULL;
  int min, max;
  int maxNodes = INT_MIN;
  findLargestBSTSubtree(root, min, max, maxNodes, largestBST);
  return largestBST;
}
```

**Question: Least Common Ancestor of a Binary Tree**

Here what we need to know is how to do the bottom-up way recursively traversal the tree and solve the similar problem.

 

```c++
// This is bottom up solution with O(n), bottom up from recursion.
TreeNode* lowestCommonAncestor(TreeNode* root, TreeNode* p, TreeNode* q) {
    if (!root) return NULL;
    if (root == p || root == q) return root;
    TreeNode *left = lowestCommonAncestor(root->left, p, q);
    TreeNode *right = lowestCommonAncestor(root->right, p, q);
    if (left && right) return root;  // if p and q are on both sides
    if (left != NULL) return left;
    	else return right; // return either side which has p or q.
}
```

Question: Least Common Ancestor of a Binary tree, knowing the parent node

```c++
Node *LCA(Node *root, Node *p, Node *q) {
  hash_set<Node *> visited;
  while (p || q) {
    if (p) {
      if (!visited.insert(p).second)
        return p; // insert p failed (p exists in the table)
      p = p->parent;
    }
    if (q) {
      if (!visited.insert(q).second)
        return q; // insert q failed (q exists in the table)
      q = q->parent;
    }
  }
  return NULL;
}
```

