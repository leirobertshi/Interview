### **Question 39** [Combination Sum I](https://leetcode.com/problems/combination-sum/description/) (**each number can be used for unlimited times**)

Idea:

1. Sort the candidate array first so as to generate non-descending order results.[Optional]

2. If target > 0, recursively call:

   - combinationSum(candidate, i, target - candidate[i]), i = start ... N - 1
   - if target == 0, found the item.

3. Avoid duplicated combinations

    (two ways):

   - skip candidate[i] if candidate[i] == candidate[i - 1], i > start; (Backtracking)
   - 
   - '[don't add the list if listSet.contains(list). (DFS)

```c++
class Solution {
public:
    std::vector<std::vector<int> > combinationSum(std::vector<int> &candidates, int target) {
        std::sort(candidates.begin(), candidates.end());
        std::vector<std::vector<int> > res;
        std::vector<int> combination;
        combinationSum(candidates, target, res, combination, 0);
        return res;
    }
private:
    void combinationSum(std::vector<int> &candidates, int target, std::vector<std::vector<int> > &res, std::vector<int> &combination, int begin) {
        if (!target) {
            res.push_back(combination);
            return;
        }
        for (int i = begin; i != candidates.size() && target >= candidates[i]; ++i) {
            combination.push_back(candidates[i]);
            combinationSum(candidates, target - candidates[i], res, combination, i);
            combination.pop_back();
        }
    }
};
```

```c++
// non-sorting solution
class Solution {
public:
    std::vector<std::vector<int> > combinationSum(std::vector<int> &candidates, int target) {
       
        std::vector<std::vector<int> > res;
        std::vector<int> combination;
        combinationSum(candidates, target, res, combination, 0);
        return res;
    }
private:
    void combinationSum(std::vector<int> &candidates, int target, std::vector<std::vector<int> > &res, std::vector<int> &combination, int begin) {
        if (!target) {
            res.push_back(combination);
            return;
        }
        for (int i = begin; i != candidates.size() && target >0; ++i) {   // non-sorting change
            combination.push_back(candidates[i]);
            combinationSum(candidates, target - candidates[i], res, combination, i);
            combination.pop_back();
        }
    }
};
```

c

### **Question 40** [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/submissions/1) (**each number can be used for only once**)

Idea : similarly to the last question.

Recursively call:

- addUp(candidate, i + 1, target - candidate[i]), i = start ... N - 1

将 candidate 的范围往前推一格，这样避免再次选用用过的 number

```c++
class Solution {
public:
    std::vector<std::vector<int> > combinationSum2(std::vector<int> &candidates, int target) {
        std::sort(candidates.begin(), candidates.end());
        std::vector<std::vector<int> > res;
        std::vector<int> combination;
        combinationSum2(candidates, target, res, combination, 0);
        return res;
    }
private:
    void combinationSum2(std::vector<int> &candidates, int target, std::vector<std::vector<int> > &res, std::vector<int> &combination, int begin) {
        if (!target) {
            res.push_back(combination);
            return;
        }
        for (int i = begin; i != candidates.size() && target >= candidates[i]; ++i)
            if (i == begin || candidates[i] != candidates[i - 1]) {
                combination.push_back(candidates[i]);
                combinationSum2(candidates, target - candidates[i], res, combination, i + 1);
                combination.pop_back();
            }
    }
};
```

Question [[Combination III]](https://leetcode.com/problems/combination-sum-iii/description/) 

```c++
public:
    std::vector<std::vector<int> > combinationSum3(int k, int n) {
        std::vector<std::vector<int> > res;
        std::vector<int> combination;
        combinationSum3(n, res, combination, 1, k);
        return res;
    }
private:
    void combinationSum3(int n, std::vector<std::vector<int> > &res, std::vector<int> &combination, int start, int k) {
 
        if (k==0 && n==0) {
            res.push_back(combination);
            return;
        }
        for (int i = start; i<= 9 && i<=n; i++) {
            combination.push_back(i);
            combinationSum3(n-i, res, combination, i+1, k-1);
            combination.pop_back();
        }
        

    }
```



Question 19 Common Combination 

Given two integers *n* and *k*, return all possible combinations of *k* numbers out of 1 ... *n*.



```c++
public:
    vector<vector<int> > res;
    void dfs(vector<int> &cand, int st, int ed, vector<int> &lr){
        if (lr.size()==ed){
            res.push_back(lr);
            return;
        }
        for (int i = st; i< cand.size();i++){
                lr.push_back(cand[i]);
                dfs(cand,i+1,ed,lr);
                lr.pop_back();
        }
         
         
    }
    vector<vector<int> > combine(int n, int k) {
        res.clear();
        if ((k<1)||(n<1)||(k>n)){return res;}
        vector <int> cand;
        for (int i = 1; i<=n;i++){
            cand.push_back(i);
        }
        vector<int> lr;
        dfs(cand,0,k,lr);
        return res;
    }
```



Question: [Letter Combinations of a Phone Number](https://leetcode.com/problems/letter-combinations-of-a-phone-number/submissions/1)



```c++
public:
    
    vector<string> letterCombinations(string phoneNum) {
        string str;
        vector<string> res;
        if (phoneNum.length() <1) return res;
	    doTeleWords(phoneNum, 0, str, res);
        return res;
    }
    
public: void doTeleWords(string phoneNum, int size, string &str, vector<string> &res) {

	// int maxLength = 7;

	if (size == phoneNum.length()) {
        res.push_back(str);
		return;
	}

	int number = phoneNum[size] -'0';
    // if(number <0 || number > 9) exit(1);

    string temp = getchars(number);
    for (int j = 0; j <temp.length(); j++) {
        char c = temp[j];
        str.push_back(c);
        doTeleWords(phoneNum, size + 1, str, res);
        str.erase(str.begin() + str.length() - 1);
    }

    }
    
public: string getchars(int num){
        vector<string> v = {"", "", "abc", "def", "ghi", "jkl", "mno", "pqrs", "tuv", "wxyz"};
        string temp = v[num];
        return temp;
    }
```



Question [Permutation](https://leetcode.com/problems/permutations/description/)

```c++
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> candidate;
        vector<bool> used(nums.size(), false);
        back_track(res, candidate,nums,0, used);
        return res;
    }
    
    void back_track(vector<vector<int>> &res, vector<int> &can, vector<int>& nums, int index , vector<bool> &used){
        if(index == nums.size()){
            res.push_back(can);
            return;
        }
        for(int i = 0; i<nums.size(); i++){
            if(used[i]) continue;
            can.push_back(nums[i]);
            used[i] = true;
            back_track(res, can,nums, index+1,used);
            used[i] =false;
            can.pop_back();
        }
    }
```

[Permutation II](https://leetcode.com/problems/permutations-ii/description/)  has duplicate item

```c++
    vector<vector<int>> permute(vector<int>& nums) {
        vector<vector<int>> res;
        vector<int> candidate;
        std::sort(nums.begin(), nums.end());
        vector<bool> used(nums.size(), false);
        back_track(res, candidate,nums,0, used);
        return res;
    }
    
    void back_track(vector<vector<int>> &res, vector<int> &can, vector<int>& nums, int index , vector<bool> &used){
        if(index == nums.size()){
            res.push_back(can);
            return;
        }
        for(int i = 0; i<nums.size(); i++){
            if(used[i] || (i>0 && nums[i-1] == nums[i] && !used[i-1] )) continue;
            can.push_back(nums[i]);
            used[i] = true;
            back_track(res, can,nums, index+1,used);
            used[i] =false;
            can.pop_back();
        }
    }
```

The reason that we use (**i>0 && nums[i-1] == nums[i] && !used[i-1] )**

First of all, the conclusion is both !use[i - 1] and use[i - 1] work, but the !use[i - 1] is more efficient.

Here goes the explanation:
If we use
if(i > 0 && nums[i] == nums[i - 1] && use[i - 1]) continue;
Chances are that we have the same value,and we will never have chance to use the second one, which causes the list will never grow to the length of nums.length, and waste exists.
Here we have an example of the simple [1, 2, 2']. We start at list = [], and in the first level of backtrack, we have:
[1]
[2]
[2']
In the second level, when list = [1], ve:
[1, 2]
[1, 2']
When list = [2], when i = 0 we have [2, 1] without difficulty, when i = 1, "2" is already used so we simply continue. But when i = 2, we found that nums[2] = nums[1] and nums[1] is already used, so we discard it and the list is still [1]. The problem is that when we have to figure out the next element of this track, we will find that the "2'" will never be used because nums[2] == nums[1] && used(nums[1]) is always true. This track will never reach the recursive base and be added to the final answer.

If we use
if(i > 0 && nums[i] == nums[i - 1] && !use[i - 1]) continue;
When we figure out the first element of the list,when i = 2 we find that nums[2] == nums[1] true and used(nums[1]) false, so we directly skip the list starting with "2'", and we get
[1] and
[2]
only. The same rule continues on the next steps so every list we generated will reach to an end and return.

That why !use[i - 1] is better. Hope that my explanation is clear enough.



Question next Permutation

[Thought process](https://leetcode.com/problems/next-permutation/solution/)

```c++
void nextPermutation(vector<int>& nums) {
    int i = nums.size() - 1, k = i;
    while (i > 0 && nums[i-1] >= nums[i])
        i--;
    for (int j=i; j<k; j++, k--)
        swap(nums[j], nums[k]);  // 1. swap the right side of divide item
    if (i > 0) {
        k = i--;
        while (nums[k] <= nums[i])
            k++;				
        swap(nums[i], nums[k]);  // 2. find the next big item and swap the item. 
    }
}
```

Question Subset

Given a set of **distinct** integers, *nums*, return all possible subsets (the power set).

**Note:** The solution set must not contain duplicate subsets. The same as combination on the top. 

 

```
Input: nums = [1,2,3]
Output:
[
  [3],
  [1],
  [2],
  [1,2,3],
  [1,3],
  [2,3],
  [1,2],
  []
]
```



```c++
    vector<vector<int>> subsets(vector<int>& S) {
        sort(S.begin(), S.end()); //
        vector<vector<int> > result;
        vector<int> path;
        dfs(S, S.begin(), path, result);
        return result;
    }
    
    void dfs(const vector<int> &S, vector<int>::iterator start,
vector<int> &path, vector<vector<int> > &result) {
result.push_back(path);
    for (auto i = start; i < S.end(); i++) {
        // if (i != start && *i == *(i-1)) continue;
        path.push_back(*i);
        dfs(S, i + 1, path, result);
        path.pop_back();
    }
    }
```

