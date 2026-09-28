```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f; // 最大值
const int MAXN = 1e6+5;     // 数据范围: 最大n
int n,m,s;
struct Edge { // 边结构体
    int v, w; // v:目标点，w:边权
};
vector<Edge> G[MAXN];  // 邻接表
int dis[MAXN];         // 定义dis[i]为从s到i的最短路径权之和

void bellman_ford(int s) {
    memset(dis,0x3f,sizeof(dis)); // 初始化dis为最大值
    dis[s]=0;                     // 从s到s的最短路径权之和
    for(int i=1;i<=n-1;i++) { // 一条最短路径最多包含n-1条边，松弛n-1轮就够了
        bool flag=0;          // 记录本轮是否发生过松弛
        for(int u=1;u<=n;u++) { // 枚举所有起点
            if(dis[u]==INF) continue;   // 还没到达的点无法继续松弛，跳过
            for(const auto &e : G[u]) { // 遍历从u出发的每一条边
                int v = e.v;
                int w = e.w;
                if (dis[v] > dis[u] + w) {
                    dis[v] = dis[u] + w;
                    flag = 1; // 标记本轮发生了松弛
                }
            }
        }
        if(!flag) return; // 一整轮都没有松弛发生，说明已经收敛，提前退出
    }
}

int main(){
    cin>>n>>m>>s;
    for(int i=1;i<=m;i++){
        int u,v,w;
        cin>>u>>v>>w;
        G[u].push_back({v,w}); // vector邻接表存图
    }
    bellman_ford(s);
    for(int i=1;i<=n;i++)
        cout << (dis[i]==INF ? INT_MAX : dis[i]) << ' '; // 题目要求无解输出INT_MAX
}
```
[[推论-Bellman-ford最短路]]