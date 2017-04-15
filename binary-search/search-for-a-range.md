### Search for a Range
Given a sorted array of n integers, find the starting and ending position of a given target value.
If the target is not found in the array, return [-1, -1].

http://www.lintcode.com/en/problem/search-for-a-range/

```
class Solution {
private:
    void getRange(const vector<int> & A, int target, int target_i, vector<int> & begin_end){
        int begin(target_i),end(target_i);
        while(begin >= 0){
            if(A[begin] != target){ break; }
            begin_end[0] = begin;
            begin--;
        }
        
        while(end < A.size()){
            if(A[end] != target) { break; }
            begin_end[1] = end;
            end++;
        }
    }
    
    /** 
     *@param A : an integer sorted array
     *@param target :  an integer to be inserted
     *return : a list of length 2, [index1, index2]
     */
public:
    vector<int> searchRange(vector<int> &A, int target) {
        vector<int> begin_end(2,-1);
        if(A.empty()) return begin_end;
        
        int target_i(-1);
        int begin(0),end(A.size()-1);
        while(begin <= end){
            int mid = (begin + end)/2;
            
            if(A[mid] > target){
                end = mid-1;
                continue;
            }
            
            if(A[mid] < target){
                begin = mid+1;
                continue;
            }
            
            target_i = mid;
            break;
        }
        
        if(target_i == -1) { return begin_end; }
        getRange(A,target,target_i,begin_end);
        return begin_end;
    }
};
```
