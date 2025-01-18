---
title: Q-7-11. 紅白彩帶(APCS)
date: 2025-01-19 00:00:35
tags:
  - Union and Find
categories:
  - [解題紀錄,AP325]
---

# 題目
AP325 Q-7-11. 紅白彩帶(APCS)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include <bits/stdc++.h>
using namespace std;
#define N 100005
int p[N],maxn=0;
bool color[N];
multiset<int> mst;
int find(int x){
    if(p[x]<0)
        return x;
    return p[x]=find(p[x]);
}
void unionn(int x,int y){
    int r1=find(x);
    int r2=find(y);
    auto it=mst.find(-p[r1]);
    mst.erase(it);
    it=mst.find(-p[r2]);
    mst.erase(it);
    if(p[r1]>p[r2]){
        p[r2]+=p[r1];
        p[r1]=r2;
        mst.insert(-p[r2]);
        maxn=max(maxn,-p[r2]);
    }else{
        p[r1]+=p[r2];
        p[r2]=r1;
        mst.insert(-p[r1]);
        maxn=max(maxn,-p[r1]);
    }
}
int main(){
    int n,k;
    cin>>n>>k;
    for(int i=1;i<=n;i++){
        cin>>color[i];
        if(color[i]){
            p[i]=-1;
            mst.insert(1);
            if(color[i-1]==1) unionn(i,i-1);
        }
    }
    int mx=*mst.rbegin(),mn=*mst.begin();
    for(int i=0,temp;i<k;i++){
        cin>>temp;
        color[temp]=1;
        p[temp]=-1;
        mst.insert(1);
        for(int j=-1;j<2;j+=2){
            if(color[temp+j]) unionn(temp,temp+j);
        }
        mx+=maxn;
        mn+=*mst.begin();
    }
    cout<<mx<<"\n"<<mn;
    return 0;
}
```
