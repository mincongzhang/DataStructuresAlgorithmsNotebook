
### Sqrt
Implement int sqrt(int x).
http://www.lintcode.com/en/problem/sqrtx/

```
//Newton's method

#include <iostream>
class Solution {
public:
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    int sqrt(int x) {
        if(x == 0) return 0;
        
        //sqrt(x)
        //https://en.wikipedia.org/wiki/Methods_of_computing_square_roots
        
        double s = double(x)/2.0;
        while(true){
            double s_n = 0.5*(s + x/s);
            if(int(s_n) == int(s)){ break; }
            
            s = s_n;
        }
        
        return int(s);
    }
};
```

```
#include <iostream>
class Solution {
public:
    /**
     * @param x: An integer
     * @return: The sqrt of x
     */
    int sqrt(int x) {
        if(x == 1) return x;

        long begin(0),end(x);
        while(begin+1 < end){
            long mid = (begin + end)/2;
            long square = mid*mid;
            
            if(square == x) return mid;
            if(square > x){
                end = mid;
                continue;
            } else {
                begin = mid;
                continue;
            }
        }
    
        return begin;
    }
};
```