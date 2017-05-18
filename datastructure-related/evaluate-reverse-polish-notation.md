### Evaluate Reverse Polish Notation

Evaluate the value of an arithmetic expression in Reverse Polish Notation.

Valid operators are `+, -, *, /`. Each operand may be an integer or another expression.

Some examples:
```
  ["2", "1", "+", "3", "*"] -> ((2 + 1) * 3) -> 9
  ["4", "13", "5", "/", "+"] -> (4 + (13 / 5)) -> 6
```

https://leetcode.com/problems/evaluate-reverse-polish-notation/#/description

```
//Design: using functor, and polymorphism
typedef unsigned int uint;

struct Operator {
    virtual int operator()(int a, int b)=0;  
};
struct Add : public Operator {
    int operator()(int a, int b){ return a+b; }
};
struct Minus : public Operator {
    int operator()(int a, int b){ return a-b; }
};
struct Multi : public Operator {
    int operator()(int a, int b){ return a*b; }
};
struct Div : public Operator {
    int operator()(int a, int b){ return a/b; }
};

class Solution {
private:
    unordered_map<string,Operator *> m_oper_map;
    bool isOperator(const string & s){
        return m_oper_map.find(s)!=m_oper_map.end();
    }
    
public:
    Solution(){
        m_oper_map.insert(make_pair("+",new Add()));
        m_oper_map.insert(make_pair("-",new Minus()));
        m_oper_map.insert(make_pair("*",new Multi()));
        m_oper_map.insert(make_pair("/",new Div()));
    }


    int evalRPN(vector<string>& tokens) {
        stack<int> stk;
        
        for(uint i=0; i<tokens.size(); ++i){
            const std::string & s = tokens[i];
            
            //1.handle number
            if(!isOperator(s)){
                stk.push(atoi(s.c_str()));
                continue;
            }
            
            //2.handle operator
            //check validation
            if(stk.size()<2) { return INT_MIN; }
            int n2 = stk.top(); stk.pop();
            int n1 = stk.top(); stk.pop();
            int result;
            result = (*m_oper_map[s])(n1,n2);
            stk.push(result);
        }
        
        if(stk.size()!=1) { return INT_MIN; }
        return stk.top();
        
    }
};
```

```
//Design: using function pointer
namespace {
typedef unsigned int uint;

int my_add(int a, int b){return a+b;}
int my_minus(int a,int b){return a-b;}
int my_multi(int a,int b){return a*b;}
int my_div(int a,int b){return a/b;}
}

class Solution {
private:
    //unordered_map<string,int (*)(int,int)> m_oper_map; //this is also ok
    typedef int (*oper)(int,int);
    unordered_map<string,oper> m_oper_map;
    
    bool isOperator(const string & s){
        return m_oper_map.find(s)!=m_oper_map.end();   
    }
    
public:
    Solution(){
        m_oper_map["+"] = &my_add;
        m_oper_map["-"] = &my_minus;
        m_oper_map["*"] = &my_multi;
        m_oper_map["/"] = &my_div;
    }

    int evalRPN(vector<string>& tokens) {
        stack<int> stk;
        
        for(uint i=0; i<tokens.size(); ++i){
            const std::string & s = tokens[i];
            
            //1.handle number
            if(!isOperator(s)){
                stk.push(atoi(s.c_str()));
                continue;
            }
            
            //2.handle operator
            //check validation
            if(stk.size()<2) { return INT_MIN; }
            int n2 = stk.top(); stk.pop();
            int n1 = stk.top(); stk.pop();
            int result;
            result = m_oper_map[s](n1,n2);

            stk.push(result);
        }
        
        if(stk.size()!=1) { return INT_MIN; }
        return stk.top();
        
    }
};
```

```
typedef unsigned int uint;

class Solution {
private:
    bool isOperator(const string & s){
        return (s=="+" || s=="-" || s=="/" || s=="*");   
    }
    
public:
    int evalRPN(vector<string>& tokens) {
        stack<int> stk;
        
        for(uint i=0; i<tokens.size(); ++i){
            const std::string & s = tokens[i];
            
            //1.handle number
            if(!isOperator(s)){
                stk.push(atoi(s.c_str()));
                continue;
            }
            
            //2.handle operator
            //check validation
            if(stk.size()<2) { return INT_MIN; }
            int n2 = stk.top(); stk.pop();
            int n1 = stk.top(); stk.pop();
            int result;
            switch (s[0]) {
                case '+': result = n1+n2; break;
                case '-': result = n1-n2; break;
                case '*': result = n1*n2; break;
                case '/': result = n1/n2; break;
                default: return INT_MIN;
            }
            stk.push(result);
        }
        
        if(stk.size()!=1) { return INT_MIN; }
        return stk.top();
        
    }
};
```
