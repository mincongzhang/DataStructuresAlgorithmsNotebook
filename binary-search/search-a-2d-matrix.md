### Search a 2D Matrix

Write an efficient algorithm that searches for a value in an m x n matrix.  
This matrix has the following properties:  
-Integers in each row are sorted from left to right.  
-The first integer of each row is greater than the last integer of the previous row.

http://www.lintcode.com/en/problem/search-a-2d-matrix/

```
typedef vector<vector<int> > Matrix;

class Solution {
private:
    int m_target;

    int searchColumn(const vector<vector<int> > & matrix){
        int begin = 0;
        int end   = matrix.size()-1;

        while(begin+1 < end){
            int mid = (begin+end)/2;
            int mid_val = matrix[mid][0];
            if(mid_val > m_target){
                end = mid;
                continue;
            }

            if(mid_val < m_target){
                begin = mid+1;
                continue;
            }

            return mid;
        }

        return begin;
    }


    bool searchRow(const vector<int> & row){
        int begin = 0;
        int end = row.size()-1;
        while(begin <= end){
            int mid = (begin+end)/2;
            int mid_val = row[mid];

            if(mid_val == m_target) return true;

            if(mid_val > m_target){
                end = mid-1;//NOTE:beware
            } else {
                begin = mid+1;//NOTE: this happens again...NOTE: because int mid = floor((begin+end)/2)
            }
        }

        return false;
    }

public:
    /**
     * @param matrix, a list of lists of integers
     * @param target, an integer
     * @return a boolean, indicate whether matrix contains target
     */
    bool searchMatrix(vector<vector<int> > &matrix, int target) {
        if(matrix.empty()) return false;
        m_target = target;

        int col = searchColumn(matrix);
        if(matrix[col][0] == m_target){
            return true;
        }

        return searchRow(matrix[col]);
    }
};
```



