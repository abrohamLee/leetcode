#### 主要的问题在于怎么判断当前的方格能填哪些数，我们直接开3个bool数组即可
> 一个小trick是：位置为 `[i, j] ` 的小方格， 在 位置为 `[i/3, j/3]` 的小九宫格(cell)中

```
class Solution {
public:
    bool row[9][9], col[9][9], cell[3][3][9]; //判断每一行，每一列，每一个小九宫格是否出现了 1-9
    void solveSudoku(vector<vector<char>>& board) {
        memset(row, 0, sizeof(row));
        memset(col, 0, sizeof(col));
        memset(cell, 0, sizeof(cell));

        for(int i=0; i<9; i++)
        {
            for(int j=0; j<9; j++)
            {
                if(board[i][j] != '.')
                {
                    int t = board[i][j] - '1'; //把1-9映射成0-8, 防止数组越界
                    row[i][t]=col[j][t]=cell[i/3][j/3][t]=true;
                }
            }
        }

        dfs(board, 0, 0); //从左上角开始搜起
    }
    bool dfs(vector<vector<char>>& board, int x, int y)
    {
        //搜索顺序为从左到右，从上到下，走「之」字形
        if(y==9) x++, y=0;
        if(x==9) return true; //已经搜完了

        //若当前位置[x, y]有数
        if(board[x][y] != '.') return dfs(board, x, y+1);
        //若当前位置[x, y]没有数，看看[x, y]处能不能填i
        for(int i=0; i<9; i++)
        {
            if(!row[x][i] && !col[y][i] && !cell[x/3][y/3][i]){
                board[x][y] = '1' + i; //由于i belong to [0,8] 此处记得+1变回去
                row[x][i]=col[y][i]=cell[x/3][y/3][i]=true;
                if(dfs(board, x, y+1)) return true;
                board[x][y] = '.';
                row[x][i]=col[y][i]=cell[x/3][y/3][i]=false;
            }
        }
        return false;

    }
};
```
