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

```
//Non-recursive solution, using a map to save metadata
class Solution {
private:
  int getMin(int n1,int n2,int n3){
    return std::min(n1,std::min(n2,n3));
  }

public:
  /**
   * @param word1 & word2: Two string.
   * @return: The minimum number of steps.
   */
  int minDistance(string word1, string word2) {
    //return getMinDist(word1,0,word2,0);
    
    vector<int> v(word1.size()+1,0);
    vector< vector<int> > dist_map(word2.size()+1,v);

    for(int j=0; j<=word1.size(); ++j){
        dist_map[0][j] = j;
    }

    for(int i=0; i<=word2.size(); ++i){
        dist_map[i][0] = i;
    }
    
    for(int i=1; i<=word2.size(); ++i){
        for(int j=1; j<=word1.size(); ++j){
            if(word2[i-1] == word1[j-1]){
                dist_map[i][j] = dist_map[i-1][j-1];
            } else {
                dist_map[i][j] = 1+getMin(dist_map[i-1][j],
                                   dist_map[i][j-1],dist_map[i-1][j-1]);
            }
        }    
    }
    
    //for(int i=0; i<=word2.size(); ++i){
    //    for(int j=0; j<=word1.size(); ++j){
    //        std::cout<<dist_map[i][j];
    //    }
    //    std::cout<<std::endl;
    //}    
    
    return dist_map.back().back();
    
  }
};
```