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