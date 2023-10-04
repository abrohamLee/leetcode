f[i][j] 表示s[1\~i] 与 p[1\~j]是否匹配:<br>
1. p[j] != '*' : (s[i] == p[j] || p[j] == '?') && f[i-1][j-1]
2. p[j] == '*':
    * '*'不匹配字符: f[i][j-1]
    * '*'匹配1个字符: f[i-1][j]
    * '*'匹配2个字符: f[i-2][j]
    * ...
   
  * f[i][j] = f[i][j-1] || f[i-1][j-1] || f[i-2][j-1] || ... || f[0][j-1]
  * f[i-1][j] = f[i-1][j-1] || f[i-2][j-1] || ... || f[0][j-1]
  *  Therefore f[i][j] = f[i][j-1] || f[i-1][j] 

 

```
class Solution {
public:
    bool isMatch(string s, string p) {
      int n = s.size(), m = p.size();
      s = ' '+s, p =' '+p;
      vector<vector<bool>> f(n+1, vector<bool>(m+1));
      f[0][0] = true;
      for(int i=0; i<=n; i++){ //f[0][j] 是有意义的，e.g. p='*********'
        for(int j=1; j<=m; j++){ //f[i][0] 无意义
          if(p[j] == '*'){
            f[i][j] = f[i][j-1] || i && f[i-1][j];
          }
          else{
            f[i][j] = (s[i] == p[j] || p[j] == '?') && i && f[i-1][j-1];
          }
        }
      }
      return f[n][m];
    }
};
```
