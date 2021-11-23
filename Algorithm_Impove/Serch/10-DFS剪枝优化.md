[TOC]

#### DFS剪枝概述

剪枝，就是减少搜索树的规模，尽早排除搜索树中不必要的分支的一种手段，形象地看，就好像剪掉了搜索树的枝条，故被称为“剪枝”。在 DFS 问题中，有以下几类剪枝方法。

![](image/Jianzhi_1.png)

![](image/Jianzhi_2.png)

优化搜索顺序

![](image/JZ_1.png)

```
优选选择分支较少的路径进行搜索，可能会提前找到结果
```



#### 165：小猫爬山

https://www.acwing.com/problem/content/167/

对当前的小猫来说：

1：放在当前已有的缆车中

2：再租一辆缆车将其放到其中

所以我们实时关心的状态有：已经运送了多少只小猫，已经租用的缆车有多少，每辆缆车上搭载的小猫重量之和。

**优化搜索顺序：** 体重较大的小猫会使得当前缆车可以搭载的小猫数量更少，也即当前 节点下的分支就会更少。就达到一个剪枝过程。所以将小猫的体重从大到小排序即可。

```c++
#include<iostream>
#include<algorithm>
#include<cstring>
using namespace std;

const int N = 20;
int n ,m;
int w[N];
int sum[N]; //  表示每一辆缆车上已有的小猫的重量 
int ans = N;

// 处理 第 u 只小猫，k 表示已经租用的缆车的数量
void dfs(int u , int k) 
{
    // 最优性剪枝
    if(k >= ans) return;
    if(u == n)
    {
        ans = k;
        return;
    }
    //  将第u只小猫放在已有的缆车上
    for(int i = 1 ; i <= k ; i++)
    {
        if(sum[i]  + w[u] <= m) //  可行性剪枝
        {
            sum[i] += w[u];
            dfs(u + 1 , k);
            sum[i] -= w[u];   //  恢复现场
        }
    }
    
    //  新租缆车
    sum[k + 1] = w[u];
    dfs(u + 1 , k + 1);
    sum[k ] = 0;
}

int main()
{
    scanf("%d%d", &n, &m);
    for(int i = 0 ; i < n ; i++) scanf("%d", &w[i]);
    
    //  优化搜索顺序
    sort(w , w + n);
    reverse(w , w + n);
    
    dfs(0,0);
    
    cout << ans << endl;
    
    return 0;
}
```



#### 166：数独

https://www.acwing.com/problem/content/168/

```
在数独中，我们需要 选择格子并对其进行填数。
1：选择分支数最少的格子  （优化搜索顺序）
2：行，列，九宫格中1-9的数字不重不漏。 （可行性剪枝）
	这里可以使用位运算进行 判定。     （位运算优化）
	行，列，九宫格，都可以使用 01 串来表示对应位置是否

位运算优化：
1：	可以使用一个二进制数来表示当前行的状态：
eg:      1 2 3 4 5 6 7 8 9
		 0 1 0 0 1 0 1 0 1    --> 二进制数
	表示其中 2 5 7 9 这几个数可以使用。
推广到	 列 和 九宫格中 , 都使用 二进制数来表示 当前状态中 1 - 9 中数字的使用。
并且因为我们 要同时满足 行 ，列 ， 九宫格，对表示其的三个 二进制数进行 与 运算。

2： 找 1 操作
使用二进制数进行表示的时候，我们需要寻找 其中 1 的个数来确定 这个数是否被使用，、
所以要用到  lowbit()  运算。来查找 当前二进制数中1个的个数。
这样就不会循环 9 次来寻找，只需要循环 1 的个数次即可.
```

```c++
#include<iostream>
#include<cstring>
#include<algorithm>
using namespace std;

const int N = 9, M = 1 << N;

// ones[M] 表示一个数的二进制表示中有多少个 1
// map 表示 以 2 为底 i 的对数 是 map[i];
int ones[M] , map[M]; 

int row[N] , col[N] , cell[3][3];
char str[100];

void init() //  预处理 行，列 ，九宫格
{
    //  (1 << N) - 1 = 511, 即 9 个位置 都是 1
    for(int i = 0 ; i < N ; i++) row[i] = col[i] = (1 << N) - 1;
    for(int i = 0 ; i < 3 ; i++)
        for(int j = 0 ; j < 3 ; j++)
            cell[i][j] = (1 << N) - 1;
}
//在 x,y 这个位置填 t，is_ste == true 是填数， is_set == false 是恢复现场，即删数
void draw(int x,int y, int t,bool is_set)
{
    //  转过来的 二维坐标，要存到一维数组， x * N + y 
    if(is_set) str[x * N + y] = '1' + t;
    else str[x * N + y] = '.';
    
    int v = 1 << t;
    if(!is_set) v = -v;
    
    row[x] -= v;
    col[y] -= v;
    cell[x / 3][y / 3] -= v;
}

int lowbit(int x)
{
    return x & -x;
}
int get(int x, int y)
{
    return row[x] & col[y] & cell[x/3][y/3];
}

bool dfs(int cnt)
{
    if(!cnt) return true;
    int minv = 10;
    int x,y;
    for(int i = 0 ; i < N ; i++)
        for(int j = 0 ; j < N ; j++)
            if(str[i * N + j] == '.')
            {
                int state = get(i,j);
                if(ones[state] < minv)
                {
                    minv = ones[state];
                    x = i, y = j;
                }
            }
    
    int state = get(x, y);  //  枚举当前状态中所有的 1
    for(int i = state ; i ; i -= lowbit(i))
    {
        int t = map[lowbit(i)];  //  将需要填的数处理出来（
        draw(x , y , t ,true);
        if(dfs(cnt - 1)) return true;
        draw(x, y , t, false);
    }
    return false;
}


int main()
{
    //  初始化 ones[]  和  map[]
    for(int i = 0 ; i < N ; i++) map[1 << i] = i;
   	
    for(int i = 0 ; i < 1 << N ; i++)  
        for(int j = 0 ; j < N ; j++)
            ones[i] += i >> j & 1;
    
    while(cin >> str , str[0] != 'e')
    {
        init();
        
        int cnt = 0; //  . 数
        for(int i = 0 , k = 0 ; i < N ; i++)
            for(int j = 0 ; j < N ; j++, k ++)
                if(str[k] != '.')
                {
                    int t = str[k] - '1';
                    draw(i ,j , t , true);
                }
                else cnt ++;
                
        dfs(cnt);
        puts(str);
    }
    
    return 0;
}

```



#### 167：木棒

https://www.acwing.com/problem/content/169/

```


```



#### 168：生日蛋糕

https://www.acwing.com/activity/content/problem/content/1488/

```


```



