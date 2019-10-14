---
title: CF1244C The Football Season
tags: 数学
---

[题目链接](https://codeforces.com/contest/1244/problem/C)

## problem

给定$n,p,w,d$，求解任意一对$(x,y)$满足$$xw+yd=p\\ x + y \le n$$

$1\le n\le 10^{12},0\le p\le 10^{17},1\le d<w \le 10^5$

## solution

注意到$n,p$非常大，$w,d$比较小。而且$w>d$。所以我们就想让$y$尽量小。

实际上如果最终有解，那在$y\le w$中肯定有解。

证明如下:

如果有$y'=aw+k(a\ge 1,0\le k < w)$使得$xw+y'd=p$。即$xw+(aw+k)d=xw+awd+kd=(x+ad)w+kd=p$。

发现$xw+(aw+k)d$的系数和为$x+aw+k$。$(x+ad)w+kd$的系数和为$x+ad+k$。又因为$w>d$。所以后者的系数和要小。所以$d$的系数一定小于等于$w$

然后在区间$[0,w]$中枚举$y$。计算$x$即可。

## code

```cpp
#include<cstdio>
#include<iostream>
#include<cstdlib>
#include<cstring>
#include<algorithm>
#include<queue>
#include<vector>
#include<ctime>
#include<cmath>
#include<map>
#include<string>
using namespace std;
typedef long long ll;

ll read() {
	ll x = 0,f = 1; char c = getchar();
	while(c < '0' || c > '9') {if(c == '-') f = -1;c = getchar();}
	while(c >= '0' && c <= '9') {x = x * 10 + c - '0',c = getchar();}
	return x * f;
}

int main() {
	ll n = read(),p = read(),w = read(),d = read();

	for(ll y = 0;y <= w;++y) {
		if((p - y * d) % w) continue;
		ll x = (p - y * d) / w;
		if(x >= 0 && x + y <= n) {
			printf("%I64d %I64d %I64d\n",x,y,n - x - y);
			return 0;
		}
	}
	puts("-1");
	return 0;
}
```