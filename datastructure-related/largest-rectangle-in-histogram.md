
### Largest Rectangle in Histogram

Given n non-negative integers representing the histogram's bar height where the width of each bar is 1, find the area of largest rectangle in the histogram.

http://www.lintcode.com/en/problem/largest-rectangle-in-histogram/

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