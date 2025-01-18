---
title: apcs0304-cpp:調酒專家 (Mixologist)
date: 2025-01-18 23:48:04
tags:
  - Dynamic Programming
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0304-cpp:調酒專家 (Mixologist)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
typedef long long LL;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
const int N = 2000 + 5;
LL dp[N][N];
int main(){
    Rabbir_Reaper
    int n,m;
    vector<LL> v;
    cin>>n>>m;
    v.push_back(1);
    for(LL i=0,temp;i<m;i++){
        cin>>temp;
        v.push_back(temp);
    }
    for(int i=0;i<=m;i++) dp[0][i] = 1;
    for(int i=0;i<=n;i++) dp[i][0] = 1;
    for(LL i=1;i<=n;i++){
        for(int j=1;j<=m;j++){
            int limit = i - min(i,v[j]);
            dp[i][j] = dp[i][j-1]-(limit == 0 ? 0 : dp[limit-1][j-1]);
            dp[i][j] %= 998244353;
            if(j == m){
                cout<<dp[i][m];
                if(i != n) cout<<" ";
            }
            dp[i][j] += dp[i-1][j];
        }
    }
}
```
