---
layout: post
date: 2019-06-06
title: CF1175D Array Splitting
tags: 前缀和,思考题
---

[题目链接](http://codeforces.com/contest/1175/problem/D)

# 题意
给出一个长度为$n$的序列$a$，要求分为恰好$K$段。第$i$个点的贡献是$a_i \times f(i)$,$f(x)$表示x所属的是第几段。

# 思路

非常巧妙的一个思路。

先让每个元素都选K遍。然后不断的删除。

具体做法就是，先求一遍前缀和。然后找出前缀和最小的$K-1$个前缀，将其从答案中减去。初始答案为所有元素和$\times K$

这样被减j遍的元素就位于第$K-j$段中。因为是前缀和。所以前边点被减的次数一定大于等于后边。然后就符合题意了。

# 代码
```cpp
/*
* @Author: wxyww
* @Date:   2019-06-06 07:50:48
* @Last Modified time: 2019-06-06 07:56:06
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
const int N = 300000 + 100;
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
ll a[N];
int main() {
	int n = read(),K = read();
	for(int i = 1;i <= n;++i) a[i] = read() + a[i - 1];
	ll ans = a[n] * K;
	sort(a + 1,a + n);
	for(int i = 1;i <= K - 1;++i) ans -= a[i];
	cout<<ans;
	return 0;
}
```