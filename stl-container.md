### deque

实现原理: 分段数组, 首地址存于索引数组\(index array\)

优点:
\(1\)随机访问\(call by rank\), 效率略逊于vector
\(2\)deque两端, 高效插入,删除
\(3\)内存分配优于vector, 不需要复制所有元素

缺点:
\(1\)中间插入, 越往中间效率越低\(需要移动的元素越多\)

```
#include <iostream>
#include <deque>

using namespace std;

int main()
{
   std::deque<int> dq;
   for(int i=0; i<5; ++i){
       dq.push_back(i);
   }

   for(int i=0; i<5; ++i){
       dq.push_front(i);
   }

   std::cout<<"Inerating"<<std::endl;
   for(size_t i=0; i<dq.size(); ++i){
       std::cout<<dq.at(i)<<std::endl;
   }

   dq.pop_back();
   dq.pop_front();

      std::cout<<"Inerating after pop back&front"<<std::endl;
   for(size_t i=0; i<dq.size(); ++i){
       std::cout<<dq.at(i)<<std::endl;
   }

   std::cout<<"Check front back"<<std::endl;
   std::cout<<dq.front()<<std::endl;
   std::cout<<dq.back()<<std::endl;
}
```

list

slist

stack

queue

priority\_queue

