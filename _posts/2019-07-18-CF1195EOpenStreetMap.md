---
layout: post
date: 2019-07-18
title: CF1195E OpenStreetMap
tags: codeforces,单调队列
---

<a href="http://codeforces.com/contest/1195/problem/E" target="_blank"><font size=5>题目链接</font></a>

# 题意

有一个$n\times m$的矩阵，询问其中所有大小为$a \times b$的子矩阵的最小值之和。
$1\le n,m \le 3000$

# 思路

因为是子矩阵的大小是固定的。所以想到先将其中一维的最小值求出来，然后在此基础上再去求另外一维的最小值。

看数据范围不能带$log$。在每一维上单独做的时候就是一个滑动窗口。所以直接单调队列做刚好。

# 代码
```cpp
/*
* @Author: wxyww
* @Date:   2019-07-18 14:37:59
* @Last Modified time: 2019-07-18 14:55:00
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
const int N = 3010;
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
ll ans;
int t[N * N],a[N][N],b[N][N],q[N];
int main() {
	int n = read(),m = read(),nn = read(),mm = read();
	t[0] = read();int x = read(),y = read(),mod = read();
	for(int i = 1;i <= n * m;++i) t[i] = (1ll * t[i - 1] * x % mod+ y) % mod;

	for(int i = 1;i <= n;++i) {
		int head = 1,tail = 0;
		for(int j = 1;j <= m;++j) {
			a[i][j] = t[(i - 1) * m + j - 1];
			while(tail >= head && a[i][q[tail]] >= a[i][j]) --tail;
			q[++tail] = j;
			while(tail >= head && q[head] <= j - mm) ++head;
			if(j >= mm) b[i][j - mm + 1] = a[i][q[head]];
		}
	}



	for(int j = 1;j <= m - mm + 1;++j) {
		int head = 1,tail = 0;
		for(int i = 1;i <= n;++i) {
			while(tail >= head && b[q[tail]][j] >= b[i][j]) --tail;
				q[++tail] = i;
			while(tail >= head && q[head] <= i - nn) ++head;
			if(i >= nn) ans += b[q[head]][j];
		}
	}

	cout<<ans<<endl;

	return 0;
}
```