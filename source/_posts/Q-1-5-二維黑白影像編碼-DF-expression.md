---
title: Q-1-5. 二維黑白影像編碼(DF-expression)
date: 2025-01-18 16:28:40
tags:
  - Recursion
categories:
  - [解題紀錄,AP325]
---

# 題目
AP325 Q-1-5. 二維黑白影像編碼(DF-expression)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include <bits/stdc++.h> 
using namespace std; 
string s; 
int n,d; 
int dfs(int _n){ 
    if(s[d]=='0') return 0;//終止條件 
    if(s[d]=='1') return _n*_n; 
    _n/=2; 
    int temp=0; 
    for(int i=0;i<4;i++){ 
        d++;     
        temp+=dfs(_n);//進入遞迴 
    } 
    return temp; 
} 
int main(){ 
    cin>>s>>n; 
    cout<<dfs(n); 
} 
```