---
title: e810. 2.潛水 (Diving)
date: 2025-01-19 00:21:30
tags:
  - Dijkstra
categories:
  - [解題紀錄,APCS]
---

# 題目
zerojudge e810. 2.潛水 (Diving)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 505
vector<pair<int,int>> adj[N];
int n,m,A,B,done[N],apcacity;//容量
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cin>>n>>m;
    for(int i=0,u,v,w;i<m;i++){
        cin>>u>>v>>w;
        adj[u].push_back({v,w});
        adj[v].push_back({u,w});
    }
    cin>>A>>B;
    priority_queue<pair<int,int>> pq;
    pq.push({0,A});
    while(!pq.empty()){
        auto e=pq.top();pq.pop();
        int w=e.first,v=e.second;
        if(done[v]!=0) continue;
        done[v]=w;
        apcacity=min(apcacity,w);
        if(v==B) break;
        for(auto k:adj[v]){
            if(done[k.first]!=0) continue;//不一定要
            pq.push({-k.second,k.first});
        }
    }
    if(done[B]==0) cout<<"-1";
    else cout<<-apcacity;
}
```
