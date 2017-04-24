
### Subarray Sum
Given an integer array, find a subarray where the sum of numbers is zero. 
http://www.lintcode.com/en/problem/subarray-sum/

```
#include <unordered_map>
#include <utility> //pair

class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: A list of integers includes the index of the first number 
     *          and the index of the last number
     */
    
	// NOTE: 无序数组的题目如果要O(n)解法往往要用到hash table
    typedef std::unordered_map<int,std::vector<int>> CumSumMap;
    vector<int> subarraySum(vector<int> nums){
        std::vector<int> ret_begin_end(2,0);
        if(nums.empty()) return ret_begin_end;
        
        CumSumMap cum_sum_map;
        cum_sum_map.insert(std::pair< int, std::vector<int> >(0,ret_begin_end));
        
        int count(0);
        for(int it(0); it < nums.size(); ++it){
            count += nums[it];
            
            CumSumMap::const_iterator map_it = cum_sum_map.find(count);
            std::vector<int> begin_end(2,0);
            if(map_it == cum_sum_map.end()){
                begin_end[0] = it+1;
                cum_sum_map.insert(std::pair< int, std::vector<int> >(count,begin_end));
            } else {
                begin_end =(map_it->second);
                begin_end[1] = it;
                return begin_end;
            }
        }
        
        return ret_begin_end;
    }
};
```
