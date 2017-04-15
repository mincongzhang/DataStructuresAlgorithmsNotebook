###　Search in Rotated Sorted Array
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
You are given a target value to search. If found in the array return its index, otherwise return -1.
You may assume no duplicate exists in the array.
http://www.lintcode.com/en/problem/search-in-rotated-sorted-array/

```
class Solution {
    /** 
     * param A : an integer ratated sorted array
     * param target :  an integer to be searched
     * return : an integer
     */
public:
    int search(vector<int> &A, int target) {
        if(A.empty()) return -1;
        
        int begin(0),end(A.size()-1);
        
        while(begin <= end){
            int mid = (begin+end)/2;
            if(A[mid] == target) return mid;
            
            //[begin,mid] unorder
            if(A[mid] < A[end]){
                if(target > A[mid] && target <=A[end]){ //NOTE: beware of the "<="
                    begin = mid + 1;
                } else {
                    end = mid-1;
                }
            //[mid,end] unorder
            } else {
                if(target >= A[begin] && target < A[mid]){  //NOTE: beware of the ">="
                    end = mid-1;
                } else {
                    begin = mid+1;
                }
            }
        }
        
        return -1;
    }
};
```