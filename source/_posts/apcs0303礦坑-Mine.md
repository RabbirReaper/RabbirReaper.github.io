---
title: apcs0303礦坑 (Mine)
date: 2025-01-18 23:48:56
tags:
  - Queue
categories:
  - [解題紀錄,APCS 模擬團隊]
---

# 題目
APCS 模擬團隊 OJ apcs0303礦坑 (Mine)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define Rabbir_Reaper ios_base::sync_with_stdio(0); cin.tie(0);
int main(){
    Rabbir_Reaper
    int n,k;
    cin>>n>>k;
    queue<int> dis;
    for(int i=0;i<n;i++){
        string temp;
        char sex;
        int age;
        cin>>temp;
        stringstream ss(temp);
        ss>>sex;
        ss>>age;
        if(age >= 65 || age <= 12){
            dis.push(i);
        }else if(sex == 'F' && age >= 35 && age <= 45){
            dis.push(i);
        }
    }
    long long ans=0;
    for(int i=0;i<k;i++){
        ans += (long long)(dis.front()-i);
        dis.pop();
    }
    cout<<ans;
}
```
