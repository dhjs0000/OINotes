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
bool vis[MAXN];        // vis[i]标记点i是否已在队列中
void spfa(int s) {
    memset(dis,0x3f,sizeof(dis)); // 初始化dis为最大值
    dis[s]=0;                     // 从s到s的最短路径权之和
    queue<int> Q;
    Q.push(s);  // 一开始只有s被更新，入队
    vis[s]=1;   // 标记s已在队列中
    while(!Q.empty()) {           // 只要队列不为空就继续
        int u = Q.front(); Q.pop();
        vis[u]=0;  // 出队后取消标记，方便以后再次入队
        for(const auto &e : G[u]) { // 遍历从u出发的每一条边
            int v = e.v;
            int w = e.w;
            if (dis[v] > dis[u] + w) { // 松弛操作
                dis[v] = dis[u] + w;
                if(!vis[v]) {     // 没在队列里才入队，避免重复
                    vis[v]=1;
                    Q.push(v);
                }
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
    spfa(s);
    for(int i=1;i<=n;i++)
        cout << (dis[i]==INF ? INT_MAX : dis[i]) << ' '; // 题目要求无解输出INT_MAX
}
```
[[推论-SPFA最短路]]