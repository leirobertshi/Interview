# Greedy

Question [Jump I](https://leetcode.com/problems/jump-game/description/)



Greedy method

```c++
    bool canJump(vector<int>& nums) {
        int n = nums.size();
        int i = 0;
        for (int reach = 0; i < n && i <= reach; ++i)
            reach = max(i + nums[i], reach);
        return i == n;
    }
    
```

Question [Jump II](https://leetcode.com/problems/jump-game-ii/description/)



I try to change this problem to a BFS problem, where nodes in level i are all the nodes that can be reached in i-1th jump. for example. 2 3 1 1 4 , is
2||
3 1||
1 4 ||

clearly, the minimum jump of 4 is 2 since 4 is in level 3. my ac code.

```c++
 int jump(int A[], int n) {
	 if(n<2)return 0;
	 int level=0,currentMax=0,i=0,nextMax=0;

	 while(currentMax-i+1>0){		//nodes count of current level>0
		 level++;
		 for(;i<=currentMax;i++){	//traverse current level , and update the max reach of next level
			nextMax=max(nextMax,A[i]+i);
			if(nextMax>=n-1)return level;   // if last element is in level+1,  then the min jump=level 
		 }
		 currentMax=nextMax;
	 }
	 return 0;
 }
```

