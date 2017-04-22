### Reverse String

Write a function that takes a string as input and returns the string reversed.
Example:
Given s = "hello", return "olleh".

https://leetcode.com/problems/reverse-string/#/description

```
class Solution {
private:
    void swap(char & a, char & b){
        char tmp = a;
        a = b;
        b = tmp;
    }
public:
    string reverseString(string s) {
        if(s.empty()) return s;
        
        size_t begin(0),end(s.size()-1);
        
        while(begin<end){ //NOTE: cannot use <=
            swap(s[begin],s[end]);
            begin++;
            end--;
        }
        
        return s;
    }
};
```