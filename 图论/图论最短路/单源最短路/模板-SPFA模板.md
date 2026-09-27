```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6 + 7;
const int INF = 1e9;
int dis[maxn];
bool vis[maxn]; 
struct Edge{
	int v; // 终点 
	int w; // 边权 
};
vector<Edge> G[maxn];
int n, m, s;
void add_edge(int u, int v, int w)
{
	G[u].push_back({v, w});
	return ;
} 
void spfa()
{
	for (int i = 1; i <= n; i++)
	{
		dis[i] = INF;
	}
	dis[s] = 0;
	queue<int> Q;
	Q.push(s); // 一开始只有s被更新了
	vis[s] = 1;
	while (!Q.empty()) 
	{
		int u = Q.front();
		Q.pop();
		vis[u] = 0; // 取消标记 
		for (int i = 0; i < G[u].size(); i++)
		{
			int v = G[u][i].v;
			int w = G[u][i].w;
			if (dis[v] > dis[u] + w)
			{
				dis[v] = dis[u] + w; // 
				if (vis[v] == 0)
				{
					vis[v] = 1; 
					Q.push(v); // 在队列之中，无需入队 修改dis 
				} 
			}
		}
	}
	return ;	
}

int main()
{
	cin >> n >> m >> s;
	for (int i = 1; i <= m; i++)
	{
		int u, v, w;
		cin >> u >> v >> w;
		add_edge(u, v, w); 
	}
    spfa(); 
	for (int i = 1; i <= n; i++)
	{
		if (dis[i] >= INF)
		{
			cout << (1 << 31) - 1 << ' ';
		}
		else cout << dis[i] << ' ';
	}
	return 0;
}
```
[[推论-SPFA最短路]]