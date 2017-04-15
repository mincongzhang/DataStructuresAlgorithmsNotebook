
### Find Minimum in Rotated Sorted Array
Suppose a sorted array is rotated at some pivot unknown to you beforehand.
(i.e., 0 1 2 4 5 6 7 might become 4 5 6 7 0 1 2).
http://www.lintcode.com/en/problem/find-minimum-in-rotated-sorted-array/

```
class Solution {
public:
    /**
     * @param num: a rotated sorted array
     * @return: the minimum number in the array
     */
    int findMin(vector<int> &num) {
        if(num.empty()) return 0;
        
        int begin(0),end(num.size()-1);
        while(begin+1<end){
            int mid = (begin+end)/2;
            
            if(num[begin] > num[mid]){
                end = mid;
                continue;
            }
        
            if(num[mid] > num[end]){
                begin = mid;
                continue;
            }
            
            return num[begin]<num[end]? num[begin] : num[end];
        }
        
        return num[begin]<num[end]? num[begin] : num[end];
    }
};
```