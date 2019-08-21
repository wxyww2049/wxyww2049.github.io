---
layout: post
date: 2019-08-21 03:08:08
title:  CF1204D Kirk and a Binary String
---

[题目链接](http://codeforces.com/contest/1204/problem/D2)

## problem

给出一个长度为$n(n\le 10^5)$的只包含01的字符串。把尽可能多的1变为0，使得对于所有的$l \in [1,n],r\in [l,n]$，区间$[l,r]$的最长不下降子序列的长度不变。

## solution

【译自官方题解】

可以发现有些字符是确定的(即无法修改)。这些确定的字符满足以下几个条件。

1. 所有的$10$是确定的。
2. 如果字符串$p$是确定的且字符串$q$是确定的，那么字符串$pq$是确定的。
3. 如果字符串$p$是确定的，那么字符串$1p0$是确定的。
4. 根据以上三个性质可以推出，所有确定的字符串中0和1的个数相同
5. 确定的字符串的最长不降子序列长度为该字符串长度的一半。具体的就是取全部的1或全部的0。

【下方内容为自己YY】

然后考虑如何具体实现。发现上面的5条性质(其实是前3条)与括号序列相同。所以实现就非常简单了。将1看做左括号，0看做右括号。最后将不能匹配的左括号全部变为0即可。

## code

```cpp
/*
* @Author: wxyww
* @Date:   2019-08-21 15:00:10
* @Last Modified time: 2019-08-21 15:01:55
*/
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
const int N = 100000 + 100;
ll read() {
	ll x = 0,f = 1; char c = getchar();
	while(c < '0' || c > '9') {if(c == '-') f = -1;c = getchar();}
	while(c >= '0' && c <= '9') {x = x * 10 + c - '0',c = getchar();}
	return x * f;
}
char s[N];
int a[N],top;
int main() {
	scanf("%s",s + 1);
	int n = strlen(s + 1);
	for(int i = 1;i <= n;++i) {
		if(s[i] == '1') a[++top] = i;
		else if(top) --top;
	}
	while(top) s[a[top--]] = '0';
	printf("%s",s + 1);
	return 0;
}
```