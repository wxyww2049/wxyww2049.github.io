---
layout: post
date: 2019-06-06
title: CF1175E Minimal Segment Cover
tags: 倍增,线段覆盖
---

[题目链接](http://codeforces.com/contest/1175/problem/E)

# 题意

给出n条线段。m次询问，每次询问给出一个区间$[l,r]$问最少需要多少条线段才能覆盖区间$[l,r]$。
所有坐标$\le 5\times 10^5$。$n,m\le 2\times 10^ 5$

# 思路

其实是比较经典的线段覆盖问题。

$f[i][j]$表示从i开始走$2^j$条线段最远到达的位置。

然后对于每次询问都走一遍即可。

# 代码
```cpp
/*
* @Author: wxyww
* @Date:   2019-06-06 10:55:48
* @Last Modified time: 2019-06-06 14:54:02
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
const int N = 1000000 + 100,logN = 23;
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
int f[N][logN + 1];
int query(int l,int r) {
	ll ans = 0;
	for(int i = logN - 1;i >= 0;--i) { 
		if(f[l][i] < r) {
			l = f[l][i];
			ans += (1 << i);
		}
	}
	l = f[l][0];ans++;
	if(l < r) return -1;
	return ans;

}
int main() {
	int n = read(),m = read();
	int mx = 0;
	for(int i = 1;i <= n;++i) {
		int l = read() + 1,r = read() + 1;
		f[l][0] = max(f[l][0],r);
		mx = max(mx,r);
	}

	for(int i = 1;i <= mx;++i) f[i][0] = max(f[i][0],max(i,f[i - 1][0]));

	for(int j = 1;j < logN;++j) 
		for(int i = 1;i <= mx;++i) 
			f[i][j] = f[f[i][j - 1]][j - 1];

	while(m--) {
		int l = read() + 1,r = read() + 1;
		printf("%d\n",query(l,r));
	}
	return 0;
}
```