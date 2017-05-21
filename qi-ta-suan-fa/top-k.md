### Top K

topK问题在海量数据处理中也是一个非常经典的问题，所以还是要重视。

#### Median of 2 sorted array/list (Top K in list)


There are two sorted arrays nums1 and nums2 of size m and n respectively.

Find the median of the two sorted arrays. The overall run time complexity should be O(log (m+n)).

https://leetcode.com/problems/median-of-two-sorted-arrays/#/description

```
class Solution {
public:
  double findMedianSortedArrays(vector<int>& nums1, vector<int>& nums2) {
    int n1_size = nums1.size(), n2_size = nums2.size();
    int k = (n1_size + n2_size) / 2;
    int median1 = findKth(nums1, 0, n1_size, nums2, 0, n2_size, k + 1);
    if ((n2_size + n1_size) % 2 == 0)
      {
        int median2 = findKth(nums1, 0, n1_size, nums2, 0, n2_size, k);
        return (median1 + median2) / 2.0;
      }
    else return median1;
  }

  int findKth(vector<int> & n1, int n1_left, int n1_right, vector<int> & n2, int n2_left, int n2_right, int k){
    int n1_size = n1_right - n1_left;
    int n2_size = n2_right - n2_left;

    //n2 size always larger than n1
    if (n1_size > n2_size) {
      return findKth(n2, n2_left, n2_right, n1, n1_left, n1_right, k);
    }

    if (n1_size == 0){
      return n2[n2_left + k - 1];
    }
    if (k == 1){
      return min(n1[n1_left], n2[n2_left]);
    }

    int n1_left_count = min (k/2, n1_size);
    int n2_left_count = k - n1_left_count;

    int n1_offset = n1_left + n1_left_count;
    int n2_offset = n2_left + n2_left_count;

    if (n1[n1_offset - 1] == n2[n2_offset - 1]) {
      return n1[n1_offset - 1];
    }

    if (n1[n1_offset - 1] < n2[n2_offset - 1]) {
      return findKth(n1, n1_offset, n1_right, n2, n2_left, n2_right, k - n1_left_count);
    } else {
      return findKth(n1, n1_left, n1_right, n2, n2_offset, n2_right, k - n2_left_count);
    }
    
    /*
    // Could also ignore elements from the left, but actually not necessary
    if (n1[n1_offset - 1] < n2[n2_offset - 1]) {
        return findKth(n1, n1_offset, n1_right, n2, n2_left, n2_offset, k - n1_left_count);
    } else {
        return findKth(n1, n1_left, n1_offset, n2, n2_offset, n2_right, k - n2_left_count);
    }
    */
  }
};
```