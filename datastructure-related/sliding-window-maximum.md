### Sliding Window Maximum

Given an array nums, there is a sliding window of size k which is moving from the very left of the array to the very right. You can only see the k numbers in the window. Each time the sliding window moves right by one position.

For example,  
Given nums = \[1,3,-1,-3,5,3,6,7\], and k = 3.

```
Window position                Max
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

Therefore, return the max sliding window as \[3,3,5,5,6,7\].

[https://leetcode.com/problems/sliding-window-maximum/\#/description](https://leetcode.com/problems/sliding-window-maximum/#/description)

[https://discuss.leetcode.com/topic/35823/recommend-for-beginners-clean-c-implementation-with-detailed-explanation/2](https://discuss.leetcode.com/topic/35823/recommend-for-beginners-clean-c-implementation-with-detailed-explanation/2)

https://discuss.leetcode.com/topic/19099/3-c-solutions

```
//Multiset, O(nlog(k))

#include <set>

class Solution {
public:
  vector<int> maxSlidingWindow(vector<int>& nums, int k) {
    vector<int> result;
    if(nums.size()<k) return result;
    if(k<=0) return result;

    std::multiset<int> mset;
    unsigned int begin = 0;
    unsigned int end = k;

    for(unsigned int i=begin; i<end; ++i){
      mset.insert(nums[i]);
    }
    result.push_back(*mset.rbegin());
    begin++;
    end++;

    while(end<=nums.size()){
      //NOTE:This would erase multiple elems
      //mset.erase(nums[begin-1]);

      mset.erase(mset.find(nums[begin-1]));
      mset.insert(nums[end-1]);

      result.push_back(*mset.rbegin());
      begin++;
      end++;
    }

    return result;
  }
};
```

```
//priority_queue nlog(n)

#include <queue>
#include <utility>

class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> result;
        if(k>nums.size()||nums.empty()) return result; 
        
        int begin(0), end(k-1);
        std::priority_queue<std::pair<int,int> > p_q;
        for(int i=begin;i<=end;++i){
            p_q.push(std::pair<int,int>(nums[i],i));
        }
        result.push_back(p_q.top().first);
        begin++;
        end++;
        
        while(end<nums.size()){
            p_q.push(std::pair<int,int>(nums[end],end));
            
            while(p_q.top().second<begin || p_q.top().second>end){
                p_q.pop();
            }
            
            result.push_back(p_q.top().first);
            begin++;
            end++;
        }
        
        return result;
    }
};
```

```
class Solution {
public:
    vector<int> maxSlidingWindow(vector<int>& nums, int k) {
        vector<int> result;
        deque<int> q;
        for(int i=0; i<nums.size(); i++){
            std::cout<<"i:"<<i<<std::endl;
            /*** remove the front element **/
            if(!q.empty() && q.front()==i-k){
                //std::cout<<"remove front:"<<q.front()<<std::endl;
                q.pop_front();
            }

            /*** keep the element in the queue is monotically-decreasing ***/
            while(!q.empty() && nums[q.back()] < nums[i]){
                //std::cout<<"nums[q.back()]:"<<nums[q.back()]<<",nums[i]:"<<nums[i]<<",remove back:"<<q.back()<<std::endl;
                q.pop_back();
            }
            q.push_back(i);

            if(i>=k-1){
                result.push_back(nums[q.front()]);
            }
        }
        return result;
    }
};
```



