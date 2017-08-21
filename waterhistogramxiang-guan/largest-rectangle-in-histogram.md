### Largest Rectangle in Histogram

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

[http://www.lintcode.com/en/problem/largest-rectangle-in-histogram/](http://www.lintcode.com/en/problem/largest-rectangle-in-histogram/)
https://leetcode.com/problems/largest-rectangle-in-histogram/description/

```
//brute force

#include <queue>
class Solution {
public:
  /**
   * @param height: A list of integer
   * @return: The area of largest rectangle in the histogram
   */
  int largestRectangleArea(vector<int> &height) {
    int max_area = 0;

    int size = height.size();
    for(int i=0; i<size; ++i){
        //backward
        int area = 0;
        int temp_i = i-1;
        while(temp_i>=0){
            if(height[temp_i]>=height[i]){
                area+=height[i];
            } else {
                break;
            }
            temp_i--;
        }

        //forward(self included)
        temp_i = i;
        while(temp_i<size){
            if(height[temp_i]>=height[i]){
                area+=height[i];
            } else {
                break;
            }            
            temp_i++;
        }

        max_area = std::max(area,max_area);
    }

    return max_area;
  }
};
```

```
//stack
class Solution {
public:
    /**
     * @param height: A list of integer
     * @return: The area of largest rectangle in the histogram
     */
  int largestRectangleArea(std::vector<int> &height) {
    std::stack<int> increasing_height;
    int max_area = 0;
    int i = 0;
    while(i <= height.size()) {
      if (increasing_height.empty() ||
          (i < height.size() && height[i] > height[increasing_height.top()])) {
        std::cout<<"push:"<<i<<", value:"<<height[i]<<std::endl;
        increasing_height.push(i);
        ++i;
      } else {
        int h = height[increasing_height.top()];
        increasing_height.pop();
        int left = increasing_height.empty() ? -1 : increasing_height.top();
        max_area = std::max(max_area, h * (i - left - 1));
        if(left==-1){
          std::cout<<"height:"<<h<<", left: left, area:"<< h * (i - left - 1)<<std::endl;
        } else {
          std::cout<<"height:"<<h<<", left:"<<height[left]<<", area:"<< h * (i - left - 1)<<std::endl;
        }
      }
    }

    return max_area;
  }

};
```

```
//output example

push:0, value:2
height:2, left: left, area:2
push:1, value:1
push:2, value:5
push:3, value:6
height:6, left:5, area:6
height:5, left:1, area:10
push:4, value:2
push:5, value:3
height:3, left:2, area:3
height:2, left:1, area:8
height:1, left: left, area:6
push:6, value:0
10

```
