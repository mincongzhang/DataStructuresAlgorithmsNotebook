### 2 Sum Duplicates
Given an array of integers, find two numbers such that they add up to a specific target number.

Allow duplicates, position sensitive.

```
#include <unordered_map>
#include <stdio.h>
#include <iostream>
#include <cassert>

using namespace std;

int NumPairs( int x[], int size, int K)
{
	int count = 0;
	if(size<2) return count;
	unordered_map<int,int> hash;
	for(int i=0; i<size; ++i){
		hash[x[i]]++;
	}
	int same_count = 0;
	for(int i=0; i<size; ++i){
		if(K-x[i] == x[i] && hash[K-x[i]]>0){
			same_count+=hash[x[i]]-1;
			continue;
		}
		if(hash[K-x[i]]>0){
			count++;
		}
	}
	count+=same_count;
	count/=2;
	cout<<count<<endl;
	return count;
};

int main()
{
	int a[0] = {};
	int b[3] = {-1,1,0};
	int c[2] = {0,0};
	int d[3] = {1,1,1}; //pairs = 3
	int e[4] = {0,0,0,0}; //pairs = 6
	int f[5] = {0,0,1,-1,0};

	assert(NumPairs( a, sizeof(a)/sizeof(int), 10 ) == 0);
	assert(NumPairs( b, sizeof(b)/sizeof(int), 0 ) == 1);
	assert(NumPairs( c, sizeof(c)/sizeof(int), 0 ) == 1);
	assert(NumPairs( d, sizeof(d)/sizeof(int), 2 ) == 3);
	assert(NumPairs( e, sizeof(e)/sizeof(int), 0 ) == 6);
	assert(NumPairs( f, sizeof(f)/sizeof(int), 0 ) == 4);
}
```

