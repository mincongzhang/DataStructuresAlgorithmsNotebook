### Longest Chain

You are given a library with n words (w[0], w[1], ..., w[n - 1]). You
choose a word from it, and in each step, remove one letter from this
word only if doing so yields a another word in the library. What is the
longest possible chain of these removal steps?
Constraints:
1 ≤ n ≤ 50000
1 ≤ the length of each string in w ≤ 50
Each string composed of lowercase ascii letters only.
Input Format:
Complete the function "longest_chain" which contains an array of
strings "w" as its argument.
Output Format:
Return a single integer that represents the length of the longest chain of
character removals possible.
Sample Input:
6
a
b
ba
bca
bda
bdca
Sample Output:
4

```
#include <unordered_map>

namespace {
    typedef unsigned int uint;
    struct WordChain {
        string * p_word;
        int      max_chain;
    };
    typedef unordered_map <string*, int> WordGroup;
    typedef unordered_map <int, WordGroup> LengthGroup;
}

bool hasOneDiff(const string & w1, const string & w2) {
   for(uint i = 0, j=0; i < w1.size()-1; i++) {
       if(w1[i] != w2[j]) {
           if(i != j) return false; 
       } else {
           j++;
       }
   }
   return true;
}

int longestChain(vector < string > words) {
    // Get the group of words
    int largest_group = 0;
    LengthGroup groups;
    
    for(uint i=0; i<words.size(); ++i){
        int length = words[i].size();
        //Initial all word chain number as 1
        groups[length][&words[i]] = 1;
        //Get largest group
        largest_group = max(length, largest_group);
    }
    
    int longest_chain = 1;
    //Start from the group with 2 chars
    for(uint i = 2; i <= largest_group; i++) {
        if(groups[i].empty()) {
            continue;
        }
        
        Calculate longest chain from length-1 group(Dynamic Programming)
        for(WordGroup::iterator w1=groups[i].begin(); w1!=groups[i].end(); ++w1){
            for(WordGroup::iterator w2=groups[i-1].begin(); w2!=groups[i-1].end(); ++w2){
                if(hasOneDiff(*(w1->first), *(w2->first))) {
                    w1->second = max(w1->second, w2->second+1);
                }
            }
            longest_chain = max(longest_chain, w1->second);
        }
    }
    
    return longest_chain;
}
```