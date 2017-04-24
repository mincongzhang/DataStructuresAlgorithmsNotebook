### Fibonacci

```
//Fibonacci: 1,1,2,3,5,8,13,21,34,55,89,144

#include <iostream>
#include <unordered_map>
#include <sstream>

int fib(int n){
    if(n==1 || n==2) return 1;

    return fib(n-1)+fib(n-2);
}

int fib2(int n){
    int cur = 1;
    int next = 1;
    int prev;
    while(--n > 0){
        prev = cur;
        cur = next;
        next = prev+cur;
    }

    return cur;
}

int fibHash(int n, std::unordered_map<int,int> & hash){
    if(n==1 || n==2) return 1;

    int result;

    std::unordered_map<int,int>::const_iterator it = hash.find(n);
    if(it != hash.end()){
        result = it->second;    
    } else {
        result = fib(n-1)+fib(n-2);
        hash[n] = result;
    }

    return result;
}

int main(){
    std::cout<<fib(10)<<std::endl;
    std::cout<<fib2(10)<<std::endl;

    std::unordered_map<int,int> hash;
    std::cout<<fibHash(10,hash)<<std::endl;
    std::cout<<fibHash(11,hash)<<std::endl;

    return 0;
}
```



