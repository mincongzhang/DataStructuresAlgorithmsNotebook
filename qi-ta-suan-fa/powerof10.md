### Power of 10

```
#include <unordered_map>
#include <stdio.h>
#include <iostream>
#include <cassert>

using namespace std;

bool powOf10(int x){
	if(x<=0)  return false;
	if(x==1 || x==10)  return true;
	if(x<10) return false;

	int base = 10;
	while(base<x){
		base*=10;
		if(base==x){
			return true;
		}
	}

	return false;
}

int main()
{
	assert(powOf10(10)==true);
	assert(powOf10(1)==true);
	assert(powOf10(100)==true);
	assert(powOf10(4)==false);
	assert(powOf10(-1)==false);
	assert(powOf10(0)==false);
	system("pause");
}
```