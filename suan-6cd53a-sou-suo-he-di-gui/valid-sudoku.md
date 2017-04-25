

### Valid Sudoku
Determine whether a Sudoku is valid.  
A valid Sudoku board (partially filled) is not necessarily solvable. Only the filled cells need to be validated.  
http://www.lintcode.com/en/problem/valid-sudoku/  

```
class Solution {
private:
  vector<vector<int>> m_board;
  size_t m_size;

  void getBoard(const vector<vector<char>>& board){
    vector<int> array(board.size(),0);
    vector<vector<int>> tmp_board(board.size(),array);
    m_board = tmp_board;

    for(size_t r=0; r<m_size; ++r){
      for(size_t c=0; c<m_size; ++c){
        if(board[r][c] <= '9' && board[r][c] >= '1'){
          m_board[r][c] = int(board[r][c] - '0');
        } else {
          m_board[r][c] = 0;
        }
        //std::cout<<m_board[r][c];
      }
      //std::cout<<std::endl;
    }
  }

  bool canPlace(size_t row,size_t col,int num){
    //Row
    for(size_t r=0; r<m_size; ++r){
      if(r == row) continue;
      if(m_board[r][col] == num) return false;
    }

    //Col
    for(size_t c=0;c<m_size; ++c){
      if(c == col) continue;
      if(m_board[row][c] == num) return false;
    }

    //Box
    size_t row_begin = (row/3)*3;
    size_t col_begin = (col/3)*3;
    for(size_t r=0; r<3; ++r){
      for(size_t c=0; c<3; ++c){
        if(r+row_begin==row && c+col_begin==col) continue;
        if(m_board[r+row_begin][c+col_begin] == num) return false;
      }
    }

    return true;
  }

  bool solveSudoku(const size_t col){
    if(col == m_size) return true;

    //std::cout<<"checking col["<<col<<"]"<<std::endl;

    for(size_t row=0; row<m_size; ++row){
      //Already has number
      //std::cout<<"checking ["<<m_board[row][col]<<"]"<<std::endl;


      if(m_board[row][col] > 0) {
        if(canPlace(row,col,m_board[row][col])){
          continue;
        } else {
          return false;
        }
      }

      //Empty
      /*
      bool can_place = false;
      for(size_t i=0; i<=9; ++i){
        if(canPlace(row,col,i)){
          can_place = true;
          //m_board[row][col] = i;
          break;
        }
        //m_board[row][col] = 0;
      }

      if(!can_place) return false;
      */
    }

    return solveSudoku(col+1);
  }

public:
  /**
   * @param board: the board
   * @return: wether the Sudoku is valid
   */
  bool isValidSudoku(const vector<vector<char>>& board) {
    m_size = board.size();
    getBoard(board);
    return solveSudoku(0);
  }
};
```