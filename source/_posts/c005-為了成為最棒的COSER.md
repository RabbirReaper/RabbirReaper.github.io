---
title: c005 為了成為最棒的COSER
date: 2025-01-18 22:24:11
tags:
  - Sorting
categories:
  - [解題紀錄,CHSH OJ]
---

# 題目
CHSH OJ c005 為了成為最棒的COSER
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include <bits/stdc++.h>
using namespace std;
#define N 50005
pair<int,int> pii[N];

bool cmp(pair<int,int> x,pair<int,int> y){
    return x.second<y.second;
}

int main(){
    ios::sync_with_stdio(0);
    cin.tie();
    int t;
    cin>>t;
    while(t--){
        int n,sum=0;
        cin>>n;
        for(int i=0;i<n;i++) cin>>pii[i].second;
        for(int i=0,t;i<n;i++){
            cin>>t;
            pii[i].first=pii[i].second-t;
        }
        sort(pii,pii+n,cmp);
        for(int i=0,l=-1;i<n;i++){
            if(pii[i].first>=l){
                sum++;
                l=pii[i].second;
            }
        }
        cout<<sum<<"\n";
    }
}
```
