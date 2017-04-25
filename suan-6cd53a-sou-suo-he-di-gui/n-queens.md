
###  N-Queens
http://www.lintcode.com/en/problem/n-queens/

```
class Solution {
private:
  vector<vector<string> > m_results;

  void record(const vector<vector<bool> > & board, const int n){
    //std::cout<<"Solution:"<<std::endl;

    std::vector<std::string> result;
    for(int r=0; r<n; ++r){
      std::string str_row;
      for(int c=0; c<n; ++c){
        if(board[r][c]) str_row+="Q";
        else  str_row+=".";
      }
      //std::cout<<str_row<<std::endl;
      result.push_back(str_row);
    }

    m_results.push_back(result);
  }

  bool canPlace(const vector<vector<bool> > & board,const int n, const int row, const int col){
    for(int c=col; c>=0; --c){
      if(board[row][c]) return false;
    }

    int r = row;
    int c = col;
    while(r>=0 && c>=0){
      if(board[r][c]) return false;
      r--;
      c--;
    }

    r = row;
    c = col;
    while(r<n && c>=0){
      if(board[r][c]) return false;
      r++;
      c--;
    }

    return true;
  }

  bool solve(vector<vector<bool> > & board, const int n, const int c){
    //go through column "c"
    if(c == n){
      record(board,n);
      return true;
    }

    for(int r=0; r<n; ++r){
      if(canPlace(board,n,r,c)){
        board[r][c] = true;
        solve(board,n,c+1);
        board[r][c] = false;
      }
    }

    return false;
  }

public:
  /**
   * Get all distinct N-Queen solutions
   * @param n: The number of queens
   * @return: All distinct solutions
   * For example, A string '...Q' shows a queen on forth position
   */
  vector<vector<string> > solveNQueens(int n) {
    vector<bool> board_col(n,false);
    vector<vector<bool> > board(n,board_col);
    solve(board,n,0);
    return m_results;
  }
};

```