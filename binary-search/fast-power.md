###  Fast Power
Calculate the a^n % b where a, b and n are all 32bit integers.
http://www.lintcode.com/en/problem/fast-power/

```
class Solution {
public:
    /*
     * @param a, b, n: 32bit integers
     * @return: An integer
     */
    int fastPower(int a, int b, int n) {
        if (n == 0) {
            return 1 % b;
        }
        if (n == 1) {
            return a % b;
        }
        long long temp = fastPower(a, b, n / 2);
        if (n % 2 == 0) {
            return (temp * temp) % b;
        } else {
            //(a * b) % p = (a % p * b % p) % p 
            return ((temp * temp) % b * a % b) % b;
        }
    }
};
```

