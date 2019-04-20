# Heap and dequeue

**Question: sliding-window-maximum** [](https://leetcode.com/problems/sliding-window-maximum/)

[answer online](https://articles.leetcode.com/sliding-window-maximum/) 

```c++
    // heap solution
 /*   vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> res;
        priority_queue <Pair> queue;
        
        if(k == 0 || nums.size() == 0) return res;  // check the bounding
        
        for(int i=0; i<k; i++){
            queue.push(Pair(nums[i],i));
        }
        
        for(int i=k; i<nums.size(); i++){
            Pair item = queue.top();
            res.push_back(item.first);
            while(item.second <= i-k){
                queue.pop();
                if(queue.size() == 0) break; // check the queue
                item = queue.top();
            }
            queue.push(Pair(nums[i], i));
        }
        
        res.push_back((queue.top()).first);
        
        return res;
    } */
    
    
    /*  double linked list solution, you only need to keep the queue with the highest item.
    */vector<int> maxSlidingWindow(vector<int>& A, int k) {
        vector<int> res;
        deque<int> queue;
        
        if (A.size() == 0 || k == 0) return res;
        for(int i = 0; i<k; i++){
            while(!queue.empty() && A[queue.back()] <= A[i]){
                queue.pop_back();
            }
            queue.push_back(i);
        }
        
        for(int i =k; i<A.size(); i++){
            res.push_back(A[queue.front()]);
            while(!queue.empty() && A[queue.back()] <= A[i]){
                queue.pop_back();
            }
            while(!queue.empty() && ( queue.front() <= i-k) ){
                queue.pop_front();
            }
            queue.push_back(i);
        }
        
        res.push_back(A[queue.front()]);
        return res;    
    }
```

Question LRU cache

```c++
class LRUCache{
private:
struct CacheNode {
    int key;
    int value;
    CacheNode(int k, int v) :key(k), value(v){}
};
public:
LRUCache(int capacity) {
	this->capacity = capacity;
}

int get(int key) {
	if (cacheMap.find(key) == cacheMap.end()) return -1;
	cacheList.splice(cacheList.begin(), cacheList, cacheMap[key]);
	cacheMap[key] = cacheList.begin();
	return cacheMap[key]->value;
}
void set(int key, int value) {
    if (cacheMap.find(key) == cacheMap.end()) {
        if (cacheList.size() == capacity) { //
            cacheMap.erase(cacheList.back().key);
            cacheList.pop_back();
        }
        cacheList.push_front(CacheNode(key, value));
        cacheMap[key] = cacheList.begin();
    } else {
        cacheMap[key]->value = value;
        cacheList.splice(cacheList.begin(), cacheList, cacheMap[key]);
        cacheMap[key] = cacheList.begin();
    }
}
private:
    list<CacheNode> cacheList;
    unordered_map<int, list<CacheNode>::iterator> cacheMap;
    int capacity;
};
```

