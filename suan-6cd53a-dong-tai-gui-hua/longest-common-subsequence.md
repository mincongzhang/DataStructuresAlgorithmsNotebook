### Longest Common Subsequence

Given two strings, find the longest common subsequence (LCS).
Your code should return the length of LCS.

http://www.lintcode.com/en/problem/longest-common-subsequence/

```
class Solution {
  std::vector< std::vector<int> > m_lcs_map;

  int LCS(const std::string & A, int a, const std::string & B, int b){
    if(a>=A.size() || b>=B.size()){
      return 0;
    }

    if(m_lcs_map[a][b]>=0) return m_lcs_map[a][b];

    int longest(0);
    if(A[a]==B[b]){
      longest = 1+LCS(A,a+1,B,b+1);
      m_lcs_map[a][b] = longest;
      return longest;
    }

    longest = std::max(LCS(A,a+1,B,b),LCS(A,a,B,b+1));
    m_lcs_map[a][b] = longest;
    return longest;
  }


public:
    /**
     * @param A, B: Two strings.
     * @return: The length of longest common subsequence of A and B.
     */
     
     
    int longestCommonSubsequence(string A, string B) {
        std::vector<int> vec_b(B.size(),-1);
        std::vector< std::vector<int> > lcs_map(A.size(),vec_b);
        m_lcs_map = lcs_map;

        return LCS(A,0,B,0);

    }
};

```