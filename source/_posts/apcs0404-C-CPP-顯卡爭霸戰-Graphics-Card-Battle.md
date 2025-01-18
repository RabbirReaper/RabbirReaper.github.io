---
title: apcs0404-C-CPP:顯卡爭霸戰(Graphics Card Battle)
date: 2025-01-18 23:51:51
tags:
  - Union and Find
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0404-C-CPP:顯卡爭霸戰(Graphics Card Battle)
# 解題思路
```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
int find(int x,vector<int> &boss){
    if(boss[x] < 0) return x;
    return boss[x] = find(boss[x],boss);
}
vector<vector<int>> adj;
int main(){
    Rabbir_Reaper
    int m,n,k,_k;
    cin>>n>>m;
    _k = n;
    vector<int> boss(n+1,-1);
    for(int i=0,a,b,c;i<m;i++){
        cin>>a>>b>>c;
        adj.push_back({c,a,b});
    }
    cin>>k;
    sort(adj.begin(),adj.end());
    for(int i=0;i<adj.size();i++){
        int u = find(adj[i][1],boss);
        int v = find(adj[i][2],boss);
        if(u != v){
            _k--;
            if(boss[u] > boss[v]) swap(u,v);
            boss[u] += boss[v];
            boss[v] = u;
            if(_k < k){
                cout<<adj[i][0];
                return 0;
            }
        }
    }
}
```
