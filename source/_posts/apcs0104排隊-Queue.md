---
title: apcs0104排隊 (Queue)
date: 2025-01-18 23:40:53
tags:
  - Queue
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0104排隊 (Queue)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
const int N = 1e5 + 5;
set<int> adj[N];
int indeg[N];
int main(){
    Rabbir_Reaper
    int n;
    priority_queue<int,vector<int>,greater<int>> pq;
    cin>>n;
    for(int to=1,m;to<=n;to++){
        cin>>m;
        indeg[to] = m;
        if(indeg[to] == 0) pq.push(to);
        for(int j=0,from;j<m;j++){
            cin>>from;
            adj[from].insert(to);
        }
    }
    while(!pq.empty()){
        int top = pq.top();
        pq.pop();
        indeg[top] = -1;
        cout<<top<<" ";
        for(auto &u:adj[top]){
            if(--indeg[u] == 0){
                pq.push(u);
            }
        }
    }
}
```
