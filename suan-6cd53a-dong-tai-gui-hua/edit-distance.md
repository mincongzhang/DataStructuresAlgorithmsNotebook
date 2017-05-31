### Edit Distance
Given two words word1 and word2, find the minimum number of steps required to convert word1 to word2. (each operation is counted as 1 step.) You have the following 3 operations permitted on a word:  
Insert a character  
Delete a character  
Replace a character  
http://www.lintcode.com/en/problem/edit-distance/  

```
class Solution {
private:
  int getMin(int n1,int n2,int n3){
    return std::min(n1,std::min(n2,n3));
  }

  int getMinDist(const string word1, const size_t word1_len, const string word2, const size_t word2_len){
    if(word1_len==0) return word2_len;//Insert all elements
    if(word2_len==0) return word1_len;//Erase  all elements

    //If last element equal
    if(word1[word1_len-1] == word2[word2_len-1]){
      return getMinDist(word1,word1_len-1,word2,word2_len-1);
    }

    //If not equal(insert/erase/replace)
    return 1+getMin(getMinDist(word1,word1_len,word2,word2_len-1),
                    getMinDist(word1,word1_len-1,word2,word2_len),
                    getMinDist(word1,word1_len-1,word2,word2_len-1));
  }
public:
  /**
   * @param word1 & word2: Two string.
   * @return: The minimum number of steps.
   */
  int minDistance(string word1, string word2) {
    return getMinDist(word1,word1.size(),word2,word2.size());
  }
};
```

```
class Solution {
private:
  int getMin(int n1,int n2,int n3){
    return std::min(n1,std::min(n2,n3));
  }
  
  int getMinDist(const std::string & word1,int w1,const std::string word2, int w2){
	if(w1==word1.size()) return word2.size()-w2;
	if(w2==word2.size()) return word1.size()-w1;
	
	if(word1[w1]==word2[w2]) return getMinDist(word1,w1+1,word2,w2+1);
	
	return 1+getMin(
	getMinDist(word1,w1,word2,w2+1),   //word1 add
	getMinDist(word1,w1+1,word2,w2),   //word1 delete
	getMinDist(word1,w1+1,word2,w2+1)  //replace
	);

  }

public:
  /**
   * @param word1 & word2: Two string.
   * @return: The minimum number of steps.
   */
  int minDistance(string word1, string word2) {
    return getMinDist(word1,0,word2,0);
  }
};
```