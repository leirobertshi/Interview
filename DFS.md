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

Question ; Restore [IP](https://leetcode.com/problems/restore-ip-addresses/)

```c++
vector<string> restoreIpAddresses(const string& s) {
    vector<string> result;
    vector<string> ip; //
    dfs(s, ip, result, 0);
    return result;
}

void dfs(string s, vector<string>& ip, vector<string> &result,
size_t start) {
    if (ip.size() == 4 && start == s.size()) { //
        result.push_back(ip[0] + '.' + ip[1] + '.' + ip[2] + '.' + ip[3]);
        return;
    }
    if (s.size()- start > (4 - ip.size()) * 3) 
        return;
    if (s.size()- start < (4 - ip.size()))
        return;
    int num = 0;
    for (size_t i = start; i < start + 3; i++) {
        num = num * 10 + (s[i] - '0');
        if (num < 0 || num > 255) continue;
    //
        ip.push_back(s.substr(start, i - start + 1));
        dfs(s, ip, result, i + 1);
        ip.pop_back();
        if (num == 0) break; // can't have '0' as begin.
    }
}
```





## BFS

Question Word [Ladder](https://leetcode.com/problems/word-ladder/)

```c++
int ladderLength(string start, string end, unordered_set<string> &dict) {
        
        // Using a map for two purposes: 
        //   1) store the distince so far.
        //   2) remove the duplication 
        map<string, int> dis;
        dis[start] = 1;
        
        queue<string> q;
        q.push(start);
        
        while(!q.empty()){

            string word = q.front(); 
            q.pop();
            
            if (word == end) {
                break;
            }
            
            for (int i=0; i<word.size(); i++){
                string temp = word;
                for(char c='a'; c<='z'; c++){
                    temp[i] = c;
                    if (dict.count(temp)>0 && dis.count(temp)==0){
                        dis[temp] = dis[word] + 1;
                        q.push(temp);
                    }
                }
            }
        }
        return (dis.count(end)==0) ? 0 : dis[end];
        
    }
```

[Panlindorme Partition](https://leetcode.com/problems/palindrome-partitioning/) I

```c++

bool isPalindrome(string &s, int start, int end)  {  

    while(start < end)  
    {  
        if(s[start] != s[end]) { 
            return false;  
        }
        start++; end--;  
    }  
    return true;  
}  

// DFS - Deepth First Search
//    
//   For example: "aaba"
//    
//                     +------+           
//              +------| aaba |-----+     
//              |      +------+     |     
//            +-v-+              +-v--+  
//            | a |aba           | aa |ba
//        +---+---+--+           +--+-+  
//        |          |              |    
//      +-v-+     +--v--+         +-v-+  
//      | a |ba   | aba |\0       | b |a 
//      +-+-+     +-----+         +-+-+  
//        |        a, aba           |    
//      +-v-+                     +-v-+  
//      | b |a                    | a |\0
//      +-+-+                     +---+  
//        |                      aa, b, a
//      +-v-+                            
//      | a |\0                          
//      +---+                            
//    a, a, b, a                         
//
//   You can see this algorithm still can be optimized, becasue there are some dupliation.
//   (  The optimization you can see the "Palindrome Partitioning II" )
//
   
void partitionHelper(string &s, int start, vector<string> &output, vector< vector<string> > &result) {

    if (start == s.size()) {
        result.push_back(output);
        return;
    }
    for(int i=start; i<s.size(); i++) {
        if ( isPalindrome(s, start, i) == true ) {
            //put the current palindrome substring into ouput
            output.push_back(s.substr(start, i-start+1) );
            //Recursively check the rest substring
            partitionHelper(s, i+1, output, result);
            //take out the current palindrome substring, in order to check longer substring.
            output.pop_back();
        }
    }
}

vector< vector<string> > partition(string s) {

    vector< vector<string> > result;
    vector<string> output;

    partitionHelper(s, 0,  output, result);

    return result;

}

void printMatrix(vector< vector<string> > &matrix)
{
    for(int i=0; i<matrix.size(); i++){
        cout << "{ ";
        for(int j=0; j< matrix[i].size(); j++) {
            cout << matrix[i][j] << ", ";
        }
        cout << "}" << endl;
    }
    cout << endl;
}


```





Question Word Ladder II [](https://leetcode.com/problems/word-ladder/)

```

```

