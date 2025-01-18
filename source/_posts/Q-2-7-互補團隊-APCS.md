---
title: Q-2-7. 互補團隊(APCS)
date: 2025-01-18 16:32:22
tags:
  - Bit Manipulation
  - Bitmask
  - Hash Table
categories:
  - [解題紀錄,AP325]
---

# 題目
AP325 Q-2-7. 互補團隊(APCS)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 500005
unordered_map<long long,int> mp;
long long all,buffer;
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    mp.clear();
    int m,n,t=0;
    cin>>m>>n;
    all=(1<<m)-1;
    for(int i=0;i<n;i++){
        buffer=0;
        string a;
        cin>>a;
        for(int j=0;j<(int)a.size();j++)
            if(a[j]>='a'){
                buffer |= 1LL<<(a[j]-'a'+26);
            }else
                buffer |= 1LL<<(a[j]-'A');
        t+=mp[buffer^all];
        mp[buffer]++;
    }
    cout<<t;
}
```
