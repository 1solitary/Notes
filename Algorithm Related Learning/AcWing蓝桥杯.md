### 由数据范围反推算法复杂度
[![7FxJEj.png](https://s4.ax1x.com/2022/01/09/7FxJEj.png)](https://imgtu.com/i/7FxJEj) 

### 目录

- [Ⅰ. 递归与递推](#__4)
- - - [92\. 递归实现指数型枚举（普通版，二进制优化）](#92__5)
    - [94\. 递归实现排列型枚举](#94__65)
    - [717\. 简单斐波那契数列（普通版，空间优化）](#717__110)
    - [95\. 费解的开关](#95__158)
    - [93\. 递归实现组合型枚举](#93__234)
    - [1209\. 带分数](#1209__277)
    - [116\. 飞行员兄弟](#116__348)
    - [1208\. 翻硬币](#1208__411)
- [Ⅱ. 二分与前缀和](#__446)
- - - [789\. 数的范围](#789__447)
    - [790\. 数的三次方根](#790__494)
    - [795\. 前缀和](#795__523)
    - [730\. 机器人跳跃问题](#730__557)
    - [1221\. 四平方和](#1221__602)
    - [1227\. 分巧克力](#1227__660)
    - [99\. 激光导弹](#99__702)
    - [1230.K倍区间](#1230K_742)
- [Ⅲ . 数学与简单DP](#__DP_777)
- - - [1205\. 买不到的数目](#1205__778)
    - [1211\. 蚂蚁感冒](#1211__797)
    - [1216\. 饮料换购](#1216__829)
    - [2\. 01背包](#2_01_853)
    - [1015\. 摘花生](#1015__877)
    - [895\. 最长上升子序列](#895__907)
    - [1212\. 地宫取宝](#1212__935)

# Ⅰ. 递归与递推

### 92\. 递归实现指数型枚举（普通版，二进制优化）

题目链接：[递归实现指数型枚举](https://www.acwing.com/problem/content/94/)  
**题目描述**  
从**1 ∼ n** 这 **n** 个整数中随机选取任意多个，输出所有可能的选择方案。  
**题目分析**  
![在这里插入图片描述](https://img-blog.csdnimg.cn/5c13e40f0df640d7b6fc37f92c53814f.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWNXaW5nLWxlaW1pbmd6ZQ==,size_20,color_FFFFFF,t_70,g_se,x_16)  
**CODE（普通版）**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 20;
int st[N];//状态，记录每个位置的状态：0表示还没考虑，1表示选它，2表示不选
int n;
void dfs(int u)
{
	if (u > n)//判断边界条件
	{
		for (int i = 1; i <= n; i++)
			if (st[i])cout << i << ' ';//输出结果
		cout << endl;
		return;
	}
	st[u] = 1;
	dfs(u + 1);// 第一个分支，选择
	st[u] = 0; // 恢复现场

	st[u] = 2;
	dfs(u + 1);// 第二个分支，不选择
	st[u] = 0;// 恢复现场
}
int main()
{
	cin >> n;
	dfs(1);//从1开始，数也是从1开始
	return 0;
}
```

**CODE\(二进制优化\)**

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
void dfs(int u, int state)
{
	if (u >= n)
	{
		for (int i = 0; i < n; i++)
			if (state >> i & 1)cout << i + 1 << ' ';
		cout << endl;
		return;
	}
	dfs(u + 1, state << 1 | 1);
	dfs(u + 1, state << 1);
}
int main()
{
	cin >> n;
	dfs(0, 0);
	return 0;
}
```

### 94\. 递归实现排列型枚举

题目链接：[递归实现排列型枚举](https://www.acwing.com/problem/content/96/)  
**题目描述**  
把 1∼n 这 n 个整数排成一行后随机打乱顺序，输出所有可能的次序。  
**题目分析**  
dfs全排列问题  
图片参考：https://www.acwing.com/solution/content/44647/  
![在这里插入图片描述](https://img-blog.csdnimg.cn/025c97c3eacc458bab9d13d708ec2c85.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWNXaW5nLWxlaW1pbmd6ZQ==,size_20,color_FFFFFF,t_70,g_se,x_16)

**CODE（普通版）**

```cpp
#include <iostream>
#include <cstring>

using namespace std;

const int N = 10;
int n;
int st[N];//状态，0代表还没搜索，1-n表示位置上的值
bool used[N];//是否已经使用过

void dfs(int u){
    if(u>n){//判断边界
        for(int i = 1; i <= n; i++){
            cout << st[i] << ' ';//输出结果
        }
        puts("");//输出换行
        return;
    }
    
    for(int i = 1; i <= n; i++){
        if(!used[i]){//检查是否使用
            st[u] = i;
            used[i] = 1;//状态改为已经使用过
            dfs(u+1);//向下遍历
            
            st[u] = 0;
            used[i] = 0;//恢复现场
        }
    }
}

int main(){
    
    cin >> n;
    
    dfs(1);
    
    return 0;
}
```

### 717\. 简单斐波那契数列（普通版，空间优化）

题目链接：[简单斐波那契数列](https://www.acwing.com/problem/content/719/)  
**题目描述**  
输出前n项  
**题目分析**  
有手就行  
**CODE\(普通版\)**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 50;
int f[N];
int n;
int main()
{
	cin >> n;
	f[0] = 1;
	cout << f[1];
	for (int i = 2; i <= n; i++)
	{
		f[i] = f[i - 1] + f[i - 2];
		cout << " " << f[i];
	}
	return 0;
}
```

**CODE（空间优化）**

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
int fn,a,b;
int main()
{
    cin>>n;
    a=0,b=1;
    for(int i=1;i<=n;i++)
    {
        cout<<a<<' ';
        fn=a+b;
        a=b;
        b=fn;
    }
    return 0;
}
```

### 95\. 费解的开关

题目链接：[费解的开关](https://www.acwing.com/problem/content/97/)  
**题目描述**  
1 表示灯开着，0 表示关着的，按一个灯会影响上下左右四个格子做出应变化  
**题目分析**  
枚举第一行所有的按法，用递推下一行更新上一行，最后判断最后一行是否都亮  
**CODE**

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 6;
const int INF = 0x3f3f3f3f;
int dx[N] = { -1,0,1,0,0 }, dy[N] = { 0,1,0,-1,0 };
char g[N][N], backup[N][N];
int n;
void turn(int x, int y)
{
	for (int i = 0; i < 5; i++)
	{
		int a = x + dx[i], b = y + dy[i];
		if (a < 0 || a >= 5 || b < 0 || b >= 5)continue;
		g[a][b] ^= 1;
	}
}
int main()
{
	cin >> n;
	while (n--)
	{
		for (int i = 0; i < 5; i++)cin >> g[i];
		int res = INF;
		for (int op = 0; op < 1 << 5; op++)
		{
			memcpy(backup, g, sizeof g);
			int step = 0;
			for (int i = 0; i < 5; i++)
			{
				if (op >> i & 1)
				{
					step++;
					turn(0, i);
				}
			}
			for (int i = 0; i < 4; i++)
			{
				for (int j = 0; j < 5; j++)
				{
					if (g[i][j] == '0')
					{
						step++;
						turn(i + 1, j);
					}

				}
			}
			bool is_dark = false;
			for (int j = 0; j < 5; j++)
			{
				if (g[4][j] == '0')
				{
					is_dark = true;
					break;
				}
			}
			if (!is_dark)res = min(res, step);
			memcpy(g, backup, sizeof backup);
		}
		if (res > 6)res = -1;
		cout << res << endl;
	}
	return 0;
}
```

### 93\. 递归实现组合型枚举

题目链接：[递归实现组合型枚举](https://www.acwing.com/problem/content/95/)  
_**题目描述**_  
从**1\~n**这n个整数种随机选出m个，输出所有可能的选择方案  
_**题目分析**_  
暴力dfs  
_**CODE**_

```cpp
#include <iostream>

using namespace std;

const int N = 30;

//共需要传入三个参数 way[N], 当前处理的位置， 当前从哪个数值开始
int way[N];//每个位置上存放的值
int n, m;

void dfs(int u, int start){
	if(u + n - start < m) return;
    if(u == m+1){
        for(int i = 1; i <= m; i++) cout<<way[i]<<' ';
        cout<<endl;
    }
    
    for(int i = start; i<=n; i++){
        way[u] = i;
        dfs(u+1, i+1);
        way[u] = 0;
        
        
    }
}

int main(){
    cin >> n >> m;
    dfs(1,1);
    
    return 0;
}
```

### 1209\. 带分数

题目链接：[带分数](https://www.acwing.com/problem/content/1211/)  
_**题目描述**_  
给定一个整数，可以表示成带分数，保证这个带分数中，1\~9只出现一次  
求有多少种表示法，不包含 **0**  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 20;
typedef long long LL;
int st[N], backup[N];
int n;
int ans;
bool check(int a, int c)
{
	LL b = n * (LL)c - a * c;
	if (!a || !b || !c)return false;//不能有 0 
	memcpy(backup, st, sizeof st);
	while (b)
	{
		int x = b % 10;
		b /= 10;
		if (!x || backup[x])return false;//重复或有 0 存在
		backup[x] = true;
	}
	for (int i = 1; i <= 9; i++)
	{
		if (!backup[i])//漏选
			return false;
	}
	return true;
}
void dfs_c(int x, int a, int c)
{
	if (x >= 10)return;
	if (check(a, c))ans++;
	for (int i = 1; i <= 9; i++)
	{
		if (!st[i])
		{
			st[i] = true;
			dfs_c(x + 1, a, c * 10 + i);
			st[i] = false;
		}
	}
}
void dfs_a(int u, int a)
{
    if(u>=10)return;
	if (a >= n)return;
	if (a)dfs_c(u, a, 0);
	for (int i = 1; i <= 9; i++)
	{
		if (!st[i])
		{
			st[i] = true;
			dfs_a(u + 1, a * 10 + i);
			st[i] = false;
		}
	}
}
int main()
{
	cin >> n;
	dfs_a(0, 0);
	cout << ans << endl;
	return 0;
}
```
```c++
// 2022/1/10 错误代码，debug不出来
#include <iostream>
#include <algorithm>
#include <cstring>

using namespace std;

typedef long long LL;
const int N = 1e6+10;
bool st[N],cp[N];

int n;
int cnt;//记录可行方案数

bool check(int a, int c){

    long long b = n * (long long)c - a * c;
    
    if(!a || !b || !c) return false;//若a,b,c其中有0，返回错误
    
    memcpy(cp, st, sizeof st);//拷贝一个状态检查数组
    
    while(b){//检查c的值各位是否满足条件
        int x = b % 10;
        b /= 10;
        if(!x || cp[x]) return false;//存在0或者已经被用过返回错误
        cp[x] = true;//标记已使用
    }
    
    for(int i = 1; i <= 9; i++){
        if(!cp[i]) return false;//最终检查所有数值均已使用
    }
    
    return true;
}

void dfs_c(int u,int a,int c){//当前c的值

    if(u > 9) return;
    if(check(a, c)) cnt++;//检查b是否符合条件，与下面道理同
    
    for(int i = 1; i <= 9; i++){
        if(!st[i]){
            st[i] = true;
            dfs_c(u+1, a, c * 10 + i);
            st[i] = false;
        }
    }
        
}

void dfs_a(int u,int a){

    if(u > 9) return;
    if(a >= n) return;//当前数值已经大于目标值，直接返回
    if(a) dfs_c(u, a, 0);//如果不为0，进行递归计算c的值，并且此处注意要在递归计算a之前
   
    for(int i = 1; i <= 9 ; i++){
        if(!st[i]){
            st[i] = true;
            dfs_a(u + 1, a * 10 + i);
            st[i] = false;
            
        }
    }
}

int main(){
    cin>>n;
    
    dfs_a(0,0);//u代表我们当前已经用了几位数字，a代表当前的数值
    
    cout << cnt;
    return 0;
}
```

### 116\. 飞行员兄弟

题目链接：[飞行员兄弟](https://www.acwing.com/problem/content/118/)  
_**题目描述**_  
给定一个**4\*4**的矩阵，有两种状态，打开或关闭  
`+`表示闭合，`-`表示打开  
可以任意改变\*\*\[i,j\]\*\*的状态，但是也会改变第 **i** 行和第 **j** 列状态  
把所有状态变成打开需要多少步数？  
_**题目分析**_  
枚举16位二进制数，暴力求解即可，主要是分析代码，思路没什么说的  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 4, INF = 100;
typedef pair<int, int>PII;
int change[N][N];
int get(int x, int y)
{
	return x * N + y;
}
int main()
{
	for (int i = 0; i < N; i++)
	{
		for (int j = 0; j < N; j++)
		{
			for (int k = 0; k < N; k++)
				change[i][j] += (1 << get(i, k)) + (1 << get(k, j));
			change[i][j] -= 1 << get(i, j);
		}
	}
	int state = 0;
	for (int i = 0; i < N; i++)
	{
		string line;
		cin >> line;
		for (int j = 0; j < N; j++)
			if (line[j] == '+')
				state += 1 << get(i, j);
	}
	vector<PII>path;
	for (int i = 0; i < 1 << 16; i++)
	{
		int now = state;
		vector<PII>temp;
		for (int j = 0; j < 16; j++)
		{
			if (i >> j & 1)
			{
				int x = j / 4, y = j % 4;
				now ^= change[x][y];
				temp.push_back({ x,y });
			}
			if (!now && (path.empty() || path.size() > temp.size()))path = temp;
		}
	}
	cout << path.size() << endl;
	for (auto& p : path)
		cout << p.first + 1 << ' ' << p.second + 1 << endl;
	return 0;
}
```

### 1208\. 翻硬币

题目链接：[翻硬币](https://www.acwing.com/problem/content/1210/)  
_**题目描述**_  
给定两个字符串序列表示两行若干硬币，如果已知了初始状态和要达到的目标状态，每次只能同时翻转相邻的两个硬币,那么对特定的局面，最少要翻动多少次呢？把翻动相邻的两个硬币叫做一步操作。  
_**题目分析**_  
第一个硬币是否翻动是固定的，所以这道题是一个递推的过程，结果固定  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 110;
int n;
char start[N], aim[N];
void turn(int i)
{
	if (start[i] == '*')start[i] = 'o';
	else start[i] = '*';
}
int main()
{
	cin >> start >> aim;
	int res = 0;
	for (int i = 0; i < strlen(start) - 1; i++)
	{
		if (start[i] != aim[i])
		{
			turn(i), turn(i + 1);
			res++;
		}
	}
	cout << res << endl;
	return 0;
}
```

# Ⅱ. 二分与前缀和

### 789\. 数的范围

题目链接：[数的范围](https://www.acwing.com/problem/content/791/)  
_**题目描述**_  
求一个**非递减**序列的某个数字的**起点位置**与**终点位置**  
_**题目分析**_  
二分解决  
![在这里插入图片描述](https://img-blog.csdnimg.cn/78bb06c5939c43279452fbfc4c0432b6.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWNXaW5nLWxlaW1pbmd6ZQ==,size_20,color_FFFFFF,t_70,g_se,x_16)  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
int a[N];
int n, m;
int main()
{
	cin >> n >> m;
	for (int i = 0; i < n; i++)cin >> a[i];
	while (m--)
	{
		int x;
		cin >> x;
		int l = 0, r = n - 1;
		while (l < r)
		{
			int mid = l + r >> 1;
			if (a[mid] >= x)r = mid;
			else l = mid + 1;
		}
		if (a[l] != x)
			cout << -1 << ' ' << -1 << endl;
		else
		{
			cout << l << ' ';
			l = 0, r = n - 1;
			while(l<r)
			{
				int mid = l + r + 1 >> 1;
				if (a[mid] <= x)l = mid;
				else r = mid - 1;
			}
			cout << r << endl;
		}
	}
	return 0;
}
```

### 790\. 数的三次方根

题目链接：[数的三次方根](https://www.acwing.com/problem/content/792/)  
_**题目描述**_  
\~\~  
_**题目分析**_  
二分法求根  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
double f(double x)
{
	return x * x * x;
}
int main()
{
	double n;
	cin >> n;
	double l = -10000, r = 10000;
	while (r - l >= 1e-7)
	{
		double mid = (l + r) / 2;
		if (f(mid) >= n)r = mid;
		else l = mid;
	}
	printf("%.6f", l);
	return 0;
}
```

### 795\. 前缀和

题目链接：[前缀和](https://www.acwing.com/problem/content/797/)  
_**题目描述**_  
输入一个长度为 n 的整数序列。

接下来再输入 m 个询问，每个询问输入一对 l,r。

对于每个询问，输出原序列中从第 l 个数到第 r 个数的和。  
_**题目分析**_  
前缀和模板题  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
int a[N], b[N];
int n, m;
int l, r;
int main()
{
	cin >> n >> m;
	for (int i = 1; i <= n; i++)
	{
		cin >> a[i];
		b[i] = b[i - 1] + a[i];
	}
	while (m--)
	{
		cin >> l >> r;
		cout << b[r] - b[l - 1] << endl;
	}
	return 0;
}
```

### 730\. 机器人跳跃问题

题目链接：[机器人跳跃问题](https://www.acwing.com/problem/content/732/)  
_**题目描述**_  
给定一个机器人，初始能量为**E**，给定一个数组，编号为 i 的建筑高度为 H\(i\) 个单位。假设机器人在第 k 个建筑，且它现在的能量值是 E，下一步它将跳到第 k+1 个建筑。

如果 H\(k+1\)>E，那么机器人就失去 H\(k+1\)−E 的能量值，否则它将得到 E−H\(k+1\) 的能量值。

游戏目标是到达第 N 个建筑，在这个过程中能量值不能为负数个单位。  
_**题目分析**_  
![在这里插入图片描述](https://img-blog.csdnimg.cn/4b53589da45c4c30914199e86ec2470c.png?x-oss-process=image/watermark,type_ZHJvaWRzYW5zZmFsbGJhY2s,shadow_50,text_Q1NETiBAQWNXaW5nLWxlaW1pbmd6ZQ==,size_20,color_FFFFFF,t_70,g_se,x_16)  
由图可知求橙色区域红色位置，则需要用  
![在这里插入图片描述](https://img-blog.csdnimg.cn/9e9400b397564226884f7a90a5628f65.png)

_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 1e5 + 10;
int h[N];
int n;
bool check(int e)
{
	for (int i = 1; i <= n; i++)
	{
		e = 2 * e - h[i];
		if (e >= 1e5)return true;
		if (e < 0)return false;
	}
	return true;
}
int main()
{
	cin >> n;
	for (int i = 1; i <= n; i++)cin >> h[i];
	int l = 0, r = 1e5;
	while (l < r)
	{
		int mid = l + r >> 1;
		if (check(mid))r = mid;
		else l = mid + 1;
	}
	cout << r << endl;
	return 0;
}
```

### 1221\. 四平方和

题目链接：[四平方和](https://www.acwing.com/problem/content/1223/)  
_**题目描述**_  
四平方和定理，又称为拉格朗日定理：

每个正整数都可以表示为至多 4 个正整数的平方和。

如果把 0 包括进去，就正好可以表示为 4 个数的平方和。

比如：

![在这里插入图片描述](https://img-blog.csdnimg.cn/a14ef2621b5d402982e2ce74fad3997f.png)  
对所有的可能表示法按 a,b,c,d 为联合主键升序排列，最后输出第一个表示法。  
_**题目分析**_  
二分+大表查值

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 2500010;
struct sum
{
	int s, c, d;
	bool operator<(const sum& t)const
	{
		if (s != t.s)return s < t.s;
		if (c != t.c)return c < t.c;
		return d < t.d;
	}
}sum[N];
int n, m;
int main()
{
	cin >> n;
	for (int c = 0; c * c <= n; c++)
		for (int d = c; c * c + d * d <= n; d++)
			sum[m++] = { c * c + d * d,c,d };
	sort(sum, sum + m);
	for(int a = 0; a * a <= n; a++)
		for (int b = 0; a * a + b * b <= n; b++)
		{
			int t = n - a * a - b * b;
			int l = 0, r = m - 1;
			while (l < r)
			{
				int mid = l + r >> 1;
				if (sum[mid].s >= t)r = mid;
				else l = mid + 1;
			}
			if (sum[l].s == t)
			{
				cout << a << ' ' << b << ' ' << sum[l].c << ' ' << sum[l].d << endl;
				return 0;
			}
		}
	return 0;
}
```

### 1227\. 分巧克力

题目链接：[分巧克力](https://www.acwing.com/problem/content/1229/)  
_**题目描述**_  
有**N**块巧克力，分成边长是整数大小相同的块数，希望巧克力尽可能大，且每个小朋友都能分到  
_**题目分析**_  
参考：  
https://www.acwing.com/solution/content/29446/  
![在这里插入图片描述](https://img-blog.csdnimg.cn/1c12b8943ef94861ac0bd7119fcd7f80.png)  
显然在一个数轴上，目标值的左侧（比目标值小的数）都是满足这个性质的，右边则不满足，所以我们要找的值是满足这个性质的最右端  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
int w[N], h[N];
int n, k;
bool check(int a)
{
	int num = 0;
	for (int i = 0; i < n; i++)
	{
		num += (w[i] / a) * (h[i] / a);
		if (num >= k)return true;
	}
	return false;
}
int main()
{
	cin >> n >> k;
	for (int i = 0; i < n; i++)cin >> h[i] >> w[i];
	int l = 1, r = 1e5;
	while (l < r)
	{
		int mid = l + r + 1 >> 1;
		if (check(mid))l = mid;
		else r = mid - 1;
	}
	cout << r << endl;
	return 0;
}
```

### 99\. 激光导弹

题目链接：[激光导弹](https://www.acwing.com/problem/content/101/)  
_**题目描述**_  
给定一个矩阵N\*N的矩阵，xi,yi有不同价值的物品，求**RxR**范围内价值最大  
_**题目分析**_  
二维矩阵前缀和  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=5010;
int g[N][N];

int main()
{
    int n,r;
    cin>>n>>r;
    r=min(r,5001);

    for(int i=0;i<n;i++){
        int x,y,w;
        scanf("%d%d%d",&x,&y,&w);
        ++x,++y;
        g[x][y]+=w;
    }

    for(int i=1;i<=5001;i++)
        for(int j=1;j<=5001;j++)
            g[i][j]+=g[i-1][j]+g[i][j-1]-g[i-1][j-1];
    int res=0;
    for(int i=r;i<=5001;i++){
        for(int j=r;j<=5001;j++){
            res=max(res,g[i][j]-g[i-r][j]-g[i][j-r]+g[i-r][j-r]);
        }
    }
    printf("%d\n",res);
    return 0;
}
```

### 1230.K倍区间

题目链接：[K倍区间](https://www.acwing.com/problem/content/1232/)  
_**题目描述**_  
给定一个长度为**N**的数列，**A1,A2,…AN**，如果其中一段连续的子序列 **Ai,Ai+1,…Aj** 之和是**K** 的倍数，我们就称这个区间 \[i,j\] 是 K 倍区间。  
_**题目分析**_  
![在这里插入图片描述](https://img-blog.csdnimg.cn/05adc8b75d3d41afb2ece12ffa05933d.png)  
![在这里插入图片描述](https://img-blog.csdnimg.cn/6f2e988731214aa18febb21c184b692e.png)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 100010;
typedef long long LL;
int n, k;
LL s[N], cnt[N];
int main()
{
	scanf("%d%d", &n,&k);
	for (int i = 1; i <= n; i++)
	{
		scanf("%lld", &s[i]);
		s[i] += s[i - 1];
	}
	LL res = 0;
	cnt[0] = 1;
	for (int i = 1; i <= n; i++)
	{
		res += cnt[s[i] % k];//res[i]记录的是模数为i的前缀和的个数，将res[i]进行 C2nCn2运算就可以得出模数为i的全部答案（整除除外）
		//C2n = 1 + 2 + 3 + … + n−1
		cnt[s[i] % k]++;
	}
	printf("%lld\n", res);
	return 0;
}
```

# Ⅲ . 数学与简单DP

### 1205\. 买不到的数目

题目链接：[买不到的数目](https://www.acwing.com/problem/content/1207/)  
_**题目描述**_  
把水果糖包成4颗一包和7颗一包的两种，有些糖果数目无法组合出来，求最大不能组成的数字  
_**题目分析**_  
如果 a,ba,b 均是正整数且互质，那么由 ax+by,x≥0,y≥0ax+by,x≥0,y≥0 不能凑出的最大数是 \(a−1\)\(b−1\)−1\(a−1\)\(b−1\)−1。  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
int main()
{
	int p, q;
	cin >> p >> q;
	cout << (p - 1) * (q - 1) - 1 << endl;
	return 0;
}
```

### 1211\. 蚂蚁感冒

题目链接：[蚂蚁感冒](https://www.acwing.com/problem/content/1213/)  
_**题目描述**_  
100 厘米的细长直杆子上有 n 只蚂蚁，有的朝左，有的朝右。两只蚂蚁碰面时，它们会同时掉头往相反的方向爬行。  
有 1 只蚂蚁感冒了。  
并且在和其它蚂蚁碰面时，会把感冒传染给碰到的蚂蚁。  
请你计算，当所有蚂蚁都爬离杆子时，有多少只蚂蚁患上了感冒。  
_**题目分析**_  
左边向左的永远不会被感染，右边向右的不会被感染  
第一个蚂蚁向左，则右边无论如何也不会被感染  
第一个蚂蚁向右，左边无论如何也不会被感染  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 55;
int n;
int x[N];
int main()
{
	cin >> n;
	for (int i = 0; i < n; i++)cin >> x[i];
	int left = 0, right = 0;
	for (int i = 1; i < n; i++)
		if (abs(x[i]) < abs(x[0]) && x[i] > 0)left++;
		else if (abs(x[i]) > abs(x[0]) && x[i] < 0)right++;
	if (x[0] > 0 && right == 0 || x[0] < 0 && left == 0)cout << 1 << endl;
	else cout << left + right + 1 << endl;
	return 0;
}
```

### 1216\. 饮料换购

题目链接：[饮料换购](https://www.acwing.com/problem/content/1218/)  
_**题目描述**_  
三个瓶盖换一个饮料，求一共能喝多少饮料  
_**题目分析**_  
模拟即可

```cpp
#include <bits/stdc++.h>
using namespace std;
int n;
int main()
{
	cin >> n;
	int res = n;
	while (n >= 3)
	{
		res += n / 3;
		n = n / 3 + n % 3;
	}
	cout << res << endl;
	return 0;
}
```

### 2\. 01背包

题目链接：[01背包](https://www.acwing.com/problem/content/2/)  
_**题目描述**_  
01背包直接上模板  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1010;
int w[N],v[N];
int f[N];
int n,m;
int main()
{
    cin>>n>>m;
    for(int i=0;i<n;i++)cin>>v[i]>>w[i];
    for(int i=0;i<n;i++)
    for(int j=m;j>=v[i];j--)
    f[j]=max(f[j],f[j-v[i]]+w[i]);
    cout<<f[m]<<endl;
    return 0;
}
```

### 题目连接：[更多动态规划题解请点击这里](https://blog.csdn.net/qq_52765554/article/details/120877558?spm=1001.2014.3001.5501)

### 1015\. 摘花生

题目链接：[摘花生](https://www.acwing.com/problem/content/1017/)  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 110;
int f[N][N];
int w[N][N];
int n, m, t;
int main()
{
	cin >> t;
	while (t--)
	{
		cin >> n >> m;
		memset(f, 0, sizeof f);
		memset(w, 0, sizeof w);
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= m; j++)
				cin >> w[i][j];
		for (int i = 1; i <= n; i++)
			for (int j = 1; j <= m; j++)
				f[i][j] = max(f[i - 1][j], f[i][j - 1]) + w[i][j];
		cout << f[n][m] << endl;
	}
	return 0;
}
```

### 895\. 最长上升子序列

\*\*\*CODE \*\*\*  
题目链接：[最长上升子序列](https://www.acwing.com/problem/content/897/)

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N=1010;
int f[N],a[N];
int n;
int main()
{
    cin>>n;
    for(int i=1;i<=n;i++)cin>>a[i];
    for(int i=1;i<=n;i++)
    {
        f[i]=1;
        for(int j=1;j<i;j++)
        {
            if(a[j]<a[i])f[i]=max(f[i],f[j]+1);
        }
    }
    int res=0;
    for(int i=1;i<=n;i++)
    res=max(res,f[i]);
    cout<<res<<endl;
    return 0;
}
```

### 1212\. 地宫取宝

题目链接：[地宫取宝](https://www.acwing.com/problem/content/1214/)  
_**题目描述**_  
给定一个 **nxm**的格子矩阵，每个格子放一个宝贝  
只能向右和向下走，入口在左上角，出口在右下角  
如果那个格子中的宝贝价值比小明手中任意宝贝价值都大，小明就可以拿起它（当然，也可以不拿）。  
在给定的局面下，他有多少种不同的行动方案能获得这 k 件宝贝。  
_**题目分析**_  
背包问题，直接上代码  
_**CODE**_

```cpp
#include <bits/stdc++.h>
using namespace std;
const int N = 55, MOD = 1e9 + 7;
int n, m, k;
int w[N][N];
int f[N][N][13][14];
int main()
{
	cin >> n >> m >> k;
	for(int i = 1; i <= n; i++)
		for (int j = 1; j <= m; j++)
		{
			cin >> w[i][j];
			w[i][j]++;
		}
	f[1][1][1][w[1][1]] = 1;
	f[1][1][0][0] = 1;
	for(int i=1; i <=n; i++)
		for (int j = 1; j <= m; j++)
		{
			if (i == 1 && j == 1)continue;
			for(int u=0; u <= k; u++)
				for (int v = 0; v <= 13; v++)
				{
					int& val = f[i][j][u][v];
					val = (val + f[i - 1][j][u][v]) % MOD;
					val = (val + f[i][j - 1][u][v]) % MOD;
					if (u > 0 && v == w[i][j])
					{
						for (int c = 0; c < v; c++)
						{
							val = (val + f[i - 1][j][u - 1][c]) % MOD;
							val = (val + f[i][j - 1][u - 1][c]) % MOD;
						}
					}
				}
		}
	int res = 0;
	for (int i = 0; i <= 13; i++)
		res = (res + f[n][m][k][i]) % MOD;
	cout << res << endl;
	return 0;
}
```