### Triangle

Given a triangle, find the minimum path sum from top to bottom. Each step you may move to adjacent numbers on the row below.  
[http://www.lintcode.com/en/problem/triangle/](http://www.lintcode.com/en/problem/triangle/)

```
class Solution {
public:
    /**
     * @param triangle: a list of lists of integers.
     * @return: An integer, minimum path sum.
     */
    int minimumTotal(vector<vector<int> > &A) {
        if(A.empty())     return 0;
        if(A.size() == 1) return A[0][0];

        const unsigned int row_num = A.size();
        unsigned int row,row_elem;

        //end at last-1 row
        for(row = 0; row < row_num-1; ++row){  
            //first elem of row+1
            A[row+1][0] += A[row][0];

            //start from 2nd element
            for(row_elem = 1; row_elem < A[row].size(); row_elem++){ 
                if(A[row][row_elem-1] < A[row][row_elem])
                    A[row+1][row_elem] += A[row][row_elem-1];
                else
                    A[row+1][row_elem] += A[row][row_elem];
            }

            //last elem of row+1
            A[row+1][A[row].size()] += A[row][A[row].size()-1];
        }

        //loop last row, get min
        vector<int> last_row = A[A.size()-1];
        int min = last_row[0];
        for(vector<int>::const_iterator i=last_row.begin();i!=last_row.end();++i){
            if(min > *i) min = *i;
        }

        return min;
    }
};
```



