### Single Element in a Sorted Array

Given a sorted array consisting of only integers where every element appears twice except for one element which appears once. Find this single element that appears only once.

Example 1:
Input: [1,1,2,3,3,4,4,8,8]
Output: 2
Example 2:
Input: [3,3,7,7,10,11,11]
Output: 10

https://leetcode.com/problems/single-element-in-a-sorted-array/#/description


```
class Solution {
public:
    int singleNonDuplicate(vector<int>& nums) {
        if(nums.size()%2==0) return -1;
        
        unsigned int begin = 0;
        unsigned int end   = nums.size()-1;
        bool even_pair = false;
        if((end/2)%2==0){
            even_pair = true;
        }

        while(begin<end){
            unsigned int mid = begin + (end-begin)/2;

            if(end-begin==2) {
                if(nums[mid] == nums[mid-1]){
                    return nums[end];
                } else {
                    return nums[begin];
                }
            }
            
            if(end-begin<2){
                return 4325;//NOTE: not sure why it doesn't work here
            }


            if(nums[mid] == nums[mid-1]){
                if(even_pair) {  
                    end = mid;   // 0 1 1 2 2
                } else {         
                    begin = mid+1; 
                }
            } else {
                if(even_pair){   
                    begin = mid; // 0 0 1 1 2
                } else {
                    end = mid-1;
                }
            }
        }
    
        return -1;  
    }
};
```