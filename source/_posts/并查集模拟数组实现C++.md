---
title: 并查集模拟数组实现C++
date: 2021-01-27 12:39:48
tags: [刷题记录]
---

C语言数组模拟实现的并查集算法，功能是将输入的已经确定属于一个集合的顶点`u v`合并到一个集合，便于后续的查询，当然查询之后也可以再输入确定的属于一个集合的`u v`顶点，再做查询。主要思想是对输入的`v u`顶点做判断，如果他们没有共同的父亲顶点，那么就把v的父亲设置为u。代码如下

<!-- more -->

```csharp
#include <bits/stdc++.h>
using namespace std;
int setNum[100];
void initSet(int n){
    for(int i=1;i<=n;i++) setNum[i]=i;
}
int getParent(int v){
    if(setNum[v]==v) return v;
    setNum[v]=getParent(setNum[v]);
    return setNum[v];
}
void andSet(int v,int u){
    int t1=getParent(v);
    int t2=getParent(u);
    if(t1!=t2) setNum[t2]=t1;
}
int main(){
    ios::sync_with_stdio(false);
    int n,m;         // n为顶点个数，m为边的条数
    n=10;   m=9;
    initSet(n);   //初始化集合关系
    int edge[9][2]={1,2, 3,4 ,5,2 ,4,6 ,2,6 ,8,7 ,9,7 ,1,6 ,2,4};
    for(int i=0;i<m;i++) andSet(edge[i][0],edge[i][1]);
    for(int i=0;i<n;i++) if(setNum[i]==i+1) cout<<i+1<<endl;
    return 0;
}
```