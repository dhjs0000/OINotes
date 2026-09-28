```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll INF = 0x3f3f3f3f3f3f3f3f; // 最大值
const int MAXN = 3005;             // 数据范围: 最大n
const ll OUT = 1000000000LL;       // 题目要求输出的"不可达"值 1e9

int n,m;
struct Edge {  // 边结构体
    int u,v;   // u:起点，v:目标点（邻接表里u没有意义）
    ll w;      // w:边权
};
struct Node {    // 点结构体（Dijkstra的优先队列用）
    int v; ll w; // v:目标点，w:当前到该点的最短路估计值
    bool operator <(const Node &a) const {
        return w > a.w; // 使得priority_queue反转变为小根堆
    }
};

vector<Edge> edges;   // 边表（Bellman-Ford用，含虚拟源点的边）
vector<Edge> G[MAXN]; // 邻接表（重构后的新图，Dijkstra用）
ll h[MAXN];           // h[i]：势函数，即虚拟源点到i的最短路长度
ll dis[MAXN];         // 定义dis[i]为从s到i的最短路径权之和
bool vis[MAXN];       // vis[i]是否已经被访问过

void bellman_ford(int s) {    // 以s为源点跑Bellman-Ford，顺便判负环
    memset(h,0x3f,sizeof(h)); // 初始化h为最大值
    h[s]=0;                   // 从s到s的最短路径权之和
    for(int i=1;i<=n;i++) {   // 一共n+1个点，一条最短路最多n条边，松弛n轮
        bool flag=0;          // 记录本轮是否发生过松弛
        for(const auto &e : edges) { // 枚举所有边
            int u=e.u, v=e.v;
            ll w=e.w;
            if (h[u]==INF) continue;   // 还没到达的点无法继续松弛，跳过
            if (h[v] > h[u] + w) {
                h[v] = h[u] + w;
                flag = 1; // 标记本轮发生了松弛
            }
        }
        if(!flag) return; // 一整轮都没有松弛发生，说明已经收敛，提前退出
        if(i==n) { printf("-1\n"); exit(0); } // 第n轮还能松弛，说明存在负环
    }
}

void dijkstra(int s) {
    memset(dis,0x3f,sizeof(dis)); // 初始化dis为最大值
    memset(vis,0,sizeof(vis));    // 初始化vis数组（在多次调用dijkstra时很有用）
    dis[s]=0;                     // 从s到s的最短路径权之和
    priority_queue<Node> q;       // 优先队列（在函数内定义的话，在多次调用dijkstra时很有用）
    q.push({s, 0});               // 将起点推入优先队列
    while(!q.empty()) {           // 一直持续运行到堆变空
        int u = q.top().v; q.pop(); // 取出队首
        if (vis[u]) continue;       // 如果已经访问过就跳过
        vis[u] = 1;                 // 设置当前点已经访问过了
        for(const auto &e : G[u]) { // 遍历从u开始的每一条边
            int v = e.v;
            ll w = e.w;
            if (dis[v] > dis[u] + w) {
                dis[v] = dis[u] + w;
                q.push({v,dis[v]}); // 将下一条边加入队列
            }
        }
    }
}

int main() {
    scanf("%d %d", &n, &m);
    edges.reserve(m + n + 5);
    for (int i = 0; i < m; i++) {
        int u,v; ll w;
        scanf("%d %d %lld", &u, &v, &w);
        edges.push_back({u, v, w});
    }
    // 虚拟源点0：向每个点连权0的边，把多源最短路变成单源最短路
    for (int i = 1; i <= n; i++) edges.push_back({0, i, 0});

    // ---------- 第一步：Bellman-Ford求势函数h，判负环 ----------
    bellman_ford(0);

    // ---------- 第二步：势能重构，新边权w'=w+h[u]-h[v]，全部非负 ----------
    for (const auto &e : edges) {
        if (e.u == 0) continue; // 跳过虚拟源点的边
        ll nw = e.w + h[e.u] - h[e.v];
        G[e.u].push_back({e.u, e.v, nw}); // 建重构后的邻接表
    }

    // ---------- 第三步：以每个点为源点跑n轮Dijkstra ----------
    for (int s = 1; s <= n; s++) {
        dijkstra(s);
        // ---------- 第四步：还原真实权值并统计答案 ----------
        ll ans = 0;
        for (int t = 1; t <= n; t++) {
            ll d = (dis[t]==INF) ? OUT : dis[t] - h[s] + h[t]; // 还原: d = d' - h[s] + h[t]
            // s==t时d自然为0，无需特判
            ans += (ll)t * d;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```
[[推论-Johnson算法]]