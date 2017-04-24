
### Word Break
Given a string s and a dictionary of words dict, determine if s can be break into a space-separated sequence of one or more dictionary words.  
e.g. Given s = "lintcode", dict = ["lint", "code"]. Return true because "lintcode" can be break as "lint code".  
http://www.lintcode.com/en/problem/word-break/  

```
//Basic solution with recursion
class Solution {
private:
  bool breaking(const std::string & s, const unordered_set<string> & dict, size_t begin){
    if(begin == s.size()) return true;

    for(size_t i=begin+1; i<=s.size(); ++i){
      std::string tmp_str = s.substr(begin,i-begin);
      if(dict.find(tmp_str)!=dict.end() && breaking(s,dict,i)){
        return true;
      }
    }
    return false;
  }

public:
  /**
   * @param s: A string s
   * @param dict: A dictionary of words dict
   */
  bool wordBreak(string s, unordered_set<string> &dict) {
    if(dict.empty() && s.empty()) return true;
    if(dict.empty()) return false;
    if(s.size()==1) return dict.find(s)!=dict.end();

    return breaking(s,dict,0);
  }
};
```

```
//Dynamic programming with recursion
#include <utility>//Pair
class Solution {
private:
  std::unordered_set<std::string> m_false_dict;
  bool breaking(const std::string & s, const unordered_set<string> & dict, size_t begin){
    if(begin == s.size()) return true;

    for(size_t i=begin+1; i<=s.size(); ++i){
      std::string tmp_str = s.substr(begin,i-begin);
      if(dict.find(tmp_str)!=dict.end() && m_false_dict.find(tmp_str)==m_false_dict.end()){
        if(breaking(s,dict,i)){
          return true;
        } else {
          m_false_dict.insert(tmp_str);
        }
      }
    }
    return false;
  }

public:
  /**
   * @param s: A string s
   * @param dict: A dictionary of words dict
   */
  bool wordBreak(string s, unordered_set<string> &dict) {
    if(dict.empty() && s.empty()) return true;
    if(dict.empty()) return false;
    if(s.size()==1) return dict.find(s)!=dict.end();

    return breaking(s,dict,0);
  }
};
```

```
//Basic solution without recursion
#include <utility>//Pair
class Solution {
private:
  //std::unordered_map<std::string,bool> m_false_dict;
  bool breaking(const std::string & s, const unordered_set<string> & dict){
    size_t begin = 0;
    std::queue<size_t> Q;
    std::unordered_set<size_t> false_begin;
    Q.push(begin);

    while(!Q.empty()){
      size_t begin_i = Q.front();
      Q.pop();

      for(size_t i=begin_i+1; i<=s.size(); ++i){
        std::string tmp_str = s.substr(begin_i,i-begin_i);
        if(dict.find(tmp_str)!=dict.end()){
            if(i==s.size()){
                return true;
            } else {
                Q.push(i);
            }
        }
      }//for
    }//while

    return false;
  }

public:
  /**
   * @param s: A string s
   * @param dict: A dictionary of words dict
   */
  bool wordBreak(string s, unordered_set<string> &dict) {
    if(dict.empty() && s.empty()) return true;
    if(dict.empty()) return false;
    if(s.size()==1) return dict.find(s)!=dict.end();

    return breaking(s,dict);
  }
};
```

```
//Dynamic programming without recursion, checking max dict word length
#include <utility>//Pair
#include <algorithm>//max

class Solution {
private:
  bool breaking(const std::string & s, const unordered_set<string> & dict){
    size_t max_len=0;
    for(unordered_set<string>::const_iterator i=dict.begin();i!=dict.end();++i){
      max_len = std::max(max_len,i->size());
    }

    size_t begin = 0;
    std::queue<size_t> Q;
    std::unordered_set<size_t> false_begins;
    Q.push(begin);

    while(!Q.empty()){
      size_t begin_i = Q.front();
      Q.pop();
      if(false_begins.find(begin_i)!=false_begins.end()) {continue;}

      for(size_t i=begin_i+1; i<=s.size()&&i-begin_i<=max_len; ++i){
        std::string tmp_str = s.substr(begin_i,i-begin_i);
        if(dict.find(tmp_str)!=dict.end()){
          if(i==s.size()){
            return true;
          } else {
            false_begins.insert(begin_i);
            Q.push(i);
          }
        }
      }//for
    }//while

    return false;
  }

public:
  /**
   * @param s: A string s
   * @param dict: A dictionary of words dict
   */
  bool wordBreak(string s, unordered_set<string> &dict) {
    if(dict.empty() && s.empty()) return true;
    if(dict.empty()) return false;
    if(s.size()==1) return dict.find(s)!=dict.end();

    return breaking(s,dict);
  }
};
```
