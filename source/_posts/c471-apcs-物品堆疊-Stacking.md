---
title: c471. apcs 物品堆疊 (Stacking)
date: 2025-01-19 00:10:43
tags:
  - Greedy
categories:
  - [解題紀錄,APCS]
---

# 題目
zerojudge c471. apcs 物品堆疊 (Stacking)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 100005
struct stacking{
    int f;
    long long w;
};
bool cmp(stacking x,stacking y){
    return x.f*y.w<y.f*x.w;
}
int main(){
    stacking box[N];
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>box[i].w;
    for(int i=0;i<n;i++)
        cin>>box[i].f;
    sort(box,box+n,cmp);
    long long ans=0;
    for(int i=n-2;i>=0;i--){
        ans+=box[i].f*box[i+1].w;
        box[i].w+=box[i+1].w;
    }
    cout<<ans<<"\n";
    return 0;
}
```
