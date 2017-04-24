
###  Maximum Product Subarray
Find the contiguous subarray within an array (containing at least one number) which has the largest product.  
http://www.lintcode.com/en/problem/maximum-product-subarray/  

```
class Solution {
public:
  /**
   * @param nums: a vector of integers
   * @return: an integer
   */
  int maxProduct(vector<int>& nums) {
    int local_max(1),local_min(1),global_max(INT_MIN);
    for(size_t i=0; i<nums.size(); ++i){
    //Estimate max/min
      int est_max = local_max*nums[i];
      int est_min = local_min*nums[i];

      local_max  = std::max(nums[i],std::max(est_max,est_min));
      local_min  = std::min(nums[i],std::min(est_max,est_min));
      global_max = std::max(global_max,local_max);
    }

    return global_max;
  }
};
```