
### First Bad Version
he code base version is an integer start from 1 to n. One day, someone committed a bad version in the code case, so it caused this version and the following versions are all failed in the unit tests. Find the first bad version.

You can call isBadVersion to help you determine which version is the first bad one. The details interface can be found in the code's annotation part.
http://www.lintcode.com/en/problem/first-bad-version/

```
/**
 * class SVNRepo {
 *     public:
 *     static bool isBadVersion(int k);
 * }
 * you can use SVNRepo::isBadVersion(k) to judge whether 
 * the kth code version is bad or not.
*/
class Solution {
public:
    /**
     * @param n: An integers.
     * @return: An integer which is the first bad version.
     */
    int findFirstBadVersion(int n) {
        if(n == 0) return 0;
        
        int begin(0),end(n);
        //find first "true"
        while(begin+1 < end){
            int mid = (begin+end)/2;
            
            if(SVNRepo::isBadVersion(mid)){
                end = mid;
                continue;
            }
            
            //if mid is not bad
            begin = mid;
        }
        
        return end;
    }
};
```