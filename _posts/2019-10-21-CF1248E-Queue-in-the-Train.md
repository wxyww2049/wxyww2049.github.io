---
title: CF1248E Queue in the Train
tags: 模拟,堆
---

## problem

火车上的一列人要去排队接水。每个人都会在某个特定的时刻口渴。口渴之后他要去排队接水，如果他前面的座位有人已经在排队或者正在接水，那么他就不会去排队。否则他就会去排队。每个人接水都为一个相同的时间P。问每个人接完水的时间。

## solution

其实模拟即可。注意题目的要求。如果一个人口渴的时候已经在排队的人中最靠前的位置也在他后面，那么他就要去排队。否则就把他扔到一个按位置从小到大排序的优先队列里面。然后模拟就行了。

## code

```cpp
#include<cstdio>
#include<queue>
#include<vector>
#include<algorithm>
#include<iostream>
#include<cstring>
#include<ctime>
#include<set>
using namespace std;
typedef long long ll;
const int N = 100010;
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
struct node {
	int pos,w;
}a[N];
bool operator < (const node &A,const node &B) {
	return A.pos > B.pos;
}
priority_queue<node>q;
bool cmp(const node &A,const node &B) {
	return A.w == B.w ? A.pos < B.pos : A.w < B.w;
}
set<int>s;
ll ans[N];
queue<node>tq;
int main() {
	int n = read(),P = read();
	for(int i = 1;i <= n;++i) {
		a[i].pos = i;a[i].w = read();
	}
	
	sort(a + 1,a + n + 1,cmp);
	
	int p = 1;
	ll now = a[1].w;
	s.insert(n + 1);
	while(p <= n || !tq.empty() || !q.empty()) {
		if(!tq.empty()) {
			node k = tq.front();tq.pop();
			now += P;
			ans[k.pos] = now;
			
			while(p <= n && a[p].w < now) {
				if(a[p].pos < *s.begin()) {
					s.insert(a[p].pos);
					tq.push(a[p++]);
				}
				else {
					q.push(a[p++]);
				}
			}
			s.erase(k.pos);
		}
		
		
		if(tq.empty() && q.empty()) 
			now = max(now,(ll)a[p].w);
		
		while(p <= n && a[p].w <= now) q.push(a[p++]);
		int t = *s.begin();
		if(!q.empty() && q.top().pos <= t); {

			tq.push(q.top());
			s.insert(q.top().pos);
			q.pop();
		}
	}
	
	for(int i = 1;i <= n;++i) printf("%I64d ",ans[i]);
	return 0;
}
```