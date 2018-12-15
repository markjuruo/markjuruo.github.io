title: '[Reach-Top] 普及模拟赛反思(模拟/排序/...)'
author: MarkJuruo
tags:
  - 模拟
  - 排序
  - 题目陷阱
categories:
  - 学习
date: 2018-09-22 20:28:00
---
好吧不得不说这份卷子算完全是信心赛的简单难度，结果我因为怀着检查一下评测机的质量而为Problem-B写了个O3优化而白白丢了一整题的分数，心痛。以后不能再做这种傻事了。题解内容正在撰写中。。。  
本次测试包含算法：~~魔改~~排序，模拟。~~好吧我故意把暴力漏了。~~

## Problem B

水明老师因为视力不好，只能看到长度为2的字符串，例如AB，AA，BA，所以它将长度为2的字符串称为水明字符串，现在小曹老师送了它一个长度为n（由大写英文字母组成）的字符串，水明老师想找到其中出现次数最多的水明字符串  
BBAABBBA 的答案是BB出现了3次
<!--more-->

### 输入描述
> 第一行输入n  ，2≤n≤100
>
> 第二行输入n个字符

### 输出描述
>输出答案
>
>若出现多个解，输出第一个出现的水明字符串

### 示例数据

输入
>7
ABACABA

输出
> AB

### 解题报告
暂空

## Problem D

水明老师想玩一会儿数字游戏，现在它有一些整数x，并将其写在黑板上，之后对其进行n-1次操作，每次操作有两种方式可选：

>1：将x除以3，必须保证x是3的倍数
2：将x乘以2

在结束操作之后，水明老师将结果写在黑板上，（原来的那些数字x当然要先被擦掉），那结果将以随机顺序作为你们的输入，即肯定不会和黑板上的顺序一致  
那你们的任务是将水明老师给你们的数字重新排列之后与黑板上的序列进行匹配，看看能否一致。  
输出的数列要满足的条件是：第i个数是第i-1个数的两倍或者三分之一（第一个数就随意咯）  
保证有解且唯一

### 输入
> 输入的第一行包含整数n（2≤n≤100） - 序列中元素的数量。   
输入的第二行包含n个整数a1，a2，...，an（1≤ai≤3*1018）

### 输出
> 输出答案

### 示例数据
输入
> 6
4 8 6 3 12 9

输出
> 9 3 6 12 4 8

### 解题报告
第一遍提交，就是没什么技术含量的暴力DFS。每个数都搜索过去，按照题目要求，因为保证有解且只有一个解，所以一旦搜到了答案就直接输出并退出程序。  
第一次提交用的暴力代码，尝试写出了54分很满意了。。。
```cpp
#include<iostream>
#include<cstdio>
#include<stack>
#include<queue>
#include<cstdlib>
using namespace std;
int n;
long long a[110];
long long ans[110];
bool vis[110]={false};
stack<int> st;
void solution(int x){
    if(x==n+1){
//        cout<<666<<endl;
		int flag=0;
        for(int i=1;i<=n;i++){
            ans[++flag]=st.top();
            st.pop();
        }
        for(int i=n;i>=1;i--){
        	cout<<ans[i]<<" ";
		}
        exit(0);
        return;
    }
    for(int i=1;i<=n;i++){
        if(vis[i]){
            continue;
        }
        if(x==1){
            vis[i]=true;
            st.push(a[i]);
            solution(2);
            st.pop();
            vis[i]=false;
        }
        else{
            if(a[i]==st.top()*2||a[i]==st.top()/3){
                st.push(a[i]);
                vis[i]=true;
                solution(x+1);
                vis[i]=false;
                st.pop();
            }
        }
    }
}
int main(){
    cin>>n;
    for(int i=1;i<=n;i++){
        cin>>a[i];
    }
    solution(1);
}
```
成功地骗到了54分，还是十分自豪的，但是和正解的思路完全不同。**吊唁正解带节奏**。

正解的思路很简单：“第i个数是第i-1个数的两倍或者三分之一”，转换一下就是“第i个数是第i-1个数的两倍，第i-1个数是第i个数的三倍”那么就可以以此作为排序标准。算出每个数中2和3因数的个数，然后2越多排在越后面，3越多排在越前面。于是我们可以得知，只要算出了每个数因数中2，3的个数，那么我们就可以根据其包含因数3的个数减去包含2的因数个数的大小进行排序。
```cpp
#include<iostream>
#include<algorithm>
using namespace std;
const int MAXN=110;
int n;
struct setNum{
	int index;//当前数字的编号
	int rawdata;//当前数字的数值
	int p2,p3;//包含2,3的因子数量
	int init(int num,int i){
		rawdata=num;
		index=i;
		int temp=num;
		p2=p3=0;
		while(temp%2==0){
			temp>>=1;
			p2++;
		}
		while(temp%3==0){
			temp/=3;
			p3++;
		}
        //计算包含2,3的因子数量
	}
	friend bool operator < (const setNum cmpnumo,const setNum cmpnum){
		if(cmpnumo.p3-cmpnumo.p2!=cmpnum.p3-cmpnum.p2){
			return cmpnumo.p3-cmpnumo.p2>cmpnum.p3-cmpnum.p2;
		}
		return cmpnumo.index>cmpnum.index;
        //如果之前的比较结果相同，就按初始顺序排列
	}
    //重定义<，之后排序就可以不再写cmp了
};
setNum arr[MAXN];
int main(){
	cin>>n;
	int temp;
	for(int i=0;i<n;i++){
		cin>>temp;
		arr[i].init(temp,i+1);
	}
	sort(arr,arr+n);
	for(int i=0;i<n;i++){
		cout<<arr[i].rawdata<<" ";
	}
} 
```

（持续更新中...）