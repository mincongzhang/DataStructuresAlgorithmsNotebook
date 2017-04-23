### Missing Number
Given an array containing n distinct numbers taken from 0, 1, 2, ..., n, find the one that is missing from the array.
For example,
Given nums = [0, 1, 3] return 2.
Note:
Your algorithm should run in linear runtime complexity. Could you implement it using only constant extra space complexity?

https://leetcode.com/problems/missing-number/#/description

```
class Solution {
public:
    int missingNumber(vector<int>& nums) {
        int size = nums.size();
        int sum = ((1+size)*size)/2;
        
        int actual_sum(0);
        for(int i=0;i<size;++i){
            actual_sum+=nums[i];
        }
        
        return sum-actual_sum;
    }
};
```