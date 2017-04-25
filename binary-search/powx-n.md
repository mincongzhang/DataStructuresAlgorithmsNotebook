### Pow

Implement pow(x, n).

```
class Solution {
private:
    double positive_pow(double x, int n){
        if(n==0){
            return 1;
        }
        
        double result = positive_pow(x,n/2);
        if(n%2==0){
            return result*result;
        } else {
            return result*result*x;
        }
    }
public:
    double myPow(double x, int n) {
        if(n>0){
            return positive_pow(x,n);
        } 
        
        return 1.0/positive_pow(x,-n);
    }
};
```
