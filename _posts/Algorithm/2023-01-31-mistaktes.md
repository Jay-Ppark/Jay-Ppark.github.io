---
title:  "C++ 주의해야할 점"

categories:
  - Algorithm
tags:
  - Algorithm
toc: true
toc_sticky: true
 
date: 2023-01-31
last_modified_at: 2023-01-31
---
# C++ 주의해야할 점  

## STL과 함수 인자  
### 함수인자  
```cpp
#include<iostream>
using namespace std;
void func(int a){
    a=5;
}
int main(void){
    int t=0;
    func(t);
    cout<<t; // 0
}
```
* 값이 복사되어 넘어가서 변하지 않음  
```cpp
#include<iostream>
using namespace std;
void func(int arr[]){
    arr[0]=10;
}
int main(void){
    int arr[3]={1,2,3};
    func(arr);
    cout<<arr[0]; // 10
}
```
* arr의 주소를 넘겨줌  
```cpp
#include<iostream>
using namespace std;
struct pt{
    int x,y;
};
void func(pt a){
    a.x=10;
}
int main(void){
    pt tmp={0,0};
    func(tmp);
    cout<<tmp.x; // 0
}
```
* 값이 다 복사되기 떄문에 영향을 주지 않음  
### 참조자(Reference)  
```cpp
void swap(int* a,int* b){
    int tmp = *a;
    *a=*b;
    *b=tmp;
}
void swap(int& a,int& b){
    int tmp=a;
    a=b;
    b=tmp;
}
```
* 참조자를 쓰면 원본을 바꿈  
### STL vector  
``` cpp
#include<iostream>
#include<vector>
using namespace std;
void func1(vector<int> v){
    v[10]=7;
}
int main(void){
    vector<int> v(100);
    func1(v);
    cout<<v[10]; // 0
    return 0;
}
```
* STL도 구조체와 비슷하게 함수 인자로 보내면 복사본을 보내기 때문에 원본에 영향을 주지 않음  
```cpp
bool cmp1(vector<int> v1,vector<int> v2,int idx){
    return v1[idx]>v2[idx];
}
```
* 시간복잡도 O(N) v1,v2를 복사하기 때문에  
```cpp
bool cmp2(vector<int>& v1,vector<int>&& v2,int idx){
    return v1[idx]>v2[idx];
}
```
* 시간복잡도 O(1) 참조대상의 주소 정보만 넘어가기 때문에  
## 표준 입출력  
* 공백을 포함한 문자열 입력  
```cpp
int main(void){
    string s;
    getline(cin,s);
    cout<<s;
    return 0;
}
```
* 입출력으로 인한 시간초과 막기  
```cpp
ios::sync_with_stdio(0); // C++ stream과 C stream의 동기화 끊기
cin.tie(0); // cin 명령을 수행하기 전에 cout 버퍼를 비우지 않도록 함
```
* endl 쓰지 않기!!  