### Find Peak element
There is an integer array which has the following features:

The numbers in adjacent positions are different.
A[0] < A[1] && A[A.length - 2] > A[A.length - 1].
We define a position P is a peek if:

A[P] > A[P-1] && A[P] > A[P+1]

http://www.lintcode.com/en/problem/find-peak-element/

```
//O(logn)
class Solution {
public:
    /**
     * @param A: An integers array.
     * @return: return any of peek positions.
     */
    int findPeak(vector<int> A) {
        if(A.size()<3) return 0;
        if(A.size()==3) return 1;
        
        size_t left = 1;
        size_t right = A.size()-2;
        size_t mid;
        
        while(left<=right){
            mid = (left+right)/2;
            //std::cout<<left<<","<<right<<","<<mid<<std::endl;
            if(A[mid]>A[mid-1] && A[mid]>A[mid+1]){
                return mid;
            }
            
            //peak at left
            if(A[mid]>A[mid+1]){
                right = mid-1;
                continue;
            }
            
            //peak at right
            left = mid+1;
        }
        
        return mid;
    }
};
```

```
//O(n)
class Solution {
public:
    /**
     * @param A: An integers array.
     * @return: return any of peek positions.
     */
    int findPeak(vector<int> A) {
        for(int i=1;i<A.size()-1;++i){
            if(A[i]>A[i-1] && A[i]>A[i+1])
                return i;
        }
        
        return -1;
    }
};
```