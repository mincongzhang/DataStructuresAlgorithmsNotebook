### Single Number
Given 2*n + 1 numbers, every numbers occurs twice except one, find it.
http://www.lintcode.com/en/problem/single-number/

```
class Solution {
public:
	/**
	 * @param A: Array of integers.
	 * return: The single number.
	 */
    int singleNumber(vector<int> &A) {
        if(A.empty()) return 0;
        
        int ret;
        for(vector<int>::const_iterator it = A.begin(); it != A.end(); ++it)
            ret ^= *it;
            
        return ret;
    }
};
```

```
#include <unordered_map>

class Solution {
public:
	/**
	 * @param A: Array of integers.
	 * return: The single number.
	 */
    int singleNumber(vector<int> &A) {
        if(A.empty()) return 0;
        
        std::unordered_map<int,int> hash;
        for(size_t i=0; i<A.size();++i){
            if(hash.find(A[i])==hash.end()){
                hash[A[i]] = 1;
            } else {
                hash[A[i]] = 2;
            }
        }
        
        int single = 0;
        for(std::unordered_map<int,int>::const_iterator it = hash.begin();
        it!=hash.end();++it){
            if(it->second == 1){
                single = it->first;
            }
        }
        
        return single;
    }
};
```

```
namespace{
    typedef unsigned int uint;
}

class Solution {
public:
	/**
	 * @param A: Array of integers.
	 * return: The single number.
	 */
    int singleNumber(vector<int> &A) {
        unordered_map<int,int> hash;
        for(uint i=0; i<A.size(); ++i){
            hash[A[i]]++;
        }
        
        for(unordered_map<int,int>::const_iterator it=hash.begin(); it!=hash.end();++it){
            if(it->second == 1) return it->first;
        }
        
        return 0;
    }
};

```