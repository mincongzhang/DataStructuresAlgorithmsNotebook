
### Distinct Subsequences
Given a string S and a string T, count the number of distinct subsequences of T in S.

A subsequence of a string is a new string which is formed from the original string by deleting some (can be none) of the characters without disturbing the relative positions of the remaining characters. (ie, "ACE" is a subsequence of "ABCDE" while "AEC" is not).  

http://www.lintcode.com/en/problem/distinct-subsequences/  

```
//e.g.
//S: xxxabcxxxabc T:abc, awnser is 4
//we can do:
//---abc------
//---------abc
//---a------bc
//---ab------c
```

```
//recursive solution

class Solution {

private:
  std::string m_S;
  std::string m_T;

  int getNumDistinct(size_t s_start, size_t t_start){
    //When there is no T left, we have 1 distinct subsequence
    if(t_start==m_T.size()){
      return 1;
    }

    if(s_start == m_S.size()){
      //when S is empty but we still have T, means we have no distinct subsequences
      return 0;
    }

    int num = 0;
    for(size_t s=s_start; s<m_S.size(); ++s){
      if(m_S[s]==m_T[t_start]){
        num += getNumDistinct(s+1,t_start+1);
      }
    }

    return num;
  }//getNumDistinct

public:
  /**
   * @param S, T: Two string.
   * @return: Count the number of distinct subsequences
   */
  int numDistinct(string &S, string &T) {
    m_S = S;
    m_T = T;
    return getNumDistinct(0,0);
  }
};
```

```
//Dynamic programming space O(T*S)
//e.g.
//S: xxxabcxxxabc T:abc, awnser is 4
//we can do:
//---abc------
//---ab------c
//---a------bc
//---------abc

//DP:
//  x x x a b c x x x a b c
//a - - - - - - - - - - - - 0
//b - - - - - - - - - - - - 0
//c - - - - - - - - - - - - 0
//  1 1 1 1 1 1 1 1 1 1 1 1 1

//start from bottom right
//if (S[i] == T[j]) {
//    N[i][j] = N[i+1][j] + N[i+1][j+1];
//} else {
//    N[i][j] = N[i+1][j];
//}

//DP:
//  x x x a b c x x x a b c
//a 4 4 4 4 1 1 1 1 1 1 0 0 0
//b 3 3 3 3 3 1 1 1 1 1 1 0 0
//c 2 2 2 2 2 2 1 1 1 1 1 1 0
//  1 1 1 1 1 1 1 1 1 1 1 1 1
```
```
Dynamic programming space O(T*S)
/*
Example
  Given S = "rabbbit", T = "rabbit", return 3.

    r a b b b i t S
  1 1 1 1 1 1 1 1
r 0 1 1 1 1 1 1 1
a 0 0 1 1 1 1 1 1
b 0 0 0 1 2 3 3 3
b 0 0 0 0 1 3 3 3
i 0 0 0 0 0 0 3 3
t 0 0 0 0 0 0 0 3
T
*/
//if (!=) =left (inherit the previous count)
//if (==) =left+upper_left, it means if equal, add the result of previous compare [x-1][y-1]/*

class Solution {

public:
  /**
   * @param S, T: Two string.
   * @return: Count the number of distinct subsequences
   */
  int numDistinct(string &S, string &T) {
    //count[s][t]
    vector<int> T_count(T.size()+1,0);
    vector< vector<int> > count(S.size()+1,T_count);

    for(size_t s=0; s<count.size();++s){
      count[s][0]=1;
    }

    for(size_t s=0; s<S.size();++s){
      for(size_t t=0; t<T.size();++t){
        if(S[s]!=T[t]){
          count[s+1][t+1]=count[s][t+1];
        } else {
          count[s+1][t+1]=count[s][t+1]+count[s][t];
        }
      }
    }

    return count.back().back();
  }
};
```

```
//Dynamic programming, space O(T)
/*
   r a b b b i t S
   1 1 1 1 1 1 1
r  1 1 1 1 1 1 1
a  0 1 1 1 1 1 1
b  0 0 1 2 3 3 3
b  0 0 0 1 3 3 3
i  0 0 0 0 0 3 3
t  0 0 0 0 0 0 3
T
*/
class Solution {

public:
  /**
   * @param S, T: Two string.
   * @return: Count the number of distinct subsequences
   */
  int numDistinct(string &S, string &T) {
    std::vector<int> count(T.size()+1,0);
    count[0]=1;

    for(int s=0; s<S.size();++s){
      //Why backward here? In this way you could get previous compare results without overwrite them
      for(int t=T.size()-1; t>=0;--t){
        if(S[s]==T[t]){
          count[t+1]+=count[t];
        }
      }
    }

    return count.back();
  }
};

//The same if you go the other way round
/*
   r a b b b i t S
r  3 0 0 0 0 0 0
a  3 3 0 0 0 0 0
b  3 3 3 3 1 0 0
b  2 2 2 2 1 0 0
i  2 1 1 1 1 1 0
t  1 1 1 1 1 1 1
   1 1 1 1 1 1 1
T
*/

```