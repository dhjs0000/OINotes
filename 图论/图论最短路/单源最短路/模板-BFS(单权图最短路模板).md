```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 1e6+5;     // 数据范围: 最大n
int n,m,s;
vector<int> G[MAXN];   // 邻接表（BFS求边数最短路，不需要边权）
int dis[MAXN];         // 定义dis[i]为从s到i的最短边数
bool vis[MAXN];        // 标记点是否入过队，防止重复入队

void bfs(int s) {
    memset(dis,0x3f,sizeof(dis)); // 初始化dis为最大值
    dis[s]=0;                     // 从s到s的距离为0
    vis[s]=1;                     // 起点先入队标记
    queue<int> Q;                 // 广度优先搜索的队列
    Q.push(s);                    // 起点入队
    while(Q.size()) {             // 队列非空就继续
        int u = Q.front(); Q.pop(); // 队首出队，开始扩展它
        for(const auto &v : G[u]) { // 遍历从u出发的每一条边
            if(vis[v]) continue;    // 已经入过队的点不重复扩展，保证每层只访问一次
            dis[v] = dis[u] + 1;    // u到v恰好一条边，步数+1
            vis[v] = 1;             // 标记已访问
            Q.push(v);              // 新点入队，等待下一轮扩展
        }
    }
}

int main(){
    cin>>n>>m>>s;
    for(int i=1;i<=m;i++){
        int u,v;
        cin>>u>>v;
        G[u].push_back(v);
    }
    bfs(s);
    for(int i=1;i<=n;i++)
        cout << (dis[i]==0x3f3f3f3f ? -1 : dis[i]) << ' '; // 不可达输出-1
}
```
[[推论-BFS最短路]]