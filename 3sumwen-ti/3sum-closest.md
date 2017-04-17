

### 3Sum Closest
Given an array S of n integers, find three integers in S such that the sum is closest to a given number, target. Return the sum of the three integers.
http://www.lintcode.com/en/problem/3sum-closest/

```
#include <algorithm>
#include <limits.h>
#include <vector>

class Solution {
private:
	int m_closest_sum;
	int m_target;

	void findCloest(const std::vector<int> & nums, int begin, int end){
		int first = nums[begin];
		begin++;

        if(begin == end) return;

		int closest_sum(INT_MAX);
		int min_diff(INT_MAX);
		while(begin < end){
			int cur_diff = std::abs(m_target - (first + nums[begin] + nums[end]));
			if(min_diff < cur_diff){
				break;
			} else {
				min_diff = cur_diff;
				closest_sum = first + nums[begin] + nums[end];
			}

			int left_diff = std::abs(m_target - (first + nums[begin+1] + nums[end]));
			int right_diff = std::abs(m_target - (first + nums[begin] + nums[end-1]));

			if(left_diff < right_diff){
				begin++;
			}	else {
				end--;
			}
		}

		if(std::abs(closest_sum - m_target)<std::abs(m_closest_sum - m_target)){
			m_closest_sum = closest_sum;
		}
	}

public:
	/**
	* @param numbers: Give an array numbers of n integer
	* @param target: An integer
	* @return: return the sum of the three integers, the sum closest target.
	*/
	int threeSumClosest(std::vector<int> nums, int target) {
		m_closest_sum = INT_MAX;
		if(nums.empty()) return m_closest_sum;

		m_target = target;
		std::sort(nums.begin(),nums.end());

		for(int begin(0),end(nums.size()-1); begin<nums.size(); ++begin){
			//if abs(diff) decrease, keep going
			//else break
			findCloest(nums,begin,end);
		}

		return 	m_closest_sum;
	}
};
```