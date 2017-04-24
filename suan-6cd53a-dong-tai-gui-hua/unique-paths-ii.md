### Unique Paths II
Follow up for "Unique Paths":  
Now consider if some obstacles are added to the grids. How many unique paths would there be?  
An obstacle and empty space is marked as 1 and 0 respectively in the grid.  

```
class Solution {
public:
  /**
   * @param obstacleGrid: A list of lists of integers
   * @return: An integer
   */
  int uniquePathsWithObstacles(vector<vector<int> > &obstacleGrid) {
    if(obstacleGrid.empty()) return 0;
    if(obstacleGrid[0][0]==1) return 0;

    vector<int> array(obstacleGrid.front().size(),1); //initial with 1
    vector< vector<int> > path_map(obstacleGrid.size(),array);

    size_t col = array.size();
    size_t row = path_map.size();
    for(size_t r = 0; r<row; ++r){
      for(size_t c = 0; c<col; ++c){
        if(obstacleGrid[r][c]==1){
          path_map[r][c] = 0;
          continue;
        }

        if(r==0 && c==0) continue;

        if(r!=0 && c!=0){
          path_map[r][c] = path_map[r-1][c] + path_map[r][c-1];
          continue;
        }

        if(r==0){
          path_map[r][c] =  path_map[r][c-1];
        } else {
          path_map[r][c] =  path_map[r-1][c];
        }
      }
    }

    return path_map.back().back();
  }
};
```

