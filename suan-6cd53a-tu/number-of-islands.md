### Number of Islands

Given a 2d grid map of '1's (land) and '0's (water), count the number of islands. An island is surrounded by water and is formed by connecting adjacent lands horizontally or vertically. You may assume all four edges of the grid are all surrounded by water.

https://leetcode.com/problems/number-of-islands/#/description

```
class Solution {
    int m_row_size, m_col_size;
private:
    bool search(vector<vector<char>>& grid, int r, int c){
        if(r<0 || r>=m_row_size || c<0 || c>=m_col_size) return false;
       
        if(grid[r][c]=='1'){
            grid[r][c] = '0';
           
            search(grid,r-1,c);
            search(grid,r,c-1);
            search(grid,r+1,c);
            search(grid,r,c+1);

            return true;
        }
        
        return false;
    }
    
public:
    int numIslands(vector<vector<char>>& grid) {
        if(grid.empty()) return 0;
        
        int count = 0;
        m_row_size = grid.size();
        m_col_size = grid.front().size();

        for(int r=0;r<m_row_size; ++r){
            for(int c=0;c<m_col_size; ++c){
                if(search(grid,r,c)){
                    count++;        
                }
            }
        }
        
        return count;
    }
};
```