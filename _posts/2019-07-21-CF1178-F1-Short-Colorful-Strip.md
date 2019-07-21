---
layout: post
date: 2019-07-21
title: CF1178 F1 Short Colorful Strip
tags: codeforces,题解
---

[题目链接](http://codeforces.com/contest/1178/problem/F1)

## 题意

有个长度为$m$公分的布，要在上面每公分都染上颜色，整块布染恰好$n(n=m)$种颜色。颜色标号从$1$到$n$。染色需遵循:
>1.从颜色$1$到颜色$n$依次，即必须先染标号小的颜色
>2.每次可以染任意一个区间，但必须满足这个区间之前的颜色是相同的。

询问将这块布染成所给颜色的方案数。

## solve

区间$dp$。

$f[l][r]$表示染好$[l,r]$这个区间的方案数。$g[l][r]$表示最小的颜色最后单独染的方案数。

所以就有$g[l][r] = f[l][t-1] \times f[t+1][r]$,($a[t]$为区间$[l,r]$中的最小值)

然后枚举一个$k$表示$[l,k]$全染成最小颜色值的方案数。

那么就有$f[i][j]=\sum\limits_{k=l}^{r-1}g[l][k]\times f[k+1][r]$

最后$f[1][n]$即为答案。

## code

```cpp
/*
* @Author: wxyww
* @Date:   2019-07-21 09:01:13
* @Last Modified time: 2019-07-21 10:48:02
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
const int N = 510,mod = 998244353;
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
int a[N],f[N][N],g[N][N];
int main() {
	int n = read(),m = read();
	for(int i = 1;i <= n;++i) a[i] = read();
	a[0] = 1e9;
	for(int i = 1;i <= n + 1;++i) g[i][i] = f[i][i] = f[i][i - 1] = g[i][i - 1] = 1;
	for(int len = 2;len <= n;++len) {
		for(int l = 1;l + len - 1 <= n;++l) {
			int r = l + len - 1,t = 0;
			for(int k = l;k <= r;++k) if(a[k] < a[t]) t = k;
			f[l][r] = g[l][r] = 1ll * f[l][t - 1] * f[t + 1][r] % mod;

			for(int k = l;k < r;++k) {
				f[l][r] = (f[l][r] + 1ll * g[l][k] * f[k + 1][r] % mod) % mod;
			}
		}
	}
	cout<<f[1][n];
	return 0;
}
```