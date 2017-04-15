### Longest Consecutive Sequence

Given an unsorted array of integers, find the length of the longest consecutive elements sequence.

Given \[100, 4, 200, 1, 3, 2\],  
The longest consecutive elements sequence is \[1, 2, 3, 4\]. Return its length: 4.

http://www.lintcode.com/en/problem/longest-consecutive-sequence/

```
//O(nlogn) solution (std::set)
#include <algorithm>

class Solution {
public:
  /**
   * @param nums: A list of integers
   * @return an integer
   */
  int longestConsecutive(vector<int> &num) {
    if(num.empty()) return 0;

    std::set<int> sorted_num;
    for(std::vector<int>::const_iterator it=num.begin(); it!=num.end(); ++it){
      sorted_num.insert(*it);
    }

    std::set<int>::const_iterator it=sorted_num.begin();
    int prev = *it;
    it++;

    int count = 1;
    int max_count = 1;
    while(it!=sorted_num.end()){
      if(abs(*it - prev)==1){
        count++;
        max_count = std::max(count,max_count);
      } else {
        count = 1;
      }

      prev=*it;
      ++it;
    }

    return max_count;
  }

};
```

```
//O(nlogn) solution (sort)

#include <algorithm>

class Solution {
public:
  /**
   * @param nums: A list of integers
   * @return an integer
   */
  int longestConsecutive(vector<int> &num) {
    if(num.empty()) return 0;

    std::sort(num.begin(),num.end());
    int count = 1;
    int max_count = 1;
    for(size_t i=1; i< num.size(); ++i){
      int diff = abs(num[i] - num[i-1]);

      //1. diff == 1, Consecutive
      if(diff==1){
        count++;
        max_count = std::max(count,max_count);
        continue;
      }

      //2. diff > 1, not Consecutive
      //reset if diff >1
      if(diff>1){
        count = 1;
      }

      //3. diff == 0, no count
      //when diff == 0, continue
    }

    return max_count;
  }
};
```

```
//O(n) solution: hashtable

#include <unordered_map>

class Solution {
public:
    /**
     * @param nums: A list of integers
     * @return an integer
     */
    int longestConsecutive(vector<int> &num) {
        std::unordered_set<int> hash;
        for(size_t i=0;i<num.size();++i){
            hash.insert(num[i]);
        }

        int length = INT_MIN;
        while(!hash.empty()){
            int cur = *hash.begin();
            int count = 1;

            hash.erase(cur);
            int next = cur+1;

            std::unordered_set<int>::const_iterator it=hash.find(next);
            while(it!=hash.end()){
                hash.erase(next);
                count++;
                next++;
                it=hash.find(next);
            }

            int prev = cur-1;
            it = hash.find(prev);
            while(it!=hash.end()){
                hash.erase(prev);
                count++;
                prev--;
                it=hash.find(prev);
            }

            length = std::max(count,length);
        }

        return length;
    }
};
```



