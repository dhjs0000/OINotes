```cpp
#include <bits/stdc++.h>
using namespace std;
const int maxn = 1e6 + 7;
int n, m, s;
struct Node // w最小的，返回所在的v 
{
	int v, w;
	// 手写比较器
	bool operator <(const Node &A) const   // 从大到小排列 （优先队列--最后一个） 
	{
		// w左边那个   A.w 右边的那个w 
		return w > A.w;	
	} 
};

vector<Node> G[maxn];
int dis[maxn]; // dis[i]表示起点s到i的最短距离 
bool vis[maxn]; // 标记哪些点已经选中了，作为永久点vis[i] =1 说明i被选中了 
void add_edge(int u, int v, int w)
{
	G[u].push_back({v, w});
}

priority_queue<Node> Q; 
void dijkstra(int s)
{
	for (int i = 1; i <= n; i++)
	{
		dis[i] = INT_MAX;	
	} 
	dis[s] = 0;
	Q.push({s, dis[s]});
	while (!Q.empty())
	{
		int id = Q.top().v; 
		Q.pop();
		if (vis[id] == 1) continue; // 不允许再更新 
		vis[id] = 1;// 设置为永久点
		for (int i = 0; i < G[id].size(); i++) 
		{
			int v = G[id][i].v;
			int w = G[id][i].w;
			if (dis[v] > dis[id] + w)
			{
				dis[v] = dis[id] + w;
				Q.push({v, dis[v]});
			}
		}
	}
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
	dijkstra(s);
	for (int i = 1; i <= n; i++) // 不能初始值为0 
	{
		cout << dis[i] << ' '; 
	}
	return 0;
}
```
[[推论-dijkstra最短路]]