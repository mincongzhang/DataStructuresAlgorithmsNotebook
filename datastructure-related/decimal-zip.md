### Decimal Zip

### ![](/assets/decimal_zip.jpg)

```
// Example program
#include <iostream>
#include <string>
#include <stack>

const int MAX = 1000000000;
const int MAX_SIZE = 11;

void fillStack(std::stack<int> & s, int num){
    if(num == 0){
        s.push(0);
        return;
    }
    
    while(num>0){
        s.push(num%10);
        num/=10;
    }
}

void fillZip(std::stack<int> & s,int & zip){
    if(s.empty()) return;
    zip *= 10;
    zip += s.top();	
	s.pop();
}

int decimalZip(int A, int B){
	std::cout<<A<<","<<B<<" => ";
    if(A<0 || B<0) return -1;
    if(A>MAX || B>MAX) return -1;
  
    std::stack<int> A_s;
    std::stack<int> B_s;
    
    fillStack(A_s,A);
    fillStack(B_s,B);
    
    if(A_s.size()+B_s.size() > MAX_SIZE){
        return -1;    
    }
    
    //Zipping
    int zip(0);
    while(!A_s.empty() || !B_s.empty()){
        fillZip(A_s,zip);
        fillZip(B_s,zip);

        if(zip>MAX) {
			return -1;
		}
    }
    
    return zip;
}

int main(){
    std::cout<<decimalZip(12,56)<<std::endl;
	std::cout<<decimalZip(56,12)<<std::endl;
	std::cout<<decimalZip(12345,678)<<std::endl;
	std::cout<<decimalZip(123,67890)<<std::endl;
	std::cout<<decimalZip(1234,0)<<std::endl;
	std::cout<<decimalZip(0,12)<<std::endl;
	std::cout<<decimalZip(0,0)<<std::endl;
	std::cout<<decimalZip(3,-1)<<std::endl;
	std::cout<<decimalZip(1,MAX)<<std::endl;
	std::cout<<decimalZip(0,MAX)<<std::endl;
	std::cout<<decimalZip(MAX,0)<<std::endl;
	std::cout<<decimalZip(MAX,MAX)<<std::endl;
	system("pause");  
}

```



