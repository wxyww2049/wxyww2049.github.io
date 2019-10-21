---
title: CF1248F Catowice City
tags: 二分图染色
---

[题目链接](http://codeforces.com/contest/1248/problem/F)

## problem

有$n$个人，每个人家有一只猫。每个人都认识一些猫(其中肯定包括自己家的猫)。选出$j$个人和$k$只猫$j,k\ge 1$。使得$j+k=n$且选出的人和猫都互不认识。

## solution

一个显然但是重要的推论是:

>每个人家都必须去一个人或者一只猫。

这样我们只需要枚举第一个人家去的是人还是猫，就可以根据他们之间的关系推出其他的人家是人还是猫。当无论第一家选择人还是选择猫，都会导致去$n$只猫或者$n$个人时无解。

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
const int N = 1000100;
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
vector<int>e1[N],e2[N];
int vis[N],cnt;
void dfs(int u) {
	vis[u] = 1;cnt++;
	for(vector<int>::iterator it = e1[u].begin();it != e1[u].end();++it) {
		if(vis[*it]) continue;
		dfs(*it);
	}
}
void dfs2(int u) {
	vis[u] = 1;cnt++;
	for(vector<int>::iterator it = e2[u].begin();it != e2[u].end();++it) {
		if(vis[*it]) continue;
		dfs2(*it);
	}
}
int main() {
	int T = read();
	while(T--) {
		int n = read(),m = read();
		cnt = 0;
		for(int i = 1;i <= n;++i) vis[i] = 0;
		for(int i = 1;i <= n;++i) e1[i].clear(),e2[i].clear();
		for(int i = 1;i <= m;++i) {
			int u = read(),v = read();
			e1[u].push_back(v);e2[v].push_back(u);
		}
		dfs(1);
		int bz = 1;
		if(cnt == n) {
			cnt = 0;
			for(int i = 1;i <= n;++i) vis[i] = 0;
			dfs2(1);
			if(cnt == n) {puts("No");continue;}
			cnt = n - cnt;
			bz = 0;
		}
		puts("Yes");
		printf("%d %d\n",cnt,n - cnt);
		for(int i = 1;i <= n;++i) {
			if(vis[i] == bz) printf("%d ",i);
		}
		puts("");
		for(int i = 1;i <= n;++i) {
			if(vis[i] != bz) printf("%d ",i);
		}
		puts("");
	}
	return 0;
}
```