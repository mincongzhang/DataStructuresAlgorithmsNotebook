

### Group Anagrams

Given an array of strings, group anagrams together.

For example, given: ["eat", "tea", "tan", "ate", "nat", "bat"], 
Return:

```
[
  ["ate", "eat","tea"],
  ["nat","tan"],
  ["bat"]
]
```

https://leetcode.com/problems/group-anagrams/#/description


```
class Solution {
    typedef std::unordered_map<std::string, vector<std::string> > Hash;
public:
    vector<vector<string>> groupAnagrams(vector<string>& strs) {
        Hash hash;
        for(int i=0; i<strs.size(); ++i){
            std::string cur_str = strs[i];
            std::sort(cur_str.begin(),cur_str.end());
            hash[cur_str].push_back(strs[i]);
        }
        
        vector< vector<string> > result;
        for(Hash::const_iterator it=hash.begin(); it!=hash.end(); ++it){
            result.push_back(it->second);
        }
        
        return result;
    }
};
```