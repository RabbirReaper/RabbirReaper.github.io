---
title: i177. 小畫家 (Painter)
date: 2025-01-19 00:12:04
tags:
  - Breadth-First Search
categories:
  - [解題紀錄,其他]
---

# 題目
zerojudge i177. 小畫家 (Painter)
# 解題思路
目前沒東西...

# 程式碼
```cpp
#include<bits/stdc++.h>
using namespace std;
#define N 520
int a[N][N]={0};
int diri[4]={-1,0,1,0};
int dirj[4]={0,1,0,-1};
bool done[N][N]={0};
struct xy{
    int x,y;
};
int main(){
    ios::sync_with_stdio(0);
    cin.tie(0);
    int h,w,si,sj,z,temp;
    cin>>h>>w>>si>>sj>>z;
    z++;
    for(int i=1;i<=h;i++){
        for(int j=1;j<=w;j++){
            cin>>a[i][j];
            a[i][j]+=1;
        }
    }
    temp=a[si][sj];
    queue<xy> q;
    q.push({si,sj});
    done[si][sj]=1;
    while(!q.empty()){
        auto e=q.front();
        q.pop();
        int x=e.x,y=e.y;
        a[x][y]=z;
        for(int i=0;i<4;i++){
        //done 是為了讓同個點不要被重複被放入
            if(!done[x+diri[i]][y+dirj[i]] && a[x+diri[i]][y+dirj[i]]==temp){
                q.push({x+diri[i],y+dirj[i]});
                done[x+diri[i]][y+dirj[i]]=1;
            }
        }
    }
    // cout<<"\n";
    for(int i=1;i<=h;i++){
        for(int j=1;j<=w;j++)
            cout<<a[i][j]-1<<" ";
        cout<<endl;
    }
}
```
