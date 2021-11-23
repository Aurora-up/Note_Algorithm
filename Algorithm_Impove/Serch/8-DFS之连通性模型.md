[TOC]

在连通性问题当中， BFS 和 DFS 都可以用来搜索连通块，对于 DFS 来说，有**局限性**：

1. DFS 只能判断是否连通
2. DFS 搜索的路径不一定是最短路
2. 不能求最短路
2. 会有爆栈的分险

**是否需要恢复现场**  

如果是棋盘这种整体作为状态时，才需要恢复现场。如果是每个格子作为状态时，不需要恢复现场。

#### 1112：迷宫

https://www.acwing.com/problem/content/description/1114/

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 110;
int n;
char g[N][N];
bool st[N][N];
int xa, ya, xb, yb;

int dx[4] = {-1, 0, 1, 0}, dy[4] = {0, 1, 0, -1};

bool dfs(int x ,int y)
{
    if(g[x][y] == '#') return false;
    if(x == xb && y == yb) return true;
    
    st[x][y] = true;
    
    for(int i = 0 ; i < 4 ; i++)
    {
        int a = x + dx[i] , b = y + dy[i];
        if(a < 0 || a >= n || b < 0 || b >= n) continue;
        if(st[a][b]) continue;
        
        if(dfs(a , b)) return true;
    }
    return false;
}


int main()
{
    int T;
    cin >> T;
    while(T--)
    {
        cin >> n;
        for(int i = 0 ; i < n; i++) cin >> g[i];
        memset(st , 0 , sizeof st);
        
        cin >> xa >> ya >> xb >> yb;
        if(dfs(xa , ya)) puts("YES");
        else puts("NO");
    }
    
    return 0;
}
```

#### 1113：红与黑

https://www.acwing.com/problem/content/1115/

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 25;

int n, m;
char g[N][N];
bool st[N][N];
int dx[4] = {-1, 0 ,1 , 0}, dy[4] = {0, 1, 0 ,-1};

int dfs(int x,int y)
{
    int cnt = 1;
    st[x][y] = true;
    for(int i = 0 ; i < 4 ; i++)
    {
        int a = x + dx[i] , b = y + dy[i];
        if(a < 0 || a >= n || b < 0 || b >= m) continue;
        if(g[a][b] != '.') continue;
        if(st[a][b]) continue;
        
        cnt += dfs(a,b);
    }
    return cnt;
}

int main()
{
    while(cin >> m >> n , n || m)
    {
        for(int i = 0 ; i < n ; i++) cin >> g[i];
        
        int x ,y;  //  x
        for(int i = 0 ; i < n ; i++)
            for(int j = 0 ; j < m ; j++)
                if(g[i][j] == '@')
                {
                    x = i , y = j;
                }
            
        memset(st , 0 ,sizeof st);
        cout << dfs(x, y) << endl;
    }
    
    return 0;
}
```

