
### Interleaving String
Given three strings: s1, s2, s3, determine whether s3 is formed by the interleaving of s1 and s2.  
http://www.lintcode.com/en/problem/interleaving-string/

```
class Solution {
public:
  /**
   * Determine whether s3 is formed by interleaving of s1 and s2.
   * @param s1, s2, s3: As description.
   * @return: true of false.
   */
  bool isInterleave(string s1, string s2, string s3) {
    size_t s1_it=0;
    size_t s2_it=0;
    size_t s3_it=0;

    for(;s3_it<s3.size();++s3_it){
      if(s1_it<s1.size()){
        if(s1[s1_it] == s3[s3_it]){
          s1_it++;
          continue;
        }
      }

      if(s2_it<s2.size()){
        if(s2[s2_it] == s3[s3_it]){
          s2_it++;
          continue;
        }
      }

      break;
    }

    return (s1_it==s1.size() && s2_it==s2.size() && s3_it==s3.size());
  }
};
```