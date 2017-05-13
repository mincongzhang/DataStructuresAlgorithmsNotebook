### Wildcard Matching

Implement wildcard pattern matching with support for '?' and '*'.


```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:

```
isMatch("aa","a") → false
isMatch("aa","aa") → true
isMatch("aaa","aa") → false
isMatch("aa", "*") → true
isMatch("aa", "a*") → true
isMatch("ab", "?*") → true
isMatch("aab", "c*a*b") → false
```

https://leetcode.com/problems/wildcard-matching/#/description

```
class Solution {
private:
    bool match(const std::string & s, const std::string & p, unsigned int s_i, unsigned int p_i){
        //END CASE 1: all match
        if(s_i==s.size() && p_i==p.size()) return true;

        //END CASE 2: s remaining
        if(s_i<s.size() && p_i==p.size()){
            return false;
        }

        //END CASE 3: p remaining
        if(s_i==s.size() && p_i<p.size()){
            //std::cout<<"p remaining:"<<p[p_i]<<std::endl;
            if(p[p_i] != '*'){
                return false;
            } 
            
            //p[p_i] == '*'
            //Don't know why return match(s,p,s_i, p_i+1) doesn't work
            while(p_i!=p.size()){
                if(p[p_i] == '*') p_i++;
                else return false;
            }
            return true;
        }
        
        //std::cout<<s[s_i]<<","<<p[p_i]<<std::endl;
        
        if(s[s_i] == p[p_i] || p[p_i]=='?') return match(s,p,s_i+1,p_i+1);
        if(p[p_i]=='*') {
            return ( match(s,p,s_i+1,p_i+1) || // * as same char
                     match(s,p,s_i+1,p_i)   || // * as multiple char
                     match(s,p,s_i,  p_i+1) ); // * as empty
        }
        
        return false;
    }

    std::string removeDuplicateStar(const std::string & p){
        if(p.size()==0) return p;
        
        std::string ret;
        for(unsigned int i=0; i<p.size()-1;++i){
            if(p[i]=='*' && p[i+1]=='*'){
                continue;
            }
            ret+=p[i];
        }
        
        ret+=p[p.size()-1];
        return ret;
    }

public:
    bool isMatch(string s, string p) {
        p = removeDuplicateStar(p);
        return match(s,p,0,0); 
    }
};
```

