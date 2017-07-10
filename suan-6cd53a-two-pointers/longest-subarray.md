### Longest Subarray



![](/assets/longest_subarray.png)

examples

```
Input 
[1 2 3 4] k = 3
Output 
2

Input
[3 1 2 1 4] k = 4
Output
3
```

```
#include <string>
#include <iostream>
#include <vector>

void updateEndSum(const std::vector<int> & a, int k, int & end, int & sum){
  while(end<a.size()){
    int cur_sum = sum+a[end];
    if(cur_sum>k){
      end--;
      break;
    }
    sum = cur_sum;
    end++;
  }
}


int maxLength(std::vector<int> a, int k) {
  //get init begin
  int  begin(0);
  while(begin<a.size() && a[begin]>k){
    begin++;
  }

  //get init end
  int sum=a[begin];
  int end = begin+1;
  updateEndSum(a,k,end,sum);

  int max_len = end-begin+1;
  //search for max len
  while(begin<a.size() && end<a.size()){
    sum-=a[begin];
    begin++;
    end++;
    updateEndSum(a,k,end,sum);
    max_len = std::max(max_len,end-begin+1);
  }

  return max_len;
}
int main(){
  std::cout<<"max len"<<std::endl;
  std::vector<int> v1;
  v1.push_back(3);
  v1.push_back(1);
  v1.push_back(2);
  v1.push_back(1);
  v1.push_back(4);
  std::cout<<maxLength(v1,4)<<std::endl;

  return 0;
}
```