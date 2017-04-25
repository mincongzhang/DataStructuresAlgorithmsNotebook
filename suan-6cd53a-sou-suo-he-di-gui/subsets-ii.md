### Subsets II

Given a list of numbers that may has duplicate numbers, return all possible subsets:  
Each element in a subset must be in non-descending order.  
The ordering between two subsets is free.  
The solution set must not contain duplicate subsets.

[http://www.lintcode.com/en/problem/subsets-ii/](http://www.lintcode.com/en/problem/subsets-ii/)

```
#include <algorithm>
class Solution {
public:
  /**
   * @param S: A set of numbers.
   * @return: A list of lists. All valid subsets.
   */
  vector<vector<int>> subsetsWithDup(const vector<int> &S) {
    vector<int> sorted_s = S;
    std::sort(sorted_s.begin(),sorted_s.end());

    vector< vector<int> > result;
    result.push_back(vector<int>());

    size_t previous_size=0;
    for(size_t i=0; i<sorted_s.size(); ++i){
      size_t size = result.size();
      for(size_t s=0; s<size; ++s){

        //NOTE: not the first element && duplicates,
        //so avoid previous created elements, only add when s >= previous_size
        if(i>0 && sorted_s[i-1]==sorted_s[i] && s < previous_size ){
          continue;
        }

        result.push_back(result[s]);
        result.back().push_back(sorted_s[i]);
      }
      previous_size = size;
    }

    return result;
  }
};

/*
S = [1,2,2]
//result
[]

[1]

[2]
[1][2]

//NOTE: not the first element && duplicates,
//so avoid previous created elements, only add when s >= previous_size
//if(i>0 && sorted_s[i-1]==sorted_s[i] && s < previous_size )
//[2]
//[1][2]
[2][2]
[1][2][2]
*/
```



