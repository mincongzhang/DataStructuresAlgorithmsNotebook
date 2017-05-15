### Jump Game

Given an array of non-negative integers, you are initially positioned at the first index of the array.  
Each element in the array represents your maximum jump length at that position.  
Determine if you are able to reach the last index.  
[http://www.lintcode.com/en/problem/jump-game/](http://www.lintcode.com/en/problem/jump-game/)

```
class Solution {
public:
    /**
     * @param A: A list of integers
     * @return: The boolean answer
     */
    bool canJump(vector<int> A) {
        int longest(0);
        for(int i=0; i<A.size();++i){
            int can_reach = i+A[i];
            if(can_reach>longest){
                longest = can_reach;
                continue;
            } 

            if(longest>= A.size()-1){return true;}
            if(longest <= i){ return false; }
        }


        return longest >= A.size()-1;
    }
};
```

```
//Dynamic Programming O(n^2)
class Solution {
private:
    bool canJump(const vector<int> & A, int i){
        if(A[i]+i <= i) return false;
        if(A[i]+i >= A.size()-1) return true;

        int step = A[i];
        bool can = false;
        while(step){
            can = can | canJump(A,i+step);
            step--;
        }
        return can;
    }

public:
    /**
     * @param A: A list of integers
     * @return: The boolean answer
     */
    bool canJump(vector<int> A) {
        if(A.size()<=1) return true;
        return canJump(A,0);
    }
};
```



