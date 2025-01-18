---
title: Q-7-7. AOV 最早完工時間
date: 2025-01-19 00:22:25
ags:
  - topological sort
categories:
  - [解題紀錄,AP325]
---

# 題目
AP325 Q-7-7. AOV 最早完工時間
# 解題思路
O(n+m)

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 10005
int n,m,dis,w[N];//in-degree 入度
bool done[N],indeg[N];
vector<int> adj[N],ans;
set<int> st;
void f(int p){
    if(indeg[p]==0) return;
    int mx=0;
    for(auto e:adj[p]){
        f(e);
        if(w[mx]<w[e])
            mx=e;
    }
    indeg[p]=0;
    w[p]=w[p]+w[mx];
    if(w[ans.front()]<w[p]){//如果多個點花的時間一樣都要判斷關鍵工作
        ans.clear();
        ans.push_back(p);
    }else if(w[ans.front()]==w[p])
        ans.push_back(p);
}
void dfs(int p){//如果多個點花的時間一樣都要判斷關鍵工作
    vector<int> mx;
    mx.push_back(0);
    st.insert(p);
    for(auto e:adj[p]){
        if(w[mx.front()]<w[e]){
            mx.clear();
            mx.push_back(e);
        }else if(w[mx.front()]==w[e])
            mx.push_back(e);
    }
    if(mx.front()!=0){
        for(auto e:mx)
            dfs(e);
    } 
}
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    cin>>n>>m;
    for(int i=1;i<=n;i++)
        cin>>w[i];
    for(int i=0,u,v;i<m;i++){
        cin>>u>>v;
        adj[v].push_back(u);
        indeg[v]=1;
    }
    ans.push_back(0);
    for(int i=1;i<=n;i++){
        if(indeg[i]==0) continue;
        f(i);
    }
    for(auto e:ans)//如果多個點花的時間一樣都要判斷關鍵工作
        dfs(e);
    cout<<w[ans.front()]<<"\n";
    for(auto e:st)
        cout<<e<<" ";
}
```
