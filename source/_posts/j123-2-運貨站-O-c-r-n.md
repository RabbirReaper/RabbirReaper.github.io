---
title: j123. 2. 運貨站 O(c * r * n)
date: 2025-01-19 00:20:05
tags:
  - Simulation
categories:
  - [解題紀錄,APCS]
---

# 題目
zerojudge j123: 2. 運貨站 O(c * r * n)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 55
struct range{
    int x,y;
};
bool block[5][4][3],Fs[N][N];//Freight station
int r,c,n,space,block_s[5]={4,3,4,4,5},trash;
range block_r[5]={{4,1},{1,3},{2,2},{2,3},{3,2}};
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    for(int i=0;i<4;i++) block[0][i][0]=1;
    for(int i=0;i<3;i++) block[1][0][i]=1;
    for(int i=0;i<2;i++) block[2][i][i]=block[2][!i][i]=1;
    for(int i=0;i<2;i++) for(int j=0;j<3;j++) block[3][i][j]=1;
    block[3][0][0]=block[3][0][1]=0;
    for(int i=0;i<3;i++) for(int j=0;j<2;j++) block[4][i][j]=1;
    block[4][0][0]=0;
    cin>>r>>c>>n;
    space=r*c;
    while(n--){
        string ti;
        int yi,d,done=-1;
        cin>>ti>>yi;
        d=ti[0]-'A';
        bool check=0;
        for(int xi=c-block_r[d].y;xi>=0;xi--){
            for(int j=yi;j<yi+block_r[d].x;j++){
                for(int k=xi;k<xi+block_r[d].y;k++){
                    if((block[d][j-yi][k-xi] && Fs[j][k])){
                        check=1;
                        break;
                    }
                }
                if(check) break;
            }
            if(check) break;
            done=xi;
        }
        if(done!=-1){
            for(int j=yi;j<yi+block_r[d].x;j++)
                for(int k=done;k<done+block_r[d].y;k++)
                    Fs[j][k]=block[d][j-yi][k-done];
            space-=block_s[d];
        }else
            trash++;
    }
    cout<<space<<" "<<trash;
}
```
