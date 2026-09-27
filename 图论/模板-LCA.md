```cpp
#include <bits/stdc++.h>
using namespace std;
const int MAXN = 500005;
const int LOG = 20;
int n, m, s;
vector<int> g[MAXN];
int dep[MAXN];
int up[LOG][MAXN];
void dfs(int u, int fa) {
    up[0][u] = fa;
    dep[u] = dep[fa] + 1;
    for (int k = 1; k < LOG; k++)
        up[k][u] = up[k-1][ up[k-1][u] ];
    for (int v : g[u]) {
        if (v == fa) continue;
        dfs(v, u);
    }
}
int lca(int u, int v) {
    if (dep[u] < dep[v]) swap(u, v);
    int d = dep[u] - dep[v];
    for (int k = LOG - 1; k >= 0; k--)
        if (d >> k & 1)
            u = up[k][u];
    if (u == v) return u;
    for (int k = LOG - 1; k >= 0; k--) {
        if (up[k][u] != up[k][v]) {
            u = up[k][u];
            v = up[k][v];
        }
    }
    return up[0][u];
}
int main() {
    cin>>n>>m>>s;
    for (int i = 1; i < n; i++) {
        int u, v; cin>>u>>v;
        g[u].push_back(v);
        g[v].push_back(u);
    }
    dep[s] = 0;
    dfs(s, s);
    while (m--) {
        int u, v; cin>>u>>v;
        cout<<lca(u, v)<<'\n';
    }
    return 0;
}
```
求深度：
DFS:
```cpp
dep[s] = 0;

void dfs(int u, int fa) {
    up[0][u] = fa;
    dep[u] = dep[fa] + 1;
    for (int k = 1; k < LOG; k++)
        up[k][u] = up[k-1][ up[k-1][u] ];
    for (int v : g[u]) {
        if (v == fa) continue;
        dfs(v, u);
    }
}
```
BFS:
```cpp
void bfs(int root) {
    queue<int> q;
    depth[root] = 0;
    q.push(root);
    while (!q.empty()) {
        int u = q.front(); q.pop();
        for (int i = head[u]; i; i = nxt[i]) {
            int v = to[i];
            if (v == fa[u]) continue;      // 跳过父亲，防止回头
            depth[v] = depth[u] + 1;
            fa[v] = u;                     // 顺手还能把父亲表也求了
            q.push(v);
        }
    }
}
```
核心LCA:
```cpp
int lca(int u, int v) {
    if (dep[u] < dep[v]) swap(u, v);
    int d = dep[u] - dep[v];
    for (int k = LOG - 1; k >= 0; k--)
        if (d >> k & 1)
            u = up[k][u];
    if (u == v) return u;
    for (int k = LOG - 1; k >= 0; k--) {
        if (up[k][u] != up[k][v]) {
            u = up[k][u];
            v = up[k][v];
        }
    }
    return up[0][u];
}
```

求两点距离：
$$dist(u,v) = dep[u] + dep[v] - 2 \cdot dep[lca(u,v)]$$
即
```cpp
int dist(int u,int v) {
	return dep[u]+dep[v]-2*dep[lca(u,v)];
}
```

[[推论-LCA最近公共祖先]]