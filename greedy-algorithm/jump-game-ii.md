### Jump Game II
Given an array of non-negative integers, you are initially positioned at the first index of the array.
Each element in the array represents your maximum jump length at that position.
Your goal is to reach the last index in the minimum number of jumps.
http://www.lintcode.com/en/problem/jump-game-ii/

```
class Solution {
public:
    /**
     * @param A: A list of lists of integers
     * @return: An integer
     */
    int jump(vector<int> A) {
        int count(0);
        int longest(0);
        for(int i=0; i<A.size();++i){
            int can_reach = i+A[i];
            if(can_reach>longest){
                count++;
                longest = can_reach;
            } 
            
            if(longest>= A.size()-1){return count;}
        }
        
        
        return count;
    }
};
```