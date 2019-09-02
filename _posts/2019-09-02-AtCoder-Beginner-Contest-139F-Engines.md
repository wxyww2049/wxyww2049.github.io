---
layout: post
date: 2019-09-02
title: AtCoder Beginner Contest 139F Engines
---

[链接](https://atcoder.jp/contests/abc139/tasks/abc139_f)

## problem

给出$n$个二元组$(x,y)$。最初位于原点$(0,0)$，每次可以从这$n$个二元组中挑出一个，然后将当前的坐标$(X,Y)$变为$(X+x,Y+y)$，每个二元组只能被选一次。选出一些二元组，使得按照这些二元组移动后与原点的欧几里得距离最大。求这个距离。

## solution

将这$n$个二元组看做$n$个向量。移动方式遵循平行四边形定则。所以两个向量夹角越小，相加形成的和向量模长就越大。所以将这些向量按照极角排序。选择的向量肯定是一些区间。
