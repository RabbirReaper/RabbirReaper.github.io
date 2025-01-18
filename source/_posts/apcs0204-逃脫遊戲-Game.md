---
title: apcs0204:逃脫遊戲 (Game)
date: 2025-01-18 23:46:27
tags:
  - Breadth-First Search
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0204:逃脫遊戲 (Game)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include <bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
const int N = 250 +5;
int dir[4][2]= {{-1,0},{0,1},{1,0},{0,-1}}; 
vector<vector<vector<int>>> adj(N,vector<vector<int>>(N,vector<int>(15,-1)));
map<pair<int,int>,vector<pair<int,int>>> mp;
int main(){
    Rabbir_Reaper
    int n,k,t;
    cin>>n>>k>>t;
    for(int i=0,x1,y1,x2,y2;i<k;i++){
        cin>>x1>>y1>>x2>>y2;
        mp[{x1,y1}].push_back({x2,y2});
    }
    queue<vector<int>> qu;
    qu.push({1,1,0});
    adj[1][1][0] = 0;
    int ans=0;
    while(!qu.empty()){
        vector<int> now = qu.front();
        qu.pop();
        if(now[0] == n && now[1] == n){
            ans = adj[n][n][now[2]];
            break;
        }
        for(int i=0;i<4;i++){
            int tx = now[0] + dir[i][0];
            int ty = now[1] + dir[i][1];
            if(tx<=0 || ty<=0 || tx>n || ty>n || adj[tx][ty][now[2]]!=-1) continue;
            adj[tx][ty][now[2]] = adj[now[0]][now[1]][now[2]]+1;
            qu.push({tx,ty,now[2]});
        }
        for(auto &u:mp[{now[0],now[1]}]){
            if(now[2]+1 >= t) continue;
            if(adj[u.first][u.second][now[2]+1] != -1) continue;
            adj[u.first][u.second][now[2]+1] = adj[now[0]][now[1]][now[2]]+1;
            qu.push({u.first,u.second,now[2]+1});
        }
    }
    cout<<ans;
}
```