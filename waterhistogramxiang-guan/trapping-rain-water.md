### Trapping Rain Water

Given n non-negative integers representing an elevation map where the width of each bar is 1, compute how much water it is able to trap after raining.

For example,   
Given \[0,1,0,2,1,0,1,3,2,1,2,1\], return 6.

https://leetcode.com/problems/trapping-rain-water/description/



![](/assets/trapping_rain_water.png)

```
//2 pointer
class Solution {
public:
    int trap(vector<int>& height) {
        if(height.empty() || height.size()<=2) return 0;
        
        int left(0),right(height.size()-1);
        while(height[left]==0) left++;
        while(height[right]==0) right--;
        
        int count(0),left_max(0),right_max(0);
        while(left<right){
            if(height[left]<height[right]){
                left_max = std::max(height[left],left_max);
                if(height[left+1]<left_max){
                    count += left_max-height[left+1];
                }
                left++;
            } else {
                right_max = std::max(height[right],right_max);
                if(height[right-1]<right_max){
                    count += right_max - height[right-1];
                }
                right--;
            }          
        }
        
        return count;
    }
};
```

```
//Dynamic programming
class Solution {
public:
    int trap(vector<int>& height) {
        vector<int> left_max  = height;
        vector<int> right_max = height;
        
        for(int i=1;i<left_max.size();++i){
            left_max[i] = std::max(left_max[i-1],left_max[i]);
        }
        
        for(int i=right_max.size()-2; i>=0; --i){
            right_max[i] = std::max(right_max[i+1],right_max[i]);
        }
        
        int count=0;
        for(int i=0; i<height.size(); ++i){
            count+=std::min(right_max[i],left_max[i])-height[i];
        }
        
        return count;
    }
};
```