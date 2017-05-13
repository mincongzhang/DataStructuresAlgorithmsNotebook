### Power of Four

Given an integer (signed 32 bits), write a function to check whether it is a power of 4.

https://leetcode.com/company/two-sigma/

```
class Solution {
public:
    bool isPowerOfFour(int num) {
        if(num<=0) return false;
        if(num==1) return true;
        
        int n = 4;
        while(true){
            if(num==n) return true;
            n*=4;
            
            if(n>num || n==INT_MAX || n<0) break;
        }
        
        return false;
    }
};
```

```
class Solution {
public:
    bool isPowerOfFour(int num) {
        if(num<=0) return false;
        
        if(num & num-1) return false;
        
        if(num & 0x55555555) return true; //101010101 ...
        
        return false;
    }
};
```

```
class Solution {
public:
    bool isPowerOfFour(int num) {
        return (num>0 && (num & (num-1))==0 && num & 0x55555555);
    }
};
```