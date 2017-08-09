### Lexicographical Numbers


Given an integern, return 1 -nin lexicographical order.

For example, given 13, return: \[1,10,11,12,13,2,3,4,5,6,7,8,9\].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

https://leetcode.com/problems/lexicographical-numbers/description/

```
//Recursive
class Solution {
private:
    void generate(int i, const int n, vector<int> & result){
        if(i>n) return;
        result.push_back(i);
        i*=10;
        
        generate(i,n,result);
        for(int j=1; j<=9; j++){
            int tmp = i+j;
            if(tmp<=n){
                generate(tmp,n,result);
            } else {
                return;
            }
        }
    }
public:
    vector<int> lexicalOrder(int n) {
        vector<int> result;

        for(int i=1; i<=9; ++i){
            generate(i,n,result);
        }
        
        return result;
    }
};
```

```
//convert to string, sort and convert back
class Solution {
public:
    vector<int> lexicalOrder(int n) {
        std::set<std::string> s;
        for(int i=1; i<=n; ++i){
            s.insert(std::to_string(i));
        }
        
        vector<int> result;
        for(std::set<std::string>::const_iterator it=s.begin(); it!=s.end(); ++it){
            result.push_back(std::stoi(*it));
        }
        
        return result;
    }
};
```

```
//define operator <
struct Lex {
    int val;
    Lex(int i):val(i){}
    /*
    bool operator>(const Lex & l) const {
        return val>l.val;
    }
       bool operator<(const Lex & l) const {
        return val<l.val;
    }
    */
};

bool operator<(const Lex & r, const Lex & l){
    int r_i(r.val), l_i(l.val);
    if(r_i<=0 || l_i<=0){
        return r_i<l_i;
    }
    
    std::stack<int> s_r, s_l;
    while(r_i){
        s_r.push(r_i%10);
        r_i/=10;
    }
    while(l_i){
        s_l.push(l_i%10);
        l_i/=10;
    }
    
    while(!s_r.empty() && !s_l.empty()){
        if(s_r.top()==s_l.top()){
            s_r.pop();
            s_l.pop();
        } else {
            return s_r.top()<s_l.top();
        }
    }
    
    return s_r.empty();
}

class Solution {
public:
    vector<int> lexicalOrder(int n) {
        std::set<Lex> s;
        for(int i=1; i<=n; ++i){
            s.insert(Lex(i));
        }
        
        vector<int> result;
        for(std::set<Lex>::const_iterator it=s.begin(); it!=s.end(); ++it){
            result.push_back(it->val);
        }
        
        return result;
    }
};
```

