### Container With Most Water

Given n non-negative integers a1, a2, ..., an, where each represents a point at coordinate \(i, ai\). n vertical lines are drawn such that the two endpoints of line i is at \(i, ai\) and \(i, 0\). Find two lines, which together with x-axis forms a container, such that the container contains the most water.

Note: You may not slant the container and n is at least 2.

[https://leetcode.com/problems/container-with-most-water](https://leetcode.com/problems/container-with-most-water)

```
class Solution {
public:
    int maxArea(vector<int>& height) {
        size_t begin(0),end(height.size()-1);
        int max_area = 0;
        while(begin<end){
            int area = std::min(height[begin],height[end]) * (end-begin);
            max_area = std::max(max_area,area);
            if(height[begin] < height[end]){
                begin++;
            } else {
                end--;
            }
        }

        return max_area;
    }
};
```



