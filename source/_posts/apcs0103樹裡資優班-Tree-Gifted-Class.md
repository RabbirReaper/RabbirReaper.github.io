---
title: apcs0103樹裡資優班 (Tree Gifted Class)
date: 2025-01-18 23:42:04
tags:
  - Binary Search
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0103樹裡資優班 (Tree Gifted Class)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
const int N = 1e5+5;
vector<int> adj[N];
string s;
int parent[N],now;
int main(){
    Rabbir_Reaper
    int n;
    cin>>n;
    for(int i=1,t;i<=n;i++){
        cin>>t;
        for(int j=0,temp;j<t;j++){
            cin>>temp;
            adj[i].push_back(temp);
            parent[temp] = i;
        }
        sort(adj[i].begin(),adj[i].end());
    }
    for(int i=1;i<=n;i++){
        if(parent[i] == 0) now = i;
    }
    cin>>s;
    for(int i=0;i<s.size();i++){
        if(s[i] == 'P'){
            if(parent[now] == 0) break;
            else now = parent[now];
        }else if(s[i] == 'C'){
            int temp = 0;
            i++;
            while(s[i]>='0' && s[i]<='9'){//*
                temp*=10;
                temp+=s[i++]-'0';
            }
            i--;
            if(temp > adj[now].size()) break;
            now = adj[now][temp-1];
        }else if(s[i] == 'R'){
            if(parent[now] == 0) break;
            int j=0;
            while(adj[parent[now]][j] != now) j++;
            if(j+1 < adj[parent[now]].size()) now = adj[parent[now]][j+1];
            else break;
        }else if(s[i] == 'L'){
            if(parent[now] == 0) break;
            int j=0;
            while(adj[parent[now]][j] != now) j++;
            if(j-1 >=0) now = adj[parent[now]][j-1];
            else break;
        }
        // cout<<now<<" ";
    }
    cout<<now;
}

```

# 測資加強版

```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
const int N = 1e6+5;
vector<int> adj[N];
string s;
int parent[N],now;
int main(){
    Rabbir_Reaper
    int n;
    cin>>n;
    for(int i=1,t;i<=n;i++){
        cin>>t;
        for(int j=0,temp;j<t;j++){
            cin>>temp;
            adj[i].push_back(temp);
            parent[temp] = i;
        }
        sort(adj[i].begin(),adj[i].end());
    }
    for(int i=1;i<=n;i++){
        if(parent[i] == 0){
            now = i;
            break;
        }
    }
    cin>>s;
    for(int i=0;i<s.size();i++){
        if(s[i] == 'P'){
            if(parent[now] == 0) break;
            else now = parent[now];
        }else if(s[i] == 'C'){
            int temp = 0;
            i++;
            while(s[i]>='0' && s[i]<='9'){
                temp*=10;
                temp+=s[i++]-'0';
            }
            i--;
            if(temp > adj[now].size()) break;
            now = adj[now][temp-1];
        }else if(s[i] == 'R'){
            if(parent[now] == 0) break;
            auto it = upper_bound(adj[parent[now]].begin(),adj[parent[now]].end(), now);
            if(it == adj[parent[now]].end()){
                break;
            }
            now = *it ;
        }else if(s[i] == 'L'){
            if(parent[now] == 0) break;
            auto it = lower_bound(adj[parent[now]].begin(),adj[parent[now]].end(), now);
            if(it == adj[parent[now]].begin()){
                break;
            }
            now = *(--it) ;
        }
        // cout<<now<<" ";
    }
    cout<<now;
}
```