### Friend Circles

There are N students in a class. Some of them are friends, while some are not. Their friendship is transitive in nature. For example, if A is a direct friend of B, and B is a direct friend of C, then A is an indirect friend of C. And we defined a friend circle is a group of students who are direct or indirect friends.

Given a N\*N matrix M representing the friend relationship between students in the class. If M\[i\]\[j\] = 1, then the ith and jth students are direct friends with each other, otherwise not. And you have to output the total number of friend circles among all the students.

```
Input: 
[[1,1,0],
 [1,1,0],
 [0,0,1]]
Output: 2
Explanation:The 0th and 1st students are direct friends, so they are in a friend circle. 
The 2nd student himself is in a friend circle. So return 2.
```

```
Input: 
[[1,1,0],
 [1,1,1],
 [0,1,1]]
Output: 1
Explanation:The 0th and 1st students are direct friends, the 1st and 2nd students are direct friends, 
so the 0th and 2nd students are indirect friends. All of them are in the same friend circle, so return 1.
```

```
namespace{
typedef unsigned int uint;
}

class Solution {
private:
    bool search(vector<vector<int>> & M,uint i){
        int count = 0;
        for(uint j=0; j<M[i].size();++j){
            if(M[i][j]>0){
                M[i][j] = 0;
                M[j][i] = 0;
                search(M,j);
                count++;
            }
        }

        return count>0;
    }


public:
    int findCircleNum(vector<vector<int>>& M) {

        int count = 0;
        for(uint i=0; i<M.size(); ++i){
            if(search(M,i)){ count++; }
        }

        return count;
    }
};
```

```
/*
 * Complete the function below.
 */
namespace{
    typedef unsigned int uint;
}

bool findNewFriend(vector<string> & friends, vector<bool> visited, uint i){
    if(visited[i]) return false;

    int direct_new_friends = 0;
    for(uint j=0; j<friends[i].size();++j){
        if(friends[i][j] == 'Y'){
            //reset, not new friend anymore
            friends[i][j] = 'N';
            friends[j][i] = 'N';

            //Find friend's friend (depth first search)
            findNewFriend(friends,visited,j);
            direct_new_friends++;
        }
    }

    visited[i] = true;
    return direct_new_friends>0;
}

int friendCircles(vector < string > friends) {
    int count = 0;

    vector<bool> visited(friends.size(),false);

    for(uint i=0; i<friends.size(); ++i){
        //search new friend for this people
        if(findNewFriend(friends,visited,i)){
            count++;
        }
    }

    return count;
}
```



