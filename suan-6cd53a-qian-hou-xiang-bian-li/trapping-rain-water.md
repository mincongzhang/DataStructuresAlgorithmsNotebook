### Trapping Rain Water
Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

http://www.lintcode.com/en/problem/trapping-rain-water/
https://leetcode.com/problems/trapping-rain-water/#/description

```
class Solution {
public:
    int trapRainWater(vector<int>& height) {
        int max_h = INT_MIN;
        for(int i=0; i<height.size();++i){
            max_h = std::max(max_h,height[i]);
        }
        
        int total_water = 0;
        for(int h=1;h<=max_h;++h){
            int cur_water = 0;
            bool collect = false;
            for(int i=0;i<height.size();++i){
                //Start trap water or finish collect water
                if(height[i]>=h){
                    total_water+=cur_water;
                    cur_water = 0;
                    collect = true;
                    continue;
                }
                
                //height[i]<h
                if(collect){
                    cur_water++;
                }
            }
        }
        return total_water;
    }
};
```