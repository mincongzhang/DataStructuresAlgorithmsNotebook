### Maximum Subarray

Given an array of integers, find a contiguous subarray which has the largest sum.  
[http://www.lintcode.com/en/problem/maximum-subarray/](http://www.lintcode.com/en/problem/maximum-subarray/)

```
//Solution1 O(n^2)
//For every element, go forward and find max, update global max

//Solution2 O(nlogn)
//Binary search, the max is either included in the middle or not, 
//get mid, search left, and search right, update max

//Solution3 O(n) DP
//update local max and global max at the same time
maxSubArray(A, i) = A[i] + maxSubArray(A, i - 1) > 0 ? maxSubArray(A, i - 1) : 0; 

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

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        if(nums.empty()) return 0;
        
        vector<int> max_v = nums;
        int max = nums[0];
        for(int i=1; i<nums.size();++i){
            max_v[i] = (max_v[i-1]<0? 0 : max_v[i-1]) + nums[i];
            max = std::max(max,max_v[i]);
        }
        
        return max;
    }
};
```

```
class Solution {
public:
    int maxSubArray(vector<int>& nums) {
        int max(INT_MIN),sum(0);
        for(int i=0;i<nums.size();++i){
            if(sum<0){
                sum=0;
            }
            sum += nums[i];
            max = std::max(max,sum);
        }
        
        return max;
    }
};
```



