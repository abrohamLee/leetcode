#### 本质上就是求一个欧拉路径
> 欧拉路径: 如果在一张图中，可以从一点出发遍历所有的边，每条边只能遍历一次，那么遍历过程中的这条路径就叫做欧拉路径

> 那么什么时候能保证欧拉路径存在呢？
>   1. 有向图是连通的
>   2. 除起点和终点外的所有点，出度=入度
>   3. 起点的出度 = 入度 + 1
>   4. 终点的入度 = 出度 + 1

> llema1 : 从起点开始遍历dfs, 遍历完后的回溯序列 就是 欧拉路径的反序列。 (证明略)
> 
> llema2 : 为了保证字典序，我们只需要先搜索字典序更小的节点。(此处容易证明)

```
class Solution {
public:
    unordered_map<string, multiset<string>> g; //建图方式：g=[A点][所有与A点相邻的点组成的集合](其实就是领接表啦)
    vector<string> ans; //用于存放欧拉路径
    vector<string> findItinerary(vector<vector<string>>& tickets) {
        //建图
        for(auto& e : tickets)g[e[0]].insert(e[1]);
        //从起点开始搜索
        dfs("JFK");
        //欧拉路径是回溯路径的逆序
        reverse(ans.begin(), ans.end());
        return ans; //返回结果
    }
    void dfs(string u) //当前已经搜到u点
    {
        while(g[u].size()) //遍历u点的邻边
        {
            auto ver = *g[u].begin(); //从集合中的第一个点开始遍历
            g[u].erase(g[u].begin()); //已遍历的边直接删掉，防止重复经过此边
            dfs(ver);
        }
        ans.push_back(u); // 当前的路径经过u点

    }
};
```
