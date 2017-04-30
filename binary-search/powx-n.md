### Pow

Implement pow(x, n).

```
class Solution {
private:
    double positivePow(double x, int n){
        if(n==0){
            return 1;
        }
        
        double result = positivePow(x,n/2);
        if(n%2==0){
            return result*result;
        } else {
            return result*result*x;
        }
    }
public:
    double myPow(double x, int n) {
        if(n>0){
            return positivePow(x,n);
        } 
        
        return 1.0/positivePow(x,-n);
    }
};
```
