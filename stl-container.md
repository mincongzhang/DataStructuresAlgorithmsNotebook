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

### list

双向列表

```
#include <iostream>
#include <list>

int main(){
  std::list<int> int_list;
  int_list.push_back(1);
  int_list.push_back(1);
  int_list.push_back(8);
  int_list.push_back(9);
  int_list.push_back(8);
  int_list.push_back(2);
  int_list.push_back(3);
  int_list.push_back(3);
  int_list.sort();
  int_list.unique();

  std::cout<<"sort, uniq and iterate:"<<std::endl;
  for(std::list<int>::iterator list_iter = int_list.begin();
      list_iter != int_list.end(); list_iter++) {
    std::cout<<*list_iter<<std::endl;
  }

  std::cout<<"front and back"<<std::endl;
  std::cout<<int_list.front()<<" "<<int_list.back()<<std::endl;

  std::cout<<"pop front and back"<<std::endl;
  int_list.pop_front();
  int_list.pop_back();
  for(std::list<int>::iterator list_iter = int_list.begin();
      list_iter != int_list.end(); list_iter++) {
    std::cout<<*list_iter<<std::endl;
  }
}
```

```
//List erase
#include <iostream>
#include <list>

int main()
{
    std::list<int> l;
    l.push_back(2);
    std::list<int>::iterator it = l.begin();
    if(it!=l.end()){
        std::cout<<*it<<std::endl;    
    }
    
    it=l.erase(it);
    if(it!=l.end()){
        std::cout<<"still exist"<<*it<<std::endl;    
    } else {
        std::cout<<"empty"<<std::endl; 
        
        if(l.begin()==l.end()){
            std::cout<<"begin==end now"<<std::endl; 
        }
    }
}
```

### forward\_list \(C++11\)

单向链表

```
#include <iostream>
#include <forward_list>

int main(){
  std::forward_list<int> int_list;
  int_list.push_front(1);
  int_list.push_front(1);
  int_list.push_front(8);
  int_list.push_front(9);
  int_list.push_front(8);
  int_list.push_front(2);
  int_list.push_front(3);
  int_list.push_front(3);
  int_list.sort();

  std::cout<<"iterate:"<<std::endl;
  for(std::forward_list<int>::iterator list_iter = int_list.begin();
      list_iter != int_list.end(); list_iter++) {
    std::cout<<*list_iter<<std::endl;
  }    

  int_list.unique();

  std::cout<<"sort, uniq and iterate:"<<std::endl;
  for(std::forward_list<int>::iterator list_iter = int_list.begin();
      list_iter != int_list.end(); list_iter++) {
    std::cout<<*list_iter<<std::endl;
  }

  std::cout<<"front"<<std::endl;
  std::cout<<int_list.front()<<std::endl;

  std::cout<<"pop front"<<std::endl;
  int_list.pop_front();
  for(std::forward_list<int>::iterator list_iter = int_list.begin();
      list_iter != int_list.end(); list_iter++) {
    std::cout<<*list_iter<<std::endl;
  }
}
```

### queue

默认使用双端队列deque的数据结构

```
#include <iostream>
#include <queue>

int main(){
  std::queue<int> int_q;
  int_q.push(1);
  int_q.push(5);
  int_q.push(8);
  int_q.push(9);

  std::cout<<"queue front:"<<int_q.front()<<std::endl;
  std::cout<<"queue back:"<<int_q.back()<<std::endl;

  int_q.pop();
  std::cout<<"queue front after pop:"<<int_q.front()<<std::endl;
  std::cout<<"queue back after pop:"<<int_q.back()<<std::endl;
}
```

### priority\_queue

底层默认采用vector向量容器

```
#include <iostream>
#include <vector>
#include <queue>
#include <functional>//greater

int main(){
  std::priority_queue<int> q;
  q.push(1);
  q.push(5);
  q.push(10);
  q.push(8);
  q.push(9);

  std::cout<<"priority_queue top:"<<q.top()<<std::endl;
  q.pop();
  std::cout<<"priority_queue top after pop:"<<q.top()<<std::endl;

  std::cout<<std::endl;

  std::priority_queue<int,std::vector<int>,std::greater<int> > q2;
  q2.push(1);
  q2.push(5);
  q2.push(10);
  q2.push(8);
  q2.push(9);

  std::cout<<"priority_queue top:"<<q2.top()<<std::endl;
  q2.pop();
  std::cout<<"priority_queue top after pop:"<<q2.top()<<std::endl;

}
```

### stack

默认使用双端队列deque的数据结构，当然也可以采用其他线性表（如vector或list），只要提供堆栈的入栈、出栈、栈顶元素访问和判断是否为空的操作即可。

```
#include <iostream>
#include <stack>

int main(){
  std::stack<int> int_stack;
  int_stack.push(1);
  int_stack.push(5);
  int_stack.push(8);
  int_stack.push(9);

  std::cout<<"stack top:"<<int_stack.top()<<std::endl;
  int_stack.pop();
  std::cout<<"stack top:"<<int_stack.top()<<std::endl;
}
```



