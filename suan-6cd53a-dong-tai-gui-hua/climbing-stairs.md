

### Climbing Stairs
You are climbing a stair case. It takes n steps to reach to the top.
Each time you can either climb 1 or 2 steps. In how many distinct ways can you climb to the top?  
http://www.lintcode.com/en/problem/climbing-stairs/  

```
class Solution {
public:
  /**
   * @param n: An integer
   * @return: An integer
   */
  int climbStairs(int n) {
    if(n<=0) return 0;
    if(n==1) return 1;
    if(n==2) return 2;
    return climbStairs(n-1) + climbStairs(n-2);
  }
};
```

```
class Solution {
public:
  /**
   * @param n: An integer
   * @return: An integer
   */
  int climbStairs(int n) {
    if(n<=0) return 1;
    if(n==1) return 1;
    if(n==2) return 2;

    std::vector<int> stairs(n,0);
    stairs[0] = 1;
    stairs[1] = 2;
    for(size_t i=2; i<n; ++i){
      stairs[i] = stairs[i-1] + stairs[i-2];
    }

    return stairs.back();
  }
};
```
