### Valid Parentheses

Given a string containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.
The brackets must close in the correct order, "()" and "()[]{}" are all valid but "(]" and "([)]" are not.

https://leetcode.com/problems/valid-parentheses/#/description

```
class Solution {
public:
    bool isValid(string s) {
        if(s.size()%2 == 1) return false;
        
        std::stack<char> stk;
        
        for(size_t it=0; it<s.size();++it){
            if(s[it]=='(' || s[it]=='[' || s[it]=='{'){
                stk.push(s[it]);
                continue;
            } 
            
            if(stk.empty()) return false;
            
            if(s[it]==')' && stk.top()=='('){
                stk.pop();
                continue;
            }

            if(s[it]==']' && stk.top()=='['){
                stk.pop();
                continue;
            }
            
            if(s[it]=='}' && stk.top()=='{'){
                stk.pop();
                continue;
            }
        }
        
        return stk.empty();
    }
};
```