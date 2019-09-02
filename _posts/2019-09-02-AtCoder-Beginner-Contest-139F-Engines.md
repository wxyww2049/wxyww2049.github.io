---
layout: post
date: 2019-09-02
title: AtCoder Beginner Contest 139F Engines
---

[链接](https://atcoder.jp/contests/abc139/tasks/abc139_f)

## problem

给出$n$个二元组$(x,y)$。最初位于原点$(0,0)$，每次可以从这$n$个二元组中挑出一个，然后将当前的坐标$(X,Y)$变为$(X+x,Y+y)$，每个二元组只能被选一次。选出一些二元组，使得按照这些二元组移动后与原点的欧几里得距离最大。求这个距离。

## solution

将这$n$个二元组看做$n$个向量。移动方式遵循平行四边形定则。所以两个向量夹角越小，相加形成的和向量模长就越大。所以将这些向量按照极角排序。选择的向量肯定是一个区间。枚举左右端点，求最大值即可。

## code

```cpp
//@Author: wxyww
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
#define pi pair<double,double>
const int N = 110;
ll read() {
	ll x = 0,f = 1; char c = getchar();
	while(c < '0' || c > '9') {if(c == '-') f = -1;c = getchar();}
	while(c >= '0' && c <= '9') {x = x * 10 + c - '0',c = getchar();}
	return x * f;
}
pi a[N];
bool cmp(const pi &A,const pi &B) {
	return atan2(A.second , A.first) < atan2(B.second , B.first);
}
int main() {
	int n = read();
	for(int i = 0;i < n;++i) a[i].first = read(),a[i].second = read();
	sort(a,a + n,cmp);
	double ans = 0;
	for(int i = 0;i < n;++i) {
		double X = a[i].first,Y = a[i].second;
		ans = max(ans,X * X + Y * Y);
		for(int j = (i + 1) % n;j != i;j = (j + 1) % n) {
			X += a[j].first,Y += a[j].second;
			ans = max(ans,X * X + Y * Y);
		}
	}
	printf("%.10lf",sqrt(ans));
	return 0;
}
```