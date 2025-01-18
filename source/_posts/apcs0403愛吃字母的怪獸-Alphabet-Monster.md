---
title: apcs0403愛吃字母的怪獸 (Alphabet Monster)
date: 2025-01-18 23:50:05
tags:
  - Hash Table
  - Heap (Priority Queue)
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0403愛吃字母的怪獸 (Alphabet Monster)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
#define int long long
const int N = 2e5 + 5;
string c;
unordered_map<char,pair<int,int>> ump;
priority_queue<pair<int,int>,vector<pair<int,int>>,greater<pair<int,int>>> pq;
int sum,ans=0;
signed main(){
    Rabbir_Reaper
    int n,m;
    cin>>n>>m;
    cin>>c;
    cin>>ump['W'].first;
    cin>>ump['M'].first;
    cin>>ump['L'].first;
    cin>>ump['W'].second;
    cin>>ump['M'].second;
    cin>>ump['L'].second;
    for(int i=0;i<n;i++){
        sum += ump[c[i]].first;
        while(!pq.empty() && pq.top().first < i){
            sum -= ump[c[pq.top().second]].first;
            pq.pop();
        }
        if(sum > m) ans++;
        pq.push({i+ump[c[i]].second-1,i});
    }
    cout<<ans;
}
```
