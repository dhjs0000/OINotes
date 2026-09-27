```cpp
#include <bits/stdc++.h>
using namespace std;
typedef long long ll;
const ll INF = (ll)4e18;          // 距离数组用的大 INF
const ll OUT = 1000000000LL;      // 题目要求输出的"不可达"值 1e9

int n, m;
struct Edge { int u, v; ll w; };
vector<Edge> edges;               // 存所有边（含虚拟边）
vector<vector<pair<int,ll>>> g;   // 重构后的邻接表，跑 Dijkstra 用

int main() {
    scanf("%d %d", &n, &m);
    edges.reserve(m + n);
    g.assign(n + 1, {});
    for (int i = 0; i < m; i++) {
        int u, v; ll w;
        scanf("%d %d %lld", &u, &v, &w);
        edges.push_back({u, v, w});
    }
    // 虚拟源点 0：向每个点连权 0 的边
    for (int i = 1; i <= n; i++) edges.push_back({0, i, 0});

    // ---------- 第一步：Bellman-Ford 求 h，判负环 ----------
    vector<ll> h(n + 1, 0);        // 源点是 0，初始全 0 即可
    bool updated = false;
    for (int i = 1; i <= n; i++) {          // 最多松弛 n 轮
        updated = false;
        for (auto &e : edges)
            if (h[e.u] + e.w < h[e.v]) {
                h[e.v] = h[e.u] + e.w;
                updated = true;
            }
        if (!updated) break;                // 提前收敛
    }
    if (updated) { printf("-1\n"); return 0; }  // 第 n 轮仍能松弛 → 负环

    // ---------- 第二步：势能重构，边权全部非负 ----------
    for (auto &e : edges)
        if (e.u != 0) {                     // 跳过虚拟边
            ll nw = e.w + h[e.u] - h[e.v];
            g[e.u].push_back({e.v, nw});
        }

    // ---------- 第三步：n 轮 Dijkstra ----------
    vector<ll> dis(n + 1);
    for (int s = 1; s <= n; s++) {
        fill(dis.begin(), dis.end(), INF);
        dis[s] = 0;
        priority_queue<pair<ll,int>, vector<pair<ll,int>>, greater<>> pq;
        pq.push({0, s});
        while (!pq.empty()) {
            auto [d, u] = pq.top(); pq.pop();
            if (d > dis[u]) continue;       // 懒删除
            for (auto [v, w] : g[u])
                if (dis[u] + w < dis[v]) {
                    dis[v] = dis[u] + w;
                    pq.push({dis[v], v});
                }
        }
        // ---------- 第四步：还原权值并输出 ----------
        ll ans = 0;
        for (int t = 1; t <= n; t++) {
            ll d = (dis[t] == INF) ? OUT : dis[t] - h[s] + h[t];
            // i == j 时 d 自然为 0，无需特判
            ans += (ll)t * d;
        }
        printf("%lld\n", ans);
    }
    return 0;
}
```
[[推论-Johnson算法]]