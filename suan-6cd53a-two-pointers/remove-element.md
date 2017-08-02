### Remove Element

Given an array and a value, remove all instances of that value in place and return the new length.  
Do not allocate extra space for another array, you must do this in place with constant memory.  
The order of elements can be changed. It doesn't matter what you leave beyond the new length.

Example:  
Given input array nums = \[3,2,2,3\], val = 3  
Your function should return length = 2, with the first two elements of nums being 2.

[https://leetcode.com/problems/remove-element/\#/description](https://leetcode.com/problems/remove-element/#/description)

```
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        size_t eq_it = 0;
        int count = 0;
        for(int it=0; it<nums.size(); ++it){
            if(nums[it]!=val){
                nums[eq_it] = nums[it];
                eq_it++;
                count++;
            }
        }

        return count;
    }
};
```

总结:遇到这种需要in place移动的题,一般就是一个move\_it,加上for循环

```
for(it=0:size){
    if(should stay){
        swap(it,move_it);
        move_it++;
    }
}
```



