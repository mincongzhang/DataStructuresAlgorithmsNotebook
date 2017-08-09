### Lexicographical Numbers


Given an integern, return 1 -nin lexicographical order.

For example, given 13, return: \[1,10,11,12,13,2,3,4,5,6,7,8,9\].

Please optimize your algorithm to use less time and space. The input size may be as large as 5,000,000.

https://leetcode.com/problems/lexicographical-numbers/description/

```
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

