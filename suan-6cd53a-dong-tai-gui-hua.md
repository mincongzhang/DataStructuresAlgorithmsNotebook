### Pascal

```
/*

     1
    1 1
   1 2 1
  1 3 3 1
 1 4 6 4 1
1 5 10 10 5 1
*/

#include <iostream>
#include <vector>
#include <unordered_map>
#include <sstream>

namespace{
    template<typename T>
    std::string toString ( T num ){
        std::stringstream ss;
        ss << num;
        return ss.str();
    }
    
    std::string getHashKey(const int n1, const int n2){
        //TODO: can still optimise here, think about symetric key, should have the same value
        return toString(n1)+","+toString(n2);
    }
}

int computePascal( int row, int col )
{
    if(row<0 || !(col >=0 && col<=row)){
        return 0;
    }
    
    if(row == 0) return 1;
    if(col == 0 || col == row) return 1;
    
    return computePascal(row-1,col) + computePascal(row-1,col-1);
}

int computePascalHash( int row, int col, std::unordered_map<std::string,int> & hash){
    if(row<0 || !(col >=0 && col<=row)){
        return 0;
    }
    
    if(row == 0) return 1;
    if(col == 0 || col == row) return 1;
    
    int result(0);
    
    std::string hash_key = getHashKey(row,col);
    std::unordered_map<std::string,int>::const_iterator it = hash.find(hash_key);
    if(it!=hash.end()){
        result = it->second;
    } else {
        result = computePascal(row-1,col) + computePascal(row-1,col-1);
        hash[hash_key]=result;
    }
    
    return result;
}

int main(){
    std::cout<<computePascal(60,3)<<std::endl;
    
    std::unordered_map<std::string,int> hash;
    std::cout<<computePascalHash(60,3,hash)<<std::endl;
    return 0;
}
```