类似于区间DP的表示方法
f[i][j][k] 表示 s1[i~i+k-1] 与 s2[j ~ j+k-1]匹配的所有方案
我们只需要看看这些方案的集合是否非空
状态计算:
  f[i][j][k] 表示的集合可以按 s1 的第一段的长度进行分类(s1 的第一段的长度可以为: 1,2,...,k-1)
  假设s1的第一段长度为 u,

  总共只有两种匹配方案:
    (1)s1和s2的第一,二段分别匹配: f[i][j][k] = f[i][j][u] && f[i+u][j+u][k-u]
         第一段                     第二段
         s1[i ~ i+u-1]             s1[i+u ~ i+k-1]
         s2[j ~ j+u-1]             s2[j+u ~ j+k-1]
    (2)s1和s2的第一,二段交叉匹配: f[i][j][k] = f[i][j+k-u][u] && f[i+u][j][k-u]
         第一段                     第二段
         s1[i ~ i+u-1]             s1[i+u ~ i+k-1]
         s2[j ~ j+k-u]             s2[j+k-u ~ j+k-1]
状态总数: O(n^3), 状态转移数量: O(n), time complexity: O(n^4)

```C++
class Solution{
public:
  bool isScramble(string s1, string s2){
    int n = s1.size();
    vector<vector<vector<bool>>> f(n, vector<vector<bool>>(n, vector<bool>(n+1)))
    for(int k=1; k<=n; k++){
      for(int i=0; i+k-1<n; i++){
        for(int j=0; j+k-1<n; j++){
          if(k==1){
            if(s1[i] == s2[j]) f[i][j][k] = true;
          }
          else{
            for(int u=1; u<k; u++){
              if(f[i][j][u] && f[i+u][j+u][k-u] || f[i][j+k-u][u] && f[i+u][j][k-u]){
                f[i][j][k] = true;
                break;
              }
            }
          }
        }
      }
    }
    return f[0][0][n];
  }
};
```
 
