---
layout: post
date: 2019-07-21
title: CF1178D Prime Graph
tags: codeforces,题解
---

## 题意

构造一张有$n(3\le n\le 1000)$个点的无向图(无重边和自环)。满足:
>1. 边的总数为素数
>2. 所有点的度数均为素数

输出方案

## solve

如果所有点的度数确定了。那么边数就是度数之和的一半。连边就很简单了。

所以考虑怎么确定点的度数。

猜想:必有至少一个$A \in [2n,3n] (3 \le n \le 1000)$满足$\frac{A}{2}$为素数。

经测试，成立。

所以可以让所有点的度数都为$2$或$3$只要找到这个$A$作为度数之和。然后边数就是$\frac{A}{2}$。

设度数为$2$的点有$x$个，度数为$3$的点有$y$个。
列方程：

$$\left\{
\begin{aligned}
2x + 3y =  A \\
x + y = n
\end{aligned}
\right.
$$

将所有点连成一个环之后，再连$y$条边即可。

## code

```cpp
/*
* @Author: wxyww
* @Date:   2019-07-21 00:20:04
* @Last Modified time: 2019-07-21 07:53:25
*/
#include<cstdio>
#include<iostream>
#include<cstdlib>
#include<cstring>
#include<algorithm>
#include<queue>
#include<vector>
#include<ctime>
using namespace std;
typedef long long ll;
const int N = 1000000 + 100;
#define pi pair<int,int>
int cnt;
ll read() {
	ll x=0,f=1;char c=getchar();
	while(c<'0'||c>'9') {
		if(c=='-') f=-1;
		c=getchar();
	}
	while(c>='0'&&c<='9') {
		x=x*10+c-'0';
		c=getchar();
	}
	return x*f;
}
int tot,dis[N],vis[N],a[N],K;
void Eur() {
	for(int i = 2;i <= K;++i) {
		if(!vis[i]) dis[++tot] = i;
		for(int j = 1;j <= tot && 1ll * dis[j] * i <= K;++j) {
			vis[dis[j] * i] = 1;
			if(i % dis[j] == 0) break;
		}
	}
}	
int A;
pi ans[N];
priority_queue<pi>q;
int p(int x) {
	if(x & 1) return x + 1;
	return x;
}
int main() {
	int n = read();
	K = 100000;
	Eur();
	for(A = n * 2;A <= n * 3;A ++) {
		if(A & 1) continue;
		if(!vis[A / 2]) break;
	}

	int Y = A - n * 2;
	int X = n - Y;

	for(int i = 1;i <= X;++i) q.push(make_pair(2,i));
	for(int i = X + 1;i <= n;++i) q.push(make_pair(3,i));
 
	for(int i = 1;i < n;++i) ans[++cnt] = make_pair(i,i + 1);
 
	ans[++cnt] = make_pair(n,1);
 
	Y /= 2;
	for(int i = 1;i <= n;i ++) {
		if(Y && !a[i] && !a[i + 2]) ans[++cnt] = make_pair(i,i + 2),Y--,a[i] = a[i + 2] = 1;
	}
 
	printf("%d\n",cnt);
	for(int i = 1;i <= cnt;++i) {
		printf("%d %d\n",ans[i].first,ans[i].second);
	}
 
	return 0;
}
```