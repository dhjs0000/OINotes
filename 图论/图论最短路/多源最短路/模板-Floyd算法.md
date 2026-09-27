```cpp
#include <bits/stdc++.h>
using namespace std;
const int INF = 0x3f3f3f3f;
int e[105][105];
int main() {
	int n,m;
	cin>>n>>m;
	memset(e,INF,sizeof(e));
	for(int i=1;i<=n;i++)e[i][i]=0;
	for(int i=1;i<=m;i++){
		int u,v,w;cin>>u>>v>>w;
		e[u][v]=min(e[u][v],w);
		e[v][u]=min(e[v][u],w);
	}
	for(int k=1;k<=n;k++){
		for(int i=1;i<=n;i++){
			for(int j=1;j<=n;j++){
				e[i][j]=min(e[i][j],e[i][k]+e[k][j]);
			}
		}
	}
	for(int i=1;i<=n;i++) {
		for(int j=1;j<=n;j++) {
			if(e[i][j]==INF)cout<<'-1'<<' ';
			else cout<<e[i][j]<<' ';
		}
		cout<<'\n';
	}
}
```
[[推论-Floyd算法]]