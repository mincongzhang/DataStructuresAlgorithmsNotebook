### Move Zeroes

Given an array nums, write a function to move all 0's to the end of it while maintaining the relative order of the non-zero elements.

For example, given nums = \[0, 1, 0, 3, 12\], after calling your function, nums should be \[1, 3, 12, 0, 0\].

[https://leetcode.com/problems/move-zeroes/](https://leetcode.com/problems/move-zeroes/)

```
class Solution {
private:
    void swap(int & a, int & b){
        int tmp = a;
        a = b;
        b = tmp;
    }

public:
    void moveZeroes(vector<int>& nums) {
        if(nums.empty()) return;

        int zero_it = -1;
        for(int it=0; it<nums.size(); ++it){
            if(nums[it]==0 && zero_it ==-1){
                zero_it = it;
                continue;
            }

            if(nums[it]!=0 && zero_it != -1){
                swap(nums[it],nums[zero_it]);
                zero_it++;
            }
        }

    }
};
```

```
class Solution {
private:
    void swap(int & a, int & b){
        int tmp = a;
        a = b;
        b = tmp;
    }
    
public:
    void moveZeroes(vector<int>& nums) {
        if(nums.empty()) return;
        
        int non_zero_it = 0;
        for(int it=0; it<nums.size(); ++it){
            if(nums[it]!=0){
                swap(nums[non_zero_it],nums[it]);
                non_zero_it++;
            }
        }
        
    }
};
```

