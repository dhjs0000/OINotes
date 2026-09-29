```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f; // 最大值
const int MAXN = 1e6+5;     // 数据范围: 最大n
int n,m,s;
struct Edge { // 边结构体
    int v, w; // v:目标点，w:边权
    bool operator <(const Edge &a) const {
        return w > a.w; // 使得priority_queue反转变为小根堆
    }
};
vector<Edge> G[MAXN];  // 邻接表
int dis[MAXN];         // 定义dis[i]为从s到i的最短路径权之和
bool vis[MAXN];        // vis[i]是否已经被访问过

void dijkstra(int s) {
    memset(dis,0x3f,sizeof(dis)); // 初始化dis为最大值
    memset(vis,0,sizeof(vis));    // 初始化vis数组（在多次调用dijkstra时很有用）
    dis[s]=0;              // 从s到s的最短路径权之和
    priority_queue<Edge> q;// 优先队列（在函数内定义的话，在多次调用dijkstra时很有用）
    q.push({s, 0});        // 将起点推入优先队列
    while(!q.empty()) {    // 一直持续运行到堆变空
        int u = q.top().v; q.pop(); // 取出队首
        if (vis[u]) continue;       // 如果已经访问过就跳过
        vis[u] = 1;                 // 设置当前点已经访问过了
        for(const auto &e : G[u]) { // 遍历从u开始的每一条边
            int v = e.v;
            int w = e.w;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                q.push({v,dis[v]}); // 将下一条边加入队列
            }
        }
    }
}

int main(){
    cin>>n>>m>>s;
    for(int i=1;i<=m;i++){
        int u,v,w;
        cin>>u>>v>>w;
        G[u].push_back({v,w}); // vector邻接表存图
    }
    dijkstra(s);
    for(int i=1;i<=n;i++)
        cout << (dis[i]==INF ? INT_MAX : dis[i]) << ' '; // 题目要求无解输出INT_MAX
}

```
[[推论-dijkstra最短路]]
