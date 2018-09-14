# DP Related

1. Question : Given a string containing just the characters `'('` and `')'`, find the length of the longest valid (well-formed) parentheses substring.

    Answer: [link to solution on leetcodeo](https://leetcode.com/problems/longest-valid-parentheses/solution/)

   - Bruit Force , Every substr, check if the string is valid, by checking the string with stack, meeting left, insert meeting right, pop-up
   - DP
   - Using stack while tracking the length, insert the indice to stack, first insert "-1"
   - count left-count and right-count from left to right and from right to left.

2. Question: Unique Binary Search Trees, Given *n*, how many structurally unique **BST's** (binary search trees) that store values 1 ... *n*?

   - Solution: [link](https://leetcode.com/problems/unique-binary-search-trees/discuss/31666/DP-Solution-in-6-lines-with-explanation)
   - draw it and form it as DP

3. Question: Longest Palindromic Substring, Given a string **s**, find the longest palindromic substring in **s**. You may assume that the maximum length of **s** is 1000. "aba" is also a valid answer.

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
     

4. Question: [Maximum Subarray](https://leetcode.com/problems/maximum-subarray) Given an integer array `nums`, find the contiguous subarray (containing at least one number) which has the largest sum and return its sum.

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

     

5. Question: [Unique Paths](https://leetcode.com/problems/unique-paths)

   strict forword

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

6. Question:[ Unique Paths II](https://leetcode.com/problems/unique-paths-ii)

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

7. 

   - 

   

