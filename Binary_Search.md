# Binary Search



## Template One Basic

```c++
int binarySearch(vector<int>& nums, int target){
  if(nums.size() == 0)
    return -1;

  int left = 0, right = nums.size() - 1;
  while(left <= right){
    // Prevent (left + right) overflow
    int mid = left + (right - left) / 2;
    if(nums[mid] == target){ return mid; }
    else if(nums[mid] < target) { left = mid + 1; }
    else { right = mid - 1; }
  }

  // End Condition: left > right
  return -1;
}
```

------

- Most basic and elementary form of Binary Search
- Search Condition can be determined without comparing to the element's neighbors (or use specific elements around it)
- No post-processing required because at each step, you are checking to see if the element has been found. If you reach the end, then you know the element is not found



- Initial Condition:` left = 0, right = length-1`
- Termination: `left > right`
- Searching Left: `right = mid-1`
- Searching Right: `left = mid+1`

**Question 1:**

Compute and return the square root of *x*, where *x* is guaranteed to be a non-negative integer.

Since the return type is an integer, the decimal digits are truncated and only the integer part of the result is returned.

```c++
    int mySqrt(int x) {
        if (x<=1)
            return x;        
        int left = 1, right = x/2;
        while (true) {
            int mid = (left + right)/2;            
            if (mid > 0 && mid > x/mid) {
                right = mid-1;
            } else if ((mid+1) > x/(mid+1)) {
                return mid;
            } else {
                left = mid+1;
            }
        }
    }
```



[Search in Roated Array](https://leetcode.com/problems/search-in-rotated-sorted-array/description/) 

Idea: **search in roated array**, check left and mid, if they are in order and if the target is between them. 

**search the minim in rotated array**, check mid and left, to see if left part is in ascending order. 

```c++
    int search(vector<int>& A, int target) {
    int lo = 0;
    int hi = A.size() - 1;
    while (lo <= hi) {
        int mid = (lo + hi) / 2;
        if (A[mid] == target) return mid;
        
        // check which side is in descent order and if the target is in it. 
        if (A[lo] <= A[mid]) {
            if (target >= A[lo] && target < A[mid]) {
                hi = mid - 1;
            } else {
                lo = mid + 1;
            }
        } else {
            if (target > A[mid] && target <= A[hi]) {
                lo = mid + 1;
            } else {
                hi = mid - 1;
            }
        }
    }
        
        return -1;
    }
```



Search in Roated Array 2

```c++
    bool search(vector<int>& nums, int target) {
        int left = 0, right =  nums.size()-1, mid;
        
        while(left<=right)
        {
            mid = (left + right) >> 1;
            if(nums[mid] == target) return true;

            // the only difference from the first one, trickly case, just updat left and right
            if( (nums[left] == nums[mid]) && (nums[right] == nums[mid]) ) {++left; --right;}

            else if(nums[left] <= nums[mid])
            {
                if( (nums[left]<=target) && (nums[mid] > target) ) right = mid-1;
                else left = mid + 1; 
            }
            else
            {
                if((nums[mid] < target) &&  (nums[right] >= target) ) left = mid+1;
                else right = mid-1;
            }
        }
        return false;
    }
```





## Template 2

Template #2 is an advanced form of Binary Search. It is used to search for an element or condition which requires *accessing the current index and its immediate right neighbor's index* in the array.

 

```c++
int binarySearch(vector<int>& nums, int target){
  if(nums.size() == 0)
    return -1;

  int left = 0, right = nums.size();
  while(left < right){
    // Prevent (left + right) overflow
    int mid = left + (right - left) / 2;
    if(nums[mid] == target){ return mid; }
    else if(nums[mid] < target) { left = mid + 1; }
    else { right = mid; }
  }

  // Post-processing:
  // End Condition: left == right
  if(left != nums.size() && nums[left] == target) return left;
  return -1;
}
```



- An advanced way to implement Binary Search.
- Search Condition needs to access element's immediate right neighbor
- **Use element's right neighbor to determine if condition is met** and decide whether to go left or right
- Gurantees Search Space is at least 2 in size at each step
- **Post-processing required.** Loop/Recursion ends when you have 1 element left. Need to assess if the remaining element meets the condition.



- Initial Condition: `left = 0, right = length`
- Termination: `left == right`
- Searching Left: `right = mid`
- Searching Right: `left = mid+1`

**Question** [153 Find minimum in the rotated array](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/description/)

solution with Template 2

```c++
int findMin(vector<int> &num) {
        int start=0,end=num.size()-1;
        
        while (start<end) {
            if (num[start]<num[end])
                return num[start];
            
            int mid = (start+end)/2;
            
            if (num[mid]>num[end]) {
                start = mid+1;
            } else {
                end = mid;
            }
        }
        
        return num[start];
    }
```

**Idea about when to use template 1 and template 2**

Just think of it in general for a binary search, you have an array and you do two steps. First you see if what you're looking at now is the answer. In this case we want to see if start is less than end. Ok now if that's not true then you go on to the second step, which is to divide the array into two parts. From start to mid, and mid + 1 to end. Now you chose which side of the array brings your closer to the solution and go back to step 1.

The point is, if we do as you say, which is to say end = mid - 1, then you are always leaving out an element when you divide the array in half. So therefore you aren't dividing it into two contiguous halves.

Here's an example to prove the point.
Line 1 [4, 7, 8, 9, 13, 14, 18, 1, 2, 3]
Line 2 [4, 7, 8, 9, 13][14, 18, 1, 2, 3]
Line 3 [14, 18, 1, 2, 3]
Line 4[14, 18 , 1]
Line 5 [18, 1]
Line 6 [1]
Line 7 return start (1)

If we had said end = mid - 1, then line 4 would have excluded the 1, which is the answer.

Question : [154. Find Minimum in Rotated Sorted Array II](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array-ii/description/)

```c++
int findMin(vector<int> &num) {
    int start = 0;
    int end = num.size()-1;
    int mid;
    while(start<end){
        if(num[start]<num[end])
        	break;
        mid = start+(end-start)/2;
        if(num[mid]>num[end]){
            start = mid+1;
        }
        else if(num[mid]==num[end]){  // This is the part different from find minimum I. 
            start++;
            end--;
        }
        else
        end= mid;
    }
    return num[start];
 }
```



## 



Question: 278. [First Bad Version](https://leetcode.com/problems/first-bad-version/description/)

```c++
// Forward declaration of isBadVersion API.
bool isBadVersion(int version);

class Solution {
public:
    int firstBadVersion(int n) {
        int left = 1;
        int right = n;
        while(left < right){
            int mid = left + (right-left)/2;
            if(!isBadVersion(mid) && isBadVersion(mid+1)){
                return mid+1;
            }else if (!isBadVersion(mid) && !isBadVersion(mid+1)){
                left = mid+1;
            }else{
                right = mid;
            }
        }
        
        if(left != n+1 && isBadVersion(left)) return left;
        return 1;
    }
};
```

Question: [Search Insert](https://leetcode.com/problems/search-insert-position/description/)

```c++
int searchInsert(vector<int>& nums, int target) {
    
    int left = 0;
    int right = nums.size()-1;
    
    if (nums.size() == 0) return 0;
    if (nums[0] > target) return 0;
    if (nums[nums.size()-1] < target) return nums.size();
    
    while(left < right){
        int mid = left + (right-left)/2;
        if(nums[mid] == target) {return mid;}
        else if (nums[mid] < target){
            left=mid+1;
        }else{
            right = mid;
        }
    }
    return left;
}
```
**Question** [find peak element](https://leetcode.com/problems/find-peak-element/description/)  and similar one [Peak Index in a Mountain Array](https://leetcode.com/problems/peak-index-in-a-mountain-array/solution/)

A peak element is an element that is greater than its neighbors.

Given an input array `nums`, where `nums[i] ≠ nums[i+1]`, find a peak element and return its index.

The array may contain multiple peaks, in that case return the index to any one of the peaks is fine.

You may imagine that `nums[-1] = nums[n] = -∞`.

 

```c++
    int findPeakElement(vector<int>& nums) {
        int left = 0; int right = nums.size()-1;
        while(left < right){
            int mid = left + (right-left)/2;
            if(nums[mid] < nums[mid+1]){
                left= mid+1;
            }else{
                right = mid;
            }
        }
        
        return left;
    }
```

## Binary Search Template III



```c++
int binarySearch(vector<int>& nums, int target){
    if (nums.size() == 0)
        return -1;

    int left = 0, right = nums.size() - 1;
    while (left + 1 < right){
        // Prevent (left + right) overflow
        int mid = left + (right - left) / 2;
        if (nums[mid] == target) {
            return mid;
        } else if (nums[mid] < target) {
            left = mid;
        } else {
            right = mid;
        }
    }

    // Post-processing:
    // End Condition: left + 1 == right
    if(nums[left] == target) return left;
    if(nums[right] == target) return right;
    return -1;
}
```

It is used to search for an element or condition which requires *accessing the current index and its immediate left and right neighbor's index* in the array.







- An alternative way to implement Binary Search
- Search Condition needs to access element's immediate left and right neighbors
- Use element's neighbors to determine if condition is met and decide whether to go left or right
- Gurantees Search Space is at least 3 in size at each step
- Post-processing required. Loop/Recursion ends when you have 2 elements left. Need to assess if the remaining elements meet the condition.

 

- Initial Condition:` left = 0, right = length-1`
- Termination: `left + 1 == right`
- Searching Left: `right = mid`
- Searching Right: `left = mid`



## **Other Search**



Question: 74 [Search a 2D matrix](https://leetcode.com/problems/search-a-2d-matrix/description/) and [240. Search a 2D Matrix II](https://leetcode.com/problems/search-a-2d-matrix-ii/description/)

Search from top right or bottom left. 

```c++
bool searchMatrix(vector<vector<int>>& matrix, int target) {
    int m = matrix.size();
    if(m == 0) return false;
    int n = matrix[0].size();
    int j = n-1;
    int i = 0;
    while(i< m && j>=0){
        if(matrix[i][j] > target){
            j--;
        }else if(matrix[i][j] <target){
            i++;
        }else{
            return true;
        }
    }
    return false;
}
```



## To do

[Divide Two Integers](https://leetcode.com/problems/divide-two-integers/description/)

Search in Rotated sorted [array](https://leetcode.com/problems/search-in-rotated-sorted-array-ii/)

[Median of Two Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays)

