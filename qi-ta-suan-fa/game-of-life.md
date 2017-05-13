### Game of Life

https://leetcode.com/problems/game-of-life/#/description

```
class Solution {
private:
    int countNeighbour(const vector<vector<int>>& board, const int i, const int j){
        int count = 0;
        
        int left  = (i>=1                ? i-1 : i);
        int right = (i<board.size()-1    ? i+1 : i);
        int up    = (j>=1                ? j-1 : j);
        int down  = (j<board[i].size()-1 ? j+1 : j);
        
        for(int n=left; n<=right; n++){
            for(int m=up; m<=down; m++){
                if(n==i && m==j) continue;
                if(board[n][m]>0) count++;
            }
        }

        return count;
    }
public:
    void gameOfLife(vector<vector<int>>& board) {
        // 0->dead -1
        // 0->live -2
        // 1->dead +2
        // 1->live +3
        
        for(int i=0; i<board.size(); ++i){
            for(int j=0; j<board[i].size();++j){
                //std::cout<<board[i][j]<<"";

                int count = countNeighbour(board,i,j);
                //std::cout<<"("<<count<<")";
                
                
                bool live = board[i][j]>0;
                if     (live && (count>3  || count <2)) board[i][j] = 2;
                else if(live && (count==3 || count==2)) board[i][j] = 3;
                else if(!live && count==3)             board[i][j] = -2;
                else                                   board[i][j] = -1;
                
                //std::cout<<">"<<board[i][j]<<",";
            }
            //std::cout<<std::endl;
        }

        for(int i=0; i<board.size(); ++i){
            for(int j=0; j<board[i].size();++j){
                if(board[i][j]==-2 || board[i][j]==3) board[i][j]=1;
                else board[i][j]=0;
            }
        }        
    }
};
```