---
title:  "백트래킹"

categories:
  - Algorithm
tags:
  - Algorithm
toc: true
toc_sticky: true
 
date: 2023-02-17
last_modified_at: 2023-02-17
---
# 백트래킹  
## 알고리즘 설명  
* 현재 상태에서 가능한 모든 후보군을 따라 들어가며 탐색하는 알고리즘  
## 예제  
* 1부터 N까지의 수 중 중복없이 M개를 고른 경우  
```cpp
// 1부터 N까지의 수 중 중복없이 M개를 고른 경우
#include<iostream>
#include<vector>
using namespace std;
bool visited[9];
vector<int> v;
int N,M;
void makeorder(int cnt){
    if(cnt==M){
        for(int i=0;i<v.size();i++){
            cout<<v[i]<<' ';
        }
        cout<<'\n';
        return;
    }
    for(int i=1;i<=N;i++){
        if(!visited[i]){
            visited[i]=true;
            v.push_back(i);
            makeorder(cnt+1);
            v.pop_back();
            visited[i]=false;
        }
    }
}
int main(void){
    cin>>N>>M;
    makeorder(0);
    return 0;
}
```
## STL next_permutation  
* 1,2,3을 가지고 만들 수 있는 모든 순열  
```cpp
// 1,2,3을 가지고 만들 수 있는 모든 순열
#include<iostream>
#include<algorithm>
using namespace std;
int a[3]={1,2,3};
int main(void){
    do{
        for(int i=0;i<3;i++){
            cout<<a[i];
        }
        cout<<'\n';
    }while(next_permutation(a,a+3));// 다음 순열이 존재하지 않는다면 false를 반환
    return 0;
    //123
    //132
    //213
    //231
    //312
    //321
}
```
* 1,2,3,4에서 수 2개를 순서 없이 뽑는 모든 경우  
```cpp
// 1,2,3,4에서 수 2개를 순서 없이 뽑는 모든 경우
#include<iostream>
#include<algorithm>
using namespace std;
int a[4]={0,0,1,1}; // 0의 개수: 뽑고 싶은 수의 개수
int main(void){
    do{
        for(int i=0;i<4;i++){
            if(a[i]==0){
                cout<<i+1;
            }
        }
        cout<<'\n';
    }while(next_permutation(a,a+4));// 다음 순열이 존재하지 않는다면 false를 반환
    return 0;
    //12
    //13
    //14
    //23
    //24
    //34
}
```