### Decimal Zip

### ![](/assets/decimal_zip.jpg)


```
// Example program
#include <iostream>
#include <string>
#include <stack>

int decimalZip(int A, int B){
    std::stack<int> A_s;
    std::stack<int> B_s;
    
    while(A){
        A_s.push(A%10);
        A/=10;
    }
    
    while(B){
        B_s.push(B%10);
        B/=10;
    }
    
    
}

int main()
{
  std::string name;
  std::cout << "What is your name? ";
  getline (std::cin, name);
  std::cout << "Hello, " << name << "!\n";
}

```