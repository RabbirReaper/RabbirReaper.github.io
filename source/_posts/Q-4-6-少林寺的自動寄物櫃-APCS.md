---
title: Q-4-6. 少林寺的自動寄物櫃(APCS)
date: 2025-01-18 16:47:31
tags: 
  - Greedy
categories:
  - [解題紀錄,AP325]
---

# 題目
AP325 Q-4-6. 少林寺的自動寄物櫃(APCS)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 100005
struct item{
    int w,f;
};
bool cmp(item p,item q){
    return p.w*q.f < q.w*p.f;
}
int main(){
    item ite[N];
    int n;
    cin>>n;
    for(int i=0;i<n;i++)
        cin>>ite[i].w;
    for(int i=0;i<n;i++)
        cin>>ite[i].f;
    sort(ite,ite+n,cmp);
    long long all=0,buffer=ite[0].w;
    for(int i=1;i<n;i++){
        all+=buffer*ite[i].f;
        buffer+=ite[i].w;
    }
    cout<<all;
    return 0;
}
```
