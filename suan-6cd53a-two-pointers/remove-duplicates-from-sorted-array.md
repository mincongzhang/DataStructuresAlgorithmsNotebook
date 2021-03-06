### Remove Duplicates from Sorted Array

Given a sorted array, remove the duplicates in place such that each element appear only once and return the new length.  
Do not allocate extra space for another array, you must do this in place with constant memory.

For example,  
Given input array nums = \[1,1,2\],  
Your function should return length = 2, with the first two elements of nums being 1 and 2 respectively. It doesn't matter what you leave beyond the new length.

[https://leetcode.com/problems/remove-duplicates-from-sorted-array/](https://leetcode.com/problems/remove-duplicates-from-sorted-array/)



总结:遇到这种需要in place移动的题,一般就是一个move\_it,加上for循环

```
move_it=0;
for(it=0:size){
    if(num[it] should stay){
        swap(it,move_it);
        move_it++;
    }
}
```

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;

        //[1,1,1,2]
        int non_dup_it = 1;
        for(int i=1; i<nums.size(); ++i){
            if(nums[i]!=nums[i-1]){//should stay
                nums[non_dup_it] = nums[i];
                non_dup_it++;
            }
        }

        return non_dup_it;
    }
};
```

```
class Solution {
public:
    int removeDuplicates(vector<int>& nums) {
        if(nums.empty()) return 0;
        int non_dup_it=0;
        for(int it=0;it<nums.size();++it){
            if(nums[it]!=nums[non_dup_it]){
                non_dup_it++;
                nums[non_dup_it]=nums[it];
            }
        }

        return non_dup_it+1;
    }
};
```



