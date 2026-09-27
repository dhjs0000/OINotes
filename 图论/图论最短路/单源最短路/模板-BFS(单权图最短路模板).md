```cpp
void add_edge(int u, int v)
{
	G[u].push_back(v);
	return ;
} 
bool vis[maxn];
int dis[maxn]; // 记录边数 
void bfs()
{
	queue<int>Q;
	Q.push(s);
	while (Q.size())
	{
		int u = Q.front();
		Q.pop();
		for (int i = 0; i < G[u].size(); i++)
	    {
	        int v = G[u][i];
	        if (vis[v] == 0)
	        {
	        	dis[v] = dis[u] + 1; // 步数+1
				vis[v] = 1;
				Q.push(v); 
			}
	    }
	}
	return ;
}
```
[[推论-BFS最短路]]