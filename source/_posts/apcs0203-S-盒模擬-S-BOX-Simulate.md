---
title: apcs0203:S 盒模擬 (S-BOX-Simulate)
date: 2025-01-18 23:45:11
tags:
  - Bit Manipulation
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0203:S 盒模擬 (S-BOX-Simulate)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
int main(){
    Rabbir_Reaper
    int n,m;
    cin>>n>>m;
    vector<int> v[4];
    for(int i=0,k=1<<(n-2);i<4;i++){
        for(int j=0,temp;j<k;j++){
            cin>>temp;
            v[i].push_back(temp);
        }
    }
    int l = (1<<(n-1)),tand = (1<<(n-2)) -1;
    for(int i=0,temp,k;i<m;i++){
        cin>>temp;
        k = (temp&1) + ((temp&l) == l ? 2 : 0);
        temp>>=1;
        temp&=tand;
        cout<<v[k][temp]<<" ";
    }
}
```
