# DP Problems

## Matrix Related

Question: **[Unique Paths](https://leetcode.com/problems/unique-paths)**

```c++
    int uniquePaths(int m, int n) {
        int matrix[m][n];
        for(int i = 0;i<m;i++){
            for(int j =0;j<n; j++){
                if (i ==0 || j==0) {
                    matrix[i][j] =1;
                    continue;
                }
                matrix[i][j] = matrix[i-1][j]+matrix[i][j-1];
            }
        }
        
        return matrix[m-1][n-1];
    }
```

Question:[ **Unique Paths II](https://leetcode.com/problems/unique-paths-ii)**

```c++
    int uniquePathsWithObstacles(vector<vector<int>>& obstacleGrid) {
        int m = obstacleGrid.size(), n = obstacleGrid[0].size();
        vector<vector<int> > dp(m + 1, vector<int> (n + 1, 0));
        dp[0][1] = 1;
        for (int i = 1; i <= m; i++)
            for (int j = 1; j <= n; j++)
                if (!obstacleGrid[i - 1][j - 1])
                    dp[i][j] = dp[i - 1][j] + dp[i][j - 1];
        return dp[m][n];
    } 
```

Explain:

In the **Unique Paths** problem, we initialize `P[0][j] = 1, P[i][0] = 1` for all valid `i, j`. Now, due to obstacles, some boundary points are no longer reachable and need to be initialized to `0`. For example, if `obstacleGrid` is like `[0, 0, 1, 0, 0]`, then the last three points are not reachable and need to be initialized to be `0`. The result is `[1, 1, 0, 0, 0]`.

*Now we can write down the following (unoptimized) code. Note that we pad the `obstacleGrid` by `1` and initialize `dp[0][1] = 1` to unify the boundary cases.*

[Minimum Path Sum](https://leetcode.com/problems/minimum-path-sum)

for first row and first col, needs to initialize seperately. 

```c++
    int minPathSum(vector<vector<int>>& grid) {
        int m = grid.size();
        int n = grid[0].size(); 
        vector<vector<int> > sum(m, vector<int>(n, grid[0][0]));
        for (int i = 1; i < m; i++)
            sum[i][0] = sum[i - 1][0] + grid[i][0];
        for (int j = 1; j < n; j++)
            sum[0][j] = sum[0][j - 1] + grid[0][j];
        for (int i = 1; i < m; i++)
            for (int j = 1; j < n; j++)
                sum[i][j]  = min(sum[i - 1][j], sum[i][j - 1]) + grid[i][j];
        return sum[m - 1][n - 1];
    }
```



## Fibonacci related

**[Climbing Stairs](https://leetcode.com/problems/climbing-stairs)**

```c++
    int climbStairs(int n) {
             vector<int> steps(n,0);
         steps[0]=1;
         steps[1]=2;
         for(int i=2;i<n;i++)
         {
             steps[i]=steps[i-2]+steps[i-1];
         }
         return steps[n-1];
    }
```

**[Unique Binary Search Trees](https://leetcode.com/problems/unique-binary-search-trees)**

[solution explain](https://leetcode.com/problems/unique-binary-search-trees/discuss/31666/DP-Solution-in-6-lines-with-explanation.

```c++
    int numTrees(int n) {
        int dp[n+1] ={};
        if(n == 1 || n ==0) return 1;
        dp[0] = 1;
        dp[1] = 1;
        int sum = 0;
        for(int i = 2; i<=n; i++){
            for(int j=1; j<=i; j++){
                dp[i] += dp[j-1]*dp[i-j];
            }
        }
        
        return dp[n];
        
    }
```



## Array Related

Question: **[Maximum Subarray](https://leetcode.com/problems/maximum-subarray)** Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

- So I change the format of the sub problem into something like: `maxSubArray(int A[], int i)`, which means the maxSubArray for A[0:i ] which must has A[i] as the end element. 

- maxSubArray(A, i) = maxSubArray(A, i - 1) > 0 ? maxSubArray(A, i - 1) : 0 + A[i]; 

- ```c++
      int maxSubArray(vector<int>& nums) {
          if (nums.size() == 0) exit(0);
          int dp [nums.size()];
          dp[0] = nums[0];
          int max = dp[0];
          
          for(int i=1; i<nums.size(); i++ ){
              dp[i] = nums[i]+ (dp[i-1]<0 ? 0:dp[i-1]);
              max = std::max(dp[i],max);
          }
          return max;
           
      }
  ```

## Left and Right:



Question: [maximal rectangle](https://leetcode.com/problems/maximal-rectangle/description/)

[solution](https://leetcode.com/problems/maximal-rectangle/discuss/29054/Share-my-DP-solution?page=1)

The DP solution proceeds row by row, starting from the first row. Let the maximal rectangle area at row i and column j be computed by [right(i,j) - left(i,j)]*height(i,j).

All the 3 variables left, right, and height can be determined by the information from previous row, and also information from the current row. So it can be regarded as a DP solution. The transition equations are:

left(i,j) = max(left(i-1,j), cur_left), cur_left can be determined from the current row

right(i,j) = min(right(i-1,j), cur_right), cur_right can be determined from the current row

height(i,j) = height(i-1,j) + 1, if matrix[i][j]=='1';

height(i,j) = 0, if matrix[i][j]=='0'

```c++
int maximalRectangle(vector<vector<char> > &matrix) {
    if(matrix.empty()) return 0;
    const int m = matrix.size();
    const int n = matrix[0].size();
    int left[n], right[n], height[n];
    fill_n(left,n,0); fill_n(right,n,n); fill_n(height,n,0);
    int maxA = 0;
    for(int i=0; i<m; i++) {
        int cur_left=0, cur_right=n; 
        for(int j=0; j<n; j++) { // compute height (can do this from either side)
            if(matrix[i][j]=='1') height[j]++; 
            else height[j]=0;
        }
        for(int j=0; j<n; j++) { // compute left (from left to right)
            if(matrix[i][j]=='1') left[j]=max(left[j],cur_left);
            else {left[j]=0; cur_left=j+1;}
        }
        // compute right (from right to left)
        for(int j=n-1; j>=0; j--) {
            if(matrix[i][j]=='1') right[j]=min(right[j],cur_right);
            else {right[j]=n; cur_right=j;}    
        }
        // compute the area of rectangle (can do this from either side)
        for(int j=0; j<n; j++)
            maxA = max(maxA,(right[j]-left[j])*height[j]);
    }
    return maxA;
}

```

Question: [**Trapping Rain Water**](https://leetcode.com/problems/trapping-rain-water/description/)

```c++
int trap(vector<int>& height)
{
	if(height == null)
		return 0;
    int ans = 0;
    int size = height.size();
    vector<int> left_max(size), right_max(size);
    left_max[0] = height[0];
    for (int i = 1; i < size; i++) {
        left_max[i] = max(height[i], left_max[i - 1]);
    }
    right_max[size - 1] = height[size - 1];
    for (int i = size - 2; i >= 0; i--) {
        right_max[i] = max(height[i], right_max[i + 1]);
    }
    for (int i = 1; i < size - 1; i++) {
        ans += min(left_max[i], right_max[i]) - height[i];
    }
    return ans;
}
```



Question: **[Largest Rectangle in histogram]**(https://leetcode.com/problems/largest-rectangle-in-histogram/description/)

Answer: [dp solution](https://leetcode.com/problems/largest-rectangle-in-histogram/discuss/28902/5ms-O(n)-Java-solution-explained-(beats-96)) record the left most and left most.



```c++
int largestRectangleArea(vector<int>& height) {
        if (heights == null || heights.size() == 0) return 0;
        int n = heights.size();
        int lessLeft[n];
        int lessRight[n];
        lessLeft[0] = -1;
        lessRight[n - 1] = n;
        for (int i = 1; i < height.length; i++) {
            int p = i - 1;

            while (p >= 0 && height[p] >= height[i]) {
                p = lessFromLeft[p];
            }
            lessFromLeft[i] = p;
        }

        for (int i = height.length - 2; i >= 0; i--) {
            int p = i + 1;

            while (p < height.length && height[p] >= height[i]) {
                p = lessFromRight[p];
            }
            lessFromRight[i] = p;
        }

        int maxArea = 0;
        for (int i = 0; i < height.length; i++) {
            maxArea = max(maxArea, height[i] * (lessFromRight[i] - lessFromLeft[i] - 1));
        }

        return maxArea;
}
```

[Stack solution](https://leetcode.com/problems/largest-rectangle-in-histogram/solution/) 

```c++
int largestRectangleArea(vector<int>& heights) {
        stack <int> stk;
        stk.push(-1);
        int max_area = 0;
        for (int i=0;i<heights.size();i++){
            while(stk.top()!=-1 && heights[i]<=heights[stk.top()]){
                int cur = stk.top();
                stk.pop();
                int dist = i-stk.top()-1;
                max_area = max(max_area, heights[cur]*dist);
            }
            stk.push(i);
        }

        while(stk.top()!=-1){
            int cur = stk.top();
            stk.pop();
            int dist = heights.size()-stk.top()-1;
            max_area = max(max_area, heights[cur]*dist);
        }
        return max_area;
    }
```

## Others:

Question:**[ Regular Expression Matching](https://leetcode.com/problems/regular-expression-matching)** 

Answer: [solution on leetcode](https://leetcode.com/problems/regular-expression-matching/discuss/5684/9-lines-16ms-C++-DP-Solutions-with-Explanations) [video](https://www.youtube.com/watch?v=l3hda49XcDE&list=PLrmLmBdmIlpuE5GEMDXWf0PWbBD9Ga1lO)

This problem has a typical solution using Dynamic Programming. We define the state `P[i][j]` to be `true` if `s[0..i)` matches `p[0..j)` and `false` otherwise. Then the state equations are:

1. `P[i][j] = P[i - 1][j - 1]`, if `p[j - 1] != '*' && (s[i - 1] == p[j - 1] || p[j - 1] == '.')`;
2. `P[i][j] = P[i][j - 2]`, if `p[j - 1] == '*'` and the pattern repeats for `0` times;
3. `P[i][j] = P[i - 1][j] && (s[i - 1] == p[j - 2] || p[j - 2] == '.')`, if `p[j - 1] == '*'` and the pattern repeats for at least `1` times. 

pay attention: 

for p\[i\]\[j\] =p\[i\-1]\[j\] , thinking of s="xa" and p="xa\*", since  s[i - 1] == p[j - 2], "a" is part of "a*", what you want to check is "x" and "xa\*", that  is P[i - 1][j]

for the code i starts with i=0, since string could empty and pattern "a*" can still match with empty string, so you need to start with i=0, but dp\[\*\]|[j\] where j=0, dp = false; since it won't match anything except when i ==0.

```c++
bool isMatch(string s, string p) {
        int m = s.length(), n = p.length(); 
        vector<vector<bool> > dp(m + 1, vector<bool> (n + 1, false));
        dp[0][0] = true;
        for (int i = 0; i <= m; i++)
            for (int j = 1; j <= n; j++)
                if (p[j - 1] == '*')
                    dp[i][j] = dp[i][j - 2] || (i > 0 && (s[i - 1] == p[j - 2] || p[j - 2] == '.') && dp[i - 1][j]);
                else dp[i][j] = i > 0 && dp[i - 1][j - 1] && (s[i - 1] == p[j - 1] || p[j - 1] == '.');
        return dp[m][n];
    }
```

 

Question : **[ Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses)** Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

Answer: [link to solution on leetcodeo](https://leetcode.com/problems/longest-valid-parentheses/solution/)

   - Bruit Force , Every substr, check if the string is valid, by checking the string with stack, meeting left, insert meeting right, pop-up
   - DP
   - Using stack while tracking the length, insert the indice to stack, first insert "-1"
   - count left-count and right-count from left to right and from right to left.

Question: **Unique Binary Search Trees**, Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1 ... *n*?

- Solution: [link](https://leetcode.com/problems/unique-binary-search-trees/discuss/31666/DP-Solution-in-6-lines-with-explanation)
- draw it and form it as DP

Question: [ Unique Binary Search Trees II](https://leetcode.com/problems/unique-binary-search-trees-ii) Given an integer *n*, generate all structurally unique **BST's** (binary search trees) that store values 1 ... *n*.

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
    vector<TreeNode *> generateTree(int from, int to)
{
    vector<TreeNode *> ret;
    if(to - from < 0) ret.push_back(0);
    else if(to - from == 0) ret.push_back(new TreeNode(from));
    else
    {
        for(int i=from; i<=to; i++)
        {
            vector<TreeNode *> l = generateTree(from, i-1);
            vector<TreeNode *> r = generateTree(i+1, to);

            for(int j=0; j<l.size(); j++)
            {
                for(int k=0; k<r.size(); k++)
                {
                    TreeNode * h = new TreeNode (i);
                    h->left = l[j];
                    h->right = r[k];
                    ret.push_back(h);
                }
            }
        }
    }
    return ret;
}

vector<TreeNode *> generateTrees(int n) {
    if (n==0) {
        vector<TreeNode*> x;
        return x;
    }
    return generateTree(1, n);
}
};
```

Question: [Longest Palindromic Substring](https://leetcode.com/problems/longest-palindromic-substring) Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000. "aba" is also a valid answer.

- get every substring and check palindromic, will take O(n^3),
- To improve will use the lower DP method to check palindromic, total O(n^2)

- 
  $$
  P(i,j) =\begin{cases}true & if S_{i}...S_{j} is\ palindromic\ string\\false & others \end{cases}
  $$
  

  therefore,
  $$
  P(i,j) = P(i+1,j-1)  \ if \ S_{i}==S_{j}
  $$
  Base cases are:
  $$
  P(i,i) = true\ P(i,i+1)=(S_{i}==S_{i+1})
  $$
  



Question: [Edit Distance](https://leetcode.com/problems/edit-distance) Given two words *word1* and *word2*, find the minimum number of operations required to convert *word1* to *word2*.

You have the following 3 operations permitted on a word:

1. Insert a character
2. Delete a character
3. Replace a character

[Answer](https://leetcode.com/problems/edit-distance/discuss/25846/20ms-Detailed-Explained-C++-Solutions-(O(n)-Space)): 

Putting these together, we now have:

1. `dp[i][0] = i`;
2. `dp[0][j] = j`;
3. `dp[i][j] = dp[i - 1][j - 1]`, if `word1[i - 1] = word2[j - 1]`;
4. `dp[i][j] = min(dp[i - 1][j - 1] + 1, dp[i - 1][j] + 1, dp[i][j - 1] + 1)`, otherwise.
   - Replace `word1[i - 1]` by `word2[j - 1]` (`dp[i][j] = dp[i - 1][j - 1] + 1 (for replacement)`);
   - Delete `word1[i - 1]` and `word1[0..i - 2] = word2[0..j - 1]` (`dp[i][j] = dp[i - 1][j] + 1 (for deletion)`);
   - Insert `word2[j - 1]` to `word1[0..i - 1]` and `word1[0..i - 1] + word2[j - 1] = word2[0..j - 1]` (`dp[i][j] = dp[i][j - 1] + 1 (for insertion)`).

```c++
int minDistance(string word1, string word2) { 
        int m = word1.length(), n = word2.length();
        vector<vector<int> > dp(m + 1, vector<int> (n + 1, 0));
        for (int i = 1; i <= m; i++)
            dp[i][0] = i; // from word1 with i length to ""
        for (int j = 1; j <= n; j++)
            dp[0][j] = j;  // from "" to word2 with j length
        for (int i = 1; i <= m; i++) {
            for (int j = 1; j <= n; j++) {
                if (word1[i - 1] == word2[j - 1]) 
                    dp[i][j] = dp[i - 1][j - 1]; // the next char is the same
                else dp[i][j] = min(dp[i - 1][j - 1] + 1, min(dp[i][j - 1] + 1, dp[i - 1][j] + 1)); // three cases like describe before
            }
        }
        return dp[m][n];
    }
```

