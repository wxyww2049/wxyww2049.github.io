---
layout: post
date: 2019-06-13
title: bzoj3053 The Closest M Points
tags: kd-tree,堆
---

<a href="https://www.lydsy.com/JudgeOnline/problem.php?id=3053"><font size=5>题目链接</font></a>

# 题意

![](https://gitee.com/wxyww/picture/raw/master/小书匠/1560421226069.png)

# 思路
~~调到哭系列~~

其实就是kd-tree的模板题。用堆维护出距离最小的m个点。然后在$kd-tree$上查询。

![](https://gitee.com/wxyww/picture/raw/master/小书匠/1560414998181.png)

![](https://gitee.com/wxyww/picture/raw/master/小书匠/1560415021993.png)

这一个小地方从上午9点调到下午4点半。。。。。真的快气哭了。。。

# 代码
```cpp
//调的心累呀！！！！
/*
* @Author: wxyww
* @Date:   2019-06-13 09:57:42
* @Last Modified time: 2019-06-13 16:35:15
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
const int N = 500010,INF = 1e9;
#define ls TR[rt].ch[0]
#define rs TR[rt].ch[1]
#define pi pair<int,int>
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
int D,WDK,tmp[N];
priority_queue<pi>q;
struct node {
	int ch[2],mx[6],mn[6],d[6];
}TR[N],a[N],TMP;
int mul(int x) {
	return x * x;
}
void up(int rt) {
	for(int i = 0;i < WDK;++i) {
		TR[rt].mx[i] = TR[rt].mn[i] = TR[rt].d[i];
		if(ls) TR[rt].mx[i] = max(TR[rt].mx[i],TR[ls].mx[i]),
			TR[rt].mn[i] = min(TR[rt].mn[i],TR[ls].mn[i]);
		if(rs) TR[rt].mx[i] = max(TR[rt].mx[i],TR[rs].mx[i]),
			TR[rt].mn[i] = min(TR[rt].mn[i],TR[rs].mn[i]);
	}
}
bool cmp(const node &A,const node &B) {
	return A.d[D] < B.d[D];
}
int build(int l,int r,int now) {
	if(l > r) return 0;
	D = now;
	int mid = (l + r) >> 1;
	nth_element(a + l ,a + mid,a + r + 1,cmp);
	int rt = mid;
	TR[mid] = a[mid];
	 ls = build(l,mid - 1,(now + 1) % WDK);
	 rs = build(mid + 1,r,(now + 1) % WDK);
	up(rt);
	return rt;
}
int dis(const node &A,const node &B) {
	int ret = 0;
	for(int i = 0;i < WDK;++i) ret += mul(A.d[i] - B.d[i]);
	return ret;
}
int get(const node &A,const node &B) {
	int ret = 0;
	for(int i = 0;i < WDK;++i)
		ret += mul(max(0,A.d[i] - B.mx[i])) + mul(max(0,B.mn[i] - A.d[i]));
	return ret;
}
void query(int rt) {
	if(!rt) return;
	int K = dis(TR[rt],TMP);
	if(K < q.top().first) q.pop(),q.push(make_pair(K,rt));
	int dl = INF,dr = INF;
	if(ls) dl = get(TMP,TR[ls]);
	if(rs) dr = get(TMP,TR[rs]);
	if(dl < dr) {
		if(dl < q.top().first) query(ls);
		if(dr < q.top().first) query(rs);
	}
	else {
		if(dr < q.top().first) query(rs);
		if(dl < q.top().first) query(ls);
	}
}

int main() {
	// freopen("3053/1.in","r",stdin);
	// freopen("m.out","w",stdout);
	int n;
	while(~scanf("%d%d",&n,&WDK)) {
		// cout<<n<<" "<<WDK<<endl;
		for(int i = 0;i < WDK;++i) a[0].mx[i] = -INF,a[0].mn[i] = INF;
		for(int i = 1;i <= n;++i) { 
			for(int j = 0;j < WDK;++j) {
				a[i].d[j] = read();
			}
		}
		int root = build(1,n,0);
		int T = read();
		// puts("!!!");
		while(T--) {

			for(int i = 0;i < WDK;++i) TMP.d[i] = read();
			int m = read();
			while(!q.empty()) q.pop();
			for(int i = 1;i <= m;++i) q.push(make_pair(INF,0));
			query(root);
			for(int i = 1;i <= m;++i) tmp[i] = q.top().second,q.pop();
			printf("the closest %d points are:\n",m);
			for(int i = m;i >= 1;--i) {
				for(int j = 0;j < WDK - 1;++j)
					printf("%d ",a[tmp[i]].d[j]);
				printf("%d\n",a[tmp[i]].d[WDK - 1]);
			}

		}
	}
	return 0;
}
/*
6 2
2 9
6 9
4 4
7 5
8 1
9 6
1
5 1
*/
```