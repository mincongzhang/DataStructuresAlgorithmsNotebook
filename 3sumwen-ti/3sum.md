### 3 Sum

Given an array S of n integers, are there elements a, b, c in S such that a + b + c = 0? Find all unique triplets in the array which gives the sum of zero.  
_Elements in a triplet \(a,b,c\) must be in non-descending order. \(ie, a ≤ b ≤ c\)  
_The solution set must not contain duplicate triplets.  
[http://www.lintcode.com/en/problem/3sum/](http://www.lintcode.com/en/problem/3sum/)

```
//Sorting: O(n^2 * logn) solution
//TODO: Hashing can be O(n^2)

#include <algorithm>
#include <iostream>

class Solution {
private:
vector<vector<int> > m_triplets;

void findTwoSum(const vector<int> & nums,int begin, int end){
    if(begin == end){ return; }

    //find 2 sum = -first;
    int sum = -nums[begin];

    //NOTE: avoid duplication
    if(!m_triplets.empty() && nums[begin] == m_triplets.back()[0]){
        return;
    }

    begin++; //NOTE: avoid duplicated. e.g. nums[begin] = nums[second] = 0.


    while(begin < end){ 
        if(nums[begin]+nums[end] > sum){
            end--;
            continue;
        }

        if(nums[begin]+nums[end] < sum){
            begin++;
            continue;
        }

        //NOTE: avoid duplication
        if(!m_triplets.empty() && -sum == m_triplets.back()[0] && 
        nums[begin] == m_triplets.back()[1] && nums[end] == m_triplets.back()[2]){
            end--; //NOTE: may have multiple triplet
            continue;
        }

        //NOTE: 3 nums have already been non-descending
        vector<int> triplet;
        triplet.push_back(-sum);
        triplet.push_back(nums[begin]);
        triplet.push_back(nums[end]);

        m_triplets.push_back(triplet);
        //break;  NOTE: may have multiple triplet
        end--;
    }
}

public:    
    /**
     * @param numbers : Give an array numbers of n integer
     * @return : Find all unique triplets in the array which gives the sum of zero.
     */
    vector<vector<int> > threeSum(vector<int> &nums) {
        if(nums.empty()) { return m_triplets; }

        std::sort(nums.begin(),nums.end());

        for(int begin(0), end(nums.size()-1); begin < nums.size(); ++begin){
            findTwoSum(nums,begin,end);
        }

        return m_triplets;
    }
};
```



