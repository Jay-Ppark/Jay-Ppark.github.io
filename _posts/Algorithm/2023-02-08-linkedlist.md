---
title:  "Linked List"

categories:
  - Algorithm
tags:
  - Algorithm
toc: true
toc_sticky: true
 
date: 2023-02-08
last_modified_at: 2023-02-08
---
# Linked List  
## 정의와 성질  
* 원소들을 저장할 때 그 다음 원소가 있는 위치를 포함  
* k번째 원소를 확인/변경하기 위해 O(k)가 필요함  
* 임의의 위치에 원소를 추가/제거는 O(1)  
* 원소들이 메모리 상에 연속해있지 않아 Cache hit rate가 낮지만 할당이 쉬움  
## 배열 vs 연결 리스트  
|| 배열 | 연결리스트 |  
|---|---|---|  
|k번째 원소의 접근| O(1) | O(k) |  
|임의 위치에 원소 추가/제거| O(N) | O(1) |  
|메모리 상의 배치| 연속 | 불연속 |  
|추가적으로 필요한 공간| - | O(N) |  
## 연결 리스트(빠르게 구현)  
* 원고를 배열로 관리  
* pre와 nxt에 이전/다음 원소의 포인터 대신 배열 상의 인덱스를 저장하는 방식  
* 메모리 누수로 실무에서는 사용X  
* 코딩테스트용  
```cpp
#include<iostream>
using namespace std;
const int MX=1000005;
int dat[MX], pre[MX], nxt[MX];//dat[i]는 i번째 원소의 값, pre[i]는 i번지 원소에 대해 이전 원소의 인덱스, nxt[i]는 다음 원소의 인덱스
int unused = 1; //현재 사용되지 않는 인덱스,새로운 원소가 들어갈 수 있는 인덱스
void insert(int addr,int num){
    //addr은 연결리스트 상에서 순서가 아니라 해당 원소의 주소
    dat[unused]=num;
    pre[unused]=addr;
    nxt[unused]=nxt[addr];
    if(nxt[addr]!=-1){
        pre[nxt[addr]]=unused;
    }
    nxt[addr]=unused;
    unused++;
}
//이부분 때문에 실무에서 사용X
//제거된 원소가 프로그램이 종료될 때까지 메모리를 점유하고 있음
void erase(int addr){
    nxt[pre[addr]]=nxt[addr];
    if(nxt[addr]!=-1){
        pre[nxt[addr]]=pre[addr];
    }
}
void traverse(){
    int cur = nxt[0];
    while(cur!=-1){
        cout<<dat[cur]<<' ';
        cur=nxt[cur];
    }
    cout<<"\n\n";
}
int main(void){
    fill(pre,pre+MX,-1);
    fill(nxt,nxt+MX,-1);
    insert_test();
    erase_test();
    return 0;
}
```
## STL list  
```cpp
#include<iostream>
#include<list>
using namespace std;
int main(void){
    list<int> L = {1,2}; // 1 2
    list<int>::iterator t = L.begin(); // t는 1을 가리키는 중
    L.push_front(10); // 10 1 2
    cout<<*t<<'\n'; // t가 가리키는 1 을 출력
    L.push_back(5); // 10 1 2 5
    L.insert(t,6); // 10 6 1 2 5 t가 가리키는 곳 앞에 6을 출력
    t++; // t를 1칸 앞으로 전진 t가 가리키는 값 2
    t=L.erase(t); // t가 가리키는 값을 제거, 그 다음 원소인 5의 위치를 반환 10 6 1 5
    cout<<*t<<'\n'; // 5
    for(auto i : L){
        cout<<i<<' ';
    }
    cout<<'\n';
    for(list<int>::iterator it = L.begin(); it!=L.end();it++){
        cout<<*it<<' ';
    }
    return 0;
}
```
## Tip  
* 문제: 원형 연결 리스트 내의 임의의 노드 하나가 주어졌을 때 해당 List의 길이를 효율적으로 구하는 방법?  
* 동일한 노드가 나올 때 까지 계속 다음 노드로 가면 됨. 공간복잡도 O(1), 시간복잡도 O(N)  
* 문제: 중간에 만나는 두 연결 리스트의 시작점들이 주어졌을 때 만나는 지점을 구하는 방법?  
* 각각의 길이를 구한 후 더 긴 쪽을 둘의 차이만큼 앞으로 이동시키고 두 시작점이 만날 떄까지 두 시작점을 동시에 한 칸씩 전진. 공간복잡도O(1), 시간복잡도O(A+B)  
* 문제 주어진 연결 리스트 안에 사이클이 있는지 판단  
* Floyd's cycle-finding algorithm 이용 한 칸씩 가는 커서와 두 칸씩 가는 커서를 동일한 시작점에서 출발시키면 사이클이 있다면 만남  