#Tree Related Questions

## a list of questions on leetcode
https://articles.leetcode.com/category/binary-tree/


Question: Saving a Binary Search Tree to a File
use preorder traversal

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

Question: Saving a Binary Tree to a file

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

Read from file to Binary tree

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

Question: Serialization/Deserialization of a Binary Tree
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

Question: Print Edge of Binary tree

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