### Wildcard Matching

Implement wildcard pattern matching with support for '?' and '*'.


```
'?' Matches any single character.
'*' Matches any sequence of characters (including the empty sequence).

The matching should cover the entire input string (not partial).

The function prototype should be:
bool isMatch(const char *s, const char *p)

Some examples:

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

```
//Dynamic programming, use 2D array to save metadata

typedef unsigned int uint;
class Solution {
private:

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

	void print(const vector< vector<bool> > & match_map){
		//match_map[s][p]
		for(uint s=0; s<match_map.size(); s++){
			for(uint p=0; p<match_map[s].size(); p++){
				std::cout<<match_map[s][p];
			}
			std::cout<<std::endl;
		}
		std::cout<<std::endl;
	}

public:
    bool isMatch(string s, string p) {
        p = removeDuplicateStar(p);
        if(s==p) return true;
        if(!s.empty()&&p.empty()) return false;
        if(s.empty()&&(p=="*")) return true;
        if(s.empty()&&(!p.empty() && p!="*")) return false;
        
		//std::cout<<"p:"<<p<<std::endl;
		//std::cout<<"s:"<<s<<std::endl;
        
        vector<bool> v(p.size()+1,false);
        vector< vector<bool> > match_map(s.size()+1,v);
        
		//1.s_i = s.size() or 0, p_i = p.size() or 0
		match_map.back().back() = true;
		
		//2.p_i = p.size(), s remaining, must be false
		//Already set to false
		
		//3.s_i = s.size(), if p_i = '*' and p_i+1 = true, p[p_i] = true
		for(uint i=p.size()-1; i>0; i--){
			if(match_map[s.size()][i+1] && p[i]=='*'){
				match_map[s.size()][i] = true;
			} else {
				match_map[s.size()][i] = false;
			}
		}
		
		//print(match_map);
		//system("pause");

		for(int s_i=s.size()-1; s_i>=0;s_i--){
			for(int p_i=p.size()-1; p_i>=0; p_i--){
				if(s[s_i]==p[p_i] || p[p_i]=='?'){
					match_map[s_i][p_i] = match_map[s_i+1][p_i+1];
					continue;
				}

				if(p[p_i] == '*'){
					match_map[s_i][p_i] = match_map[s_i+1][p_i+1] || match_map[s_i][p_i+1] || match_map[s_i+1][p_i];
					continue;
				}
				
				match_map[s_i][p_i] = false;
			}
		}

		//print(match_map);
		return match_map.front().front();
    }
};
```

