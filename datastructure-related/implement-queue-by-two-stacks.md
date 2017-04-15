
### Implement Queue by Two Stacks

The queue should support push(element), pop() and top() where pop is pop the first(a.k.a front) element in the queue.
Both pop and top methods should return the value of first element.

http://www.lintcode.com/en/problem/implement-queue-by-two-stacks/ 

```

namespace {
  const int EMPTY_VAL = 0;
}

class MyQueue {
private:
  stack<int> stack1;
  stack<int> stack2;
  int m_top;
  void stackSwap(stack<int> & stack_a, stack<int> & stack_b){
    while(!stack_a.empty()){
      stack_b.push(stack_a.top());
      stack_a.pop();
    }
  }

public:

  MyQueue() {
    m_top = EMPTY_VAL;
  }

  void push(int element) {
    if(stack1.empty()) {
      m_top = element;
    }

    stack1.push(element);
  }

  int pop() {
    int pop_val = EMPTY_VAL;
    if(stack1.empty()){
      return pop_val;
    }
    stackSwap(stack1,stack2);

    pop_val = m_top;
    stack2.pop();
    //update top after pop
    if(!stack2.empty()){
      m_top = stack2.top();
    } else {
      m_top = EMPTY_VAL;
    }

    stackSwap(stack2,stack1);

    return pop_val;
  }

  int top() {
    return m_top;
  }
};
```