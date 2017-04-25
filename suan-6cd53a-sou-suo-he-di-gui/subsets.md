
### Subsets

Given a set of distinct integers, return all possible subsets.  
http://www.lintcode.com/en/problem/subsets/

```
class Solution {
public:
  /**
   * @param S: A set of numbers.
   * @return: A list of lists. All valid subsets.
   */
  vector<vector<int> > subsets(vector<int> &nums) {
    size_t subset_size = 1 << nums.size();

    vector<vector<int> > result;
    for(size_t i=0; i<subset_size; ++i){
      vector<int> subset;
      size_t cur_i = i;
      size_t idx = 0;
      while(cur_i){
        if(cur_i & 1) subset.push_back(nums[idx]);
        cur_i >>= 1;
        idx++;
      }
      result.push_back(subset);
    }

    return result;
  }
};
```

```

class Solution {
public:
  /**
   * @param S: A set of numbers.
   * @return: A list of lists. All valid subsets.
   */

  /*
    The set of subsets of {1} is {{}, {1}}
    For {1, 2}, take {{}, {1}}, add 2 to each subset to get {{2}, {1, 2}} and take the union with {{}, {1}} to get {{}, {1}, {2}, {1, 2}}
    Repeat till you reach n
  */

  vector<vector<int> > subsets(vector<int> &nums) {
    //Or initialize with 1 empty element
    //vector<vector<int> > result(1);
    vector<vector<int> > result(1);
    for(size_t i=0; i<nums.size(); ++i){
      size_t result_size = result.size();
      for(size_t s=0; s<result_size; ++s){
        result.push_back(result[s]);
        result.back().push_back(nums[i]);
      }
    }

    return result;
  }
};
```