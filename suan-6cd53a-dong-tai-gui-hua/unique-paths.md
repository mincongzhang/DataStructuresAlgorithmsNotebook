### Unique Paths

A robot is located at the top-left corner of a m x n grid \(marked 'Start' in the diagram below\).  
The robot can only move either down or right at any point in time. The robot is trying to reach the bottom-right corner of the grid \(marked 'Finish' in the diagram below\).  
How many possible unique paths are there?

[http://www.lintcode.com/en/problem/unique-paths/](http://www.lintcode.com/en/problem/unique-paths/)

```
class Solution {
public:
    /**
     * @param n, m: positive integer (1 <= n ,m <= 100)
     * @return an integer
     */
  int uniquePaths(int m, int n) {
    if(m <= 1 || n <= 1) return 1;

    std::vector<int> path_array(n,1);
    std::vector< std::vector<int> > path_map(m,path_array);

    for(int m_i=1; m_i<m; ++m_i){
      for(int n_i=1; n_i<n; ++n_i){
        path_map[m_i][n_i] = path_map[m_i-1][n_i]+path_map[m_i][n_i-1];
      }
    }

    return path_map.back().back();
  }
};
```


```
//Dynamic programming
//saving metadata
class Solution {
private:
    bool match(const std::string & s, const std::string & p, unsigned int s_i, unsigned int p_i){
        if(m_match_map[s_i][p_i] == 1) return true;
        if(m_match_map[s_i][p_i] == -1) return false;
        
        //END CASE 1: all match
        if(s_i==s.size() && p_i==p.size()){
            m_match_map[s_i][p_i] = 1;
            return true;
        } 

        //END CASE 2: s remaining
        if(s_i<s.size() && p_i==p.size()){
            m_match_map[s_i][p_i] = -1;
            return false;
        }

        //END CASE 3: p remaining
        if(s_i==s.size() && p_i<p.size()){
            //std::cout<<"p remaining:"<<p[p_i]<<std::endl;
            if(p[p_i] != '*'){
                m_match_map[s_i][p_i] = -1;
                return false;
            } 
            
            //p[p_i] == '*'
            //Don't know why return match(s,p,s_i, p_i+1) doesn't work
            while(p_i!=p.size()){
                if(p[p_i] == '*') p_i++;
                else { 
                    m_match_map[s_i][p_i] = -1;
                    return false; 
                }
            }
            
            m_match_map[s_i][p_i] = 1;
            return true;
        }
        
        //std::cout<<s[s_i]<<","<<p[p_i]<<std::endl;
     
        if(s[s_i] == p[p_i] || p[p_i]=='?') return match(s,p,s_i+1,p_i+1);  
        if(p[p_i]=='*') {
            return ( match(s,p,s_i+1,p_i+1) || // * as same char
                     match(s,p,s_i+1,p_i)   || // * as multiple char
                     match(s,p,s_i,  p_i+1) ); // * as empty
        }
        
        m_match_map[s_i][p_i] = -1;
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
    
    vector< vector<int> > m_match_map;

public:
    bool isMatch(string s, string p) {
        p = removeDuplicateStar(p);
        vector<int> v(p.size()+1,0);
        vector< vector<int> > match_map(s.size()+1,v);
        m_match_map = match_map;
        return match(s,p,0,0); 
    }
};
```
