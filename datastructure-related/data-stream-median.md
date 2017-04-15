
### Data Stream Median

Numbers keep coming, return the median of numbers at every time a new number added.

http://www.lintcode.com/en/problem/data-stream-median/

```
#include <functional>   // std::greater
#include <queue>
class Solution {
public:
  /**
   * @param nums: A list of integers.
   * @return: The median of numbers
   */
  vector<int> medianII(vector<int> &nums) {
    std::priority_queue<int,std::vector<int>, std::greater<int> > larger_half;   //pop: 5 6 7 8 9
    std::priority_queue<int,std::vector<int>, std::less<int> >    less_half;     //pop: 4 3 2 1 0

    vector<int> medians;
    for(vector<int>::const_iterator it=nums.begin(); it!=nums.end(); ++it){

      const int & num = *it;

      //push to queue
      if(less_half.empty() || num < less_half.top()){
        less_half.push(num);
      } else {
        larger_half.push(num);
      }

      //balance
      if(less_half.size() > larger_half.size()+1){
        larger_half.push(less_half.top());
        less_half.pop();
      }
      if(larger_half.size() > less_half.size()){
        less_half.push(larger_half.top());
        larger_half.pop();
      }


      //push to medians
      medians.push_back(less_half.top());
    }

    return medians;
  }
};

```
