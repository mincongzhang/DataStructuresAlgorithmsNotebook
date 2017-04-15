
### First Position of Target
For a given sorted array (ascending order) and a target number, find the first index of this number in O(log n) time complexity.
If the target number does not exist in the array, return -1.
http://www.lintcode.com/en/problem/first-position-of-target/

```
class Solution {
private:
  int m_pos;
  void search(const vector<int> & array,const int & target, int start, int end){
    if(start > end) return;
    int half = (start+end)/2;

    if(start == end ){
      if(array[half] == target)
        m_pos = half;
      
      return;
    }

    if(array[half] >= target){
      search(array,target,0,half);
    } else {
      search(array,target,half+1,end);
    }
  }

public:
  typedef unsigned int uint;
  int binarySearch(vector<int> &array, int target) {
    m_pos = -1;
    search(array,target,0,array.size()-1);

    return m_pos;
  }
};

```