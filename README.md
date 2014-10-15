Test
====
#include<iostream>
#define MAX 110
#define inf 100000
using namespace std;
int m, n, a[MAX][MAX], path[MAX][MAX], MIN, ans[MAX]; 

void Floyd(){
	int i, j, k, dist[MAX][MAX];
	for(i = 1; i <= m; i++)
		for(j = 1; j <= m; j++)
			dist[i][j] = a[i][j];
		
	MIN = inf;
	int ant = 0;
	for(k = 1; k <= m; k++){
		for(i = 1; i <= k; i++)
			for(j = 1; j < i; j++){
				if(MIN > a[i][k] + a[j][k] + dist[i][j]){
					MIN = a[i][k] + a[j][k] + dist[i][j];
					int mid = j;						// 保存最小环路径 
					ant = 0;
					while(mid != i){
						ans[ant++] = mid;
						mid = path[i][mid];
					}
					ans[ant++] = i;
					ans[ant++] = k;
					
				}
			}
		for(i = 1; i <= m; i++)
			for(j = 1; j <= m; j++){
				if(dist[i][j] > dist[i][k] + dist[k][j]){
					dist[i][j] = dist[i][k] + dist[k][j];
					path[i][j] = path[k][j];		// 保存节点 
					
				}
			}		
	} 
	
	
	
	
	if(MIN != inf){
		for(i = 0; i < ant - 1; i++)
			cout<<ans[i]<<" ";
		cout<<ans[ant - 1]<<endl;
		return;
	}
	cout<<"No solution."<<endl;
		
}
int main(){
	while(cin>>m>>n){
		
		if(m == 0 || n == 0){
			cout<<"No solution."<<endl;
			continue;
		}
		int i, j;
		for(i = 1; i <= m; i++){
			for(j = 1; j <= m; j++){
				a[i][j] = inf;
				path[i][j] = i;
			}
				
			path[i][i] = 0;
		}
			
		for(i = 1; i <= n; i++){
			int p, q, l;
			cin>>p>>q>>l;
			if(a[p][q] > l)				// 去除重边，保留最小值 
				a[p][q] = l;
			if(a[q][p] > l)
				a[q][p] = l;
		}
		Floyd();
	}
	return 0;
	
}
