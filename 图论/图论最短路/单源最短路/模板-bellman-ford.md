```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6 + 7;
const int INF = 1e9;
int dis[maxn];
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
void bellman_ford()
{
	for (int i = 1; i <= n; i++)
	{
		dis[i] = INF;
	}
	dis[s] = 0;
	for (int i = 1; i <= n - 1; i++) // 一条路径最多添加n - 1条
	{
		bool flag = 0;
		for (int j = 1; j <= n; j++) // 枚举所有起点 
		{
			for (int k = 0; k < G[j].size(); k++) // 枚举起点能到的点，对应边的终点 
			{
				int v = G[j][k].v;
				int w = G[j][k].w;
				if (dis[v] > dis[j] + w)
				{
					dis[v] = dis[j] + w;
					flag = 1;
				}
			}
		}
		if (flag == 0) // 说明没有路径被松弛
		{
			return ;
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
	bellman_ford();
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
[[推论-Bellman-ford最短路]]