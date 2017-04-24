### Maximum Subarray
Given an array of integers, find a contiguous subarray which has the largest sum.  
http://www.lintcode.com/en/problem/maximum-subarray/  

```
//Solution1 O(n^2)
//For every element, go forward and find max, update global max

//Solution2 O(nlogn)
//Binary search, search left, and search right, and recursively solve the problem

//Solution3 O(n) update local max and global max at the same time
class Solution {
public:
  /**
   * @param nums: A list of integers
   * @return: A integer indicate the sum of max subarray
   */
  int maxSubArray(vector<int> nums) {
    if(nums.empty()) return 0;

    int local_max(INT_MIN),max(INT_MIN);
    for(size_t i=0; i<nums.size(); ++i){
      if(local_max<0){
        local_max = nums[i];
      } else {
        local_max += nums[i];
      }

      if(local_max > max){
        max = local_max;
      }
    }

    return max;
  }
};
```