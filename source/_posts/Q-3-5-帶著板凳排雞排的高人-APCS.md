---
title: Q-3-5. 帶著板凳排雞排的高人(APCS)
date: 2025-01-18 16:40:54
tags:
  - Binary Search
categories:
  - [解題紀錄,AP325]
---

# 題目
AP325 Q-3-5. 帶著板凳排雞排的高人(APCS)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include <bits/stdc++.h>
using namespace std;
#define N 200005
int h[N],p[N];
long long all=0;
vector<pair<int,int>> v;
int main(){
    int n;
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>h[i];
        h[i]*=-1;
    }
    for(int i=1;i<=n;i++){
        cin>>p[i];
        p[i]*=-1;
    }
    v.push_back({-10000005,0});
    v.push_back({h[1],1});
    for(int i=2;i<=n;i++){
        int it=lower_bound(v.begin(),v.end(),make_pair(h[i]+p[i],-1))-v.begin();
        it-=1;
        if(it == 0) all+=i-1;
        else all+=i-v[it].second-1;
        while(v.size()!=0 && h[i]<v.back().first)
            v.pop_back();
        v.push_back(make_pair(h[i],i));
    }
    cout<<all;
    return 0;
}
```
