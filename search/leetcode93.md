#### 简单的暴搜，从前往后一个一个搜，每个数：
1. 不能有前导0， e.g. 011 (illegal)
2. 必须在0~255之间

```
class Solution {
public:
    vector<string> ans; //保存所有合法的IP Address
    vector<string> restoreIpAddresses(string s) {
        dfs(s, 0, 0, ""); //从s的第0位，第0个数开始搜索，当前的方案为空
        return ans;
    }

    void dfs(string& s, int u, int k, string path){ //u表示当前搜到s的第几位了，k表示当前搜到第几个数了，path用于存储方案
        if(u==s.size()){
            //s已经搜完了
            if(k==4){
                path.pop_back(); //将最后一个'.'去掉
                ans.push_back(path);//并且正好由4个数字构成
            }
            return;
        }
        //加一个剪枝,s还没搜完，但已经凑齐了4个数字, 肯定不行, 没必要再往下搜了 
        if(k==4) return;
        int t=0; //当前搜到的数
        for(int i=u; i<s.size(); i++)
        {
            if(i>u && s[u]=='0')break; //有前导0
            t = t*10 + s[i] - '0';
            if(t <= 255)dfs(s, i+1, k+1, path + to_string(t) + '.');
            else break;

        }

    }
};
```
