###  Kth Largest Element in an Array

Find the kth largest element in an unsorted array. Note that it is the kth largest element in the sorted order, not the kth distinct element.

For example,
Given [3,2,1,5,6,4] and k = 2, return 5.

```
class Solution {
public:
    int findKthLargest(vector<int>& nums, int k) {
        std::priority_queue<int> q;
        for(size_t i=0; i<nums.size(); ++i){
            q.push(nums[i]);
        }
        
        while(--k){
            q.pop();
        }
        
        return q.top();
    }
};
```