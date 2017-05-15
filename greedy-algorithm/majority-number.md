###  Majority Number
Given an array of integers, the majority number is the number that occurs more than half of the size of the array. Find it.
http://www.lintcode.com/en/problem/majority-number/

```
//Sorting then find the majority number, O(nlogn)
//Can also use hashtable, O(n) but also need O(n) space

#include <algorithm>

class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: The majority number
     */
    int majorityNumber(vector<int> nums) {
        if(nums.empty()) return 0;
        
        std::sort(nums.begin(),nums.end());
        return nums[nums.size()/2];
    }
};
```

```
//投票算法
https://segmentfault.com/a/1190000004905350

class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return: The majority number
     */
    int majorityNumber(vector<int> nums) {
        int count = 0;
        int major_num = 0;
        
        //Voting algorithm
        //if 2 nums diff, cancel out each other
        //the remaining number should be the major number
        
        for(int i=0; i<nums.size(); ++i){
            if(count==0){
                major_num = nums[i];
                count++;
                continue;
            }
            
            if(nums[i]!=major_num){
                count--;
            } else {
                count++;
            }
        }
        
        return major_num;
    }
};


```