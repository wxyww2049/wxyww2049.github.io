---
title: CF785D Anton and School - 2
tags: 组合数学
---

[题目链接](http://codeforces.com/contest/785/problem/D)

## problem

给出一个括号序列，要求删除一些括号使得剩下的括号序列是个匹配的括号序列，且改括号序列左边全部为左括号，右边全部为右括号。

## solution

考虑枚举左右括号交界的位置$x$，为了避免重复计算，强制要求$x$左边的第一个左括号必选。然后枚举$x$的时候只枚举左括号的位置。

然后枚举括号序列的长度。假设长度为$2i$，那么左右括号就分别有$i$个，假设左边有$n$个左括号，右边有$m$个右括号。那么该位置的答案就是$\sum\limits_{i=1}^{min(n,m)}C_{n-1}^{i-1}C_{m}^i$

观察上面这个式子，当$i=0$时没有贡献，所以我们可以等价的写成$\sum\limits_{i=0}^{min(n,m)}C_{n-1}^{i-1}C_m^i$

假设$n\le m$
上面的式子也可以写成
$\sum\limits_{i=0}^nC_{n-1}^{n-i}C_m^i$

考虑这个东西的组合意义，也就相当于有$n+m-1$个物品从中选$n$个。
所以上面的东西其实就是$C_{n+m-1}^n$然后就可以$O(1)$求了。

可以发现，当$m\le n$时，推出的式子也是这个。

这样总复杂度就成了$O(n)$

## code

```cpp
#include<cstdio>
#include<queue>
#include<vector>
#include<algorithm>
#include<iostream>
#include<cstring>
#include<ctime>
using namespace std;
typedef long long ll;
const int mod = 1e9 + 7;
ll read() {
	ll x = 0,f = 1;char c = getchar();
	while(c < '0' || c > '9') {
		if(c == '-') f = -1;c = getchar();
	}
	while(c >= '0' && c <= '9') {
		x = x * 10 + c - '0';
		c = getchar();
	}
	return x * f;
}
int n;
const int N = 200100;
char s[N];
int cnta,cntb,jc[N],inv[N];
int C(int x,int y) {
	return 1ll * jc[x] * inv[y] % mod * inv[x - y] % mod;
}
int qm(int x,int y) {
	int ret = 1;
	for(;y;y >>= 1,x = 1ll * x * x % mod) {
		if(y & 1) ret = 1ll * ret * x % mod;
	}
	return ret;
}

int main() {
	scanf("%s",s + 1);
	n = strlen(s + 1);
	jc[0] = 1;
	for(int i = 1;i <= n;++i) jc[i] = 1ll * jc[i - 1] * i % mod;
	for(int i = 0;i <= n;++i) inv[i] = qm(jc[i],mod - 2);
	for(int i = 1;i <= n;++i) if(s[i] == ')') cntb++;
	ll ans = 0;
	for(int i = 1;i <= n;++i) {
		if(s[i] == '(') {
			cnta++;
			ans += C(cnta + cntb - 1,cnta);
			ans %= mod;
		}
		else cntb--;
	}
	cout<<ans;
		
	return 0;
}
```