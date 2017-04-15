### Wood Cut

Given n pieces of wood with length L\[i\] \(integer array\). Cut them into small pieces to guarantee you could have equal or more than k pieces with the same length. What is the longest length you can get from the n pieces of wood? Given L & k, return the maximum length of the small pieces.

e.g.For L=\[232, 124, 456\], k=7, return 114.

http://www.lintcode.com/en/problem/wood-cut/

```
class Solution {
public:
    /** 
     *@param L: Given n pieces of wood with length L[i]
     *@param k: An integer
     *return: The maximum length of the small pieces.
     */
    int woodCut(vector<int> L, int k) {
        if(L.empty()) return 0;

        //find max log
        //binary search [0,max_log]

        int max_log(0);
        for(int i(0); i<L.size();++i){
            if(L[i]> max_log){ 
                max_log = L[i];
            }
        }

        long min_length(0),max_length(max_log);
        while(min_length+1<max_length){
            long cur_length = (min_length+max_length)/2;

            int log_count(0);
            for(int i(0); i<L.size();++i){
                log_count += L[i]/cur_length;
            }
            if(log_count < k){
                max_length = cur_length;
                continue;
            }

            min_length = cur_length;
        }

        return min_length;
    }
};
```



