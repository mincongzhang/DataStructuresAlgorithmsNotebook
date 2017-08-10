

### Merge Intervals

Given a collection of intervals, merge all overlapping intervals.

For example,
Given [1,3],[2,6],[8,10],[15,18],
return [1,6],[8,10],[15,18].

https://discuss.leetcode.com/topic/18423/share-my-c-o-nlogn-solution

```
/**
 * Definition for an interval.
 * struct Interval {
 *     int start;
 *     int end;
 *     Interval() : start(0), end(0) {}
 *     Interval(int s, int e) : start(s), end(e) {}
 * };
 */

bool operator<(const Interval & a, const Interval & b){
    return a.start<b.start || (a.start==b.start && a.end<b.end);
}

class Solution {
public:
    vector<Interval> merge(vector<Interval>& intervals) {
        vector<Interval> result;
        if(intervals.empty()) return result;

        std::sort(intervals.begin(),intervals.end());
        result.push_back(intervals.front());
        
        for(int i=1; i<intervals.size(); ++i){
            if(result.back().end >= intervals[i].start){
                result.back().end = std::max(result.back().end,intervals[i].end);
            } else {
                result.push_back(intervals[i]);
            }
        }
        
        return result;
    }
};
```