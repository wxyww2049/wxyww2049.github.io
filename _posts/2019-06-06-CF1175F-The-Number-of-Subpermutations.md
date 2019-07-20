---
layout: post
date: 2019-06-06
title: CF1175F The Number of Subpermutations
tags: 暴力,rmq
---

<a href="http://codeforces.com/contest/1175/problem/F"><font size=5>题目链接</font></a>

# 题意

给出一个长度为$n$的序列$a$,问有多少个区间$[l,r]$满足:在区间$[l,r]$内，$[1,r-l+1]$的每个整数都恰好出现了一次。
$n \le 3 \times 10 ^ 5$,$a_i \le n$

# 思路

可以发现，其实最后的答案一定不会很大。

所以：暴力出奇迹！！！

先对题意进行小小的转化，题目等价于问有多少个区间$[l,r]$满足以下两个条件：

1.区间$[l,r]$中的每个数字都只在区间$[l,r]$中出现了一遍
2.$max\{a_l,a_{l+1}...a_r\}=r-l + 1$

**首先只考虑条件一**

从后往前扫这个序列。用$nxt_i$表示在满足每个数字只出现一遍的前提下，以i为左端，右端点最靠右的位置。(感性理解，我也不知道该咋表述了233.）换句话说，就是$[i,nxt_i - 1]$这个区间是满足条件的，而$[i,nxt_i]$是不满足条件的。用$pos_i$表示i这个数字上次出现的位置。那么就有$nxt_i = min(nxt_{i+1},pos[a_i])$

**在上面的基础上，找满足第二个条件的区间**

在当前区间左端点为l的情况下，右端点可以是$[l,nxt_l-1]$。

直接枚举肯定爆炸。

从左到右枚举右端点r，

当找到满足条件的区间时，就把答案加上1。然后继续枚举

如果当前枚举的区间不符合条件时，也就是说$l+max\{a_l,a_{l+1}...a_r\} > r$时。那么从r到$l+max\{a_l,a_{l+1}...a_r\}$肯定也是不满足条件的，所以直接把$r$调到$l+max\{a_l,a_{l+1}...a_r\}$就行了。

然后就可以跑过去这道题了(似乎还蛮快的233)。

# 代码
```cpp
/*
* @Author: wxyww
* @Date:   2019-06-06 15:53:44
* @Last Modified time: 2019-06-06 16:36:31
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
int tree[N << 2];
int a[N];
void build(int rt,int l,int r) {
	if(l == r) {
		tree[rt] = a[l];return;
	}
	int mid = (l + r) >> 1;
	build(rt << 1,l,mid);
	build(rt << 1 | 1,mid + 1,r);
	tree[rt] = max(tree[rt << 1],tree[rt << 1 | 1]);
}
int query(int rt,int l,int r,int L,int R) {
	if(L <= l && R >= r) return tree[rt];
	int mid = (l + r) >> 1;
	int ret = 0;
	if(L <= mid) ret = max(ret,query(rt << 1,l,mid,L,R));
	if(R > mid) ret = max(ret,query(rt << 1 | 1,mid + 1,r,L,R));
	return ret;
}
int nxt[N],pos[N],n;
int main() {
	n = read();
	for(int i = 1;i <= n;++i) a[i] = read(),pos[i] = n + 1;
	build(1,1,n);
	int ans = 0;
	nxt[n + 1] = n + 1;
	for(int i = n;i >= 1;--i) {
		nxt[i] = min(pos[a[i]],nxt[i + 1]);
		pos[a[i]] = i;
		for(int j = i;j < nxt[i];++j) {
			int x = query(1,1,n,i,j);
			if(i + x - 1 > j) j = i + x - 2;else ++ans;
		}
	}
	cout<<ans;
	return 0;
}
```