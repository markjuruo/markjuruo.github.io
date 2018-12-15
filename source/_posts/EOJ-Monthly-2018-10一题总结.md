title: '[EOJ] Monthly 2018.10一题总结(模拟)'
author: MarkJuruo
tags:
  - 模拟
categories:
  - 学习
date: 2018-10-02 22:39:00
mathjax: true
---
由于我太蒻了，所以只写对了一题的代码：《Problem A. oxx 的小姐姐们》  
由于我太蒻了，看不出这是数学题，就打了个十分暴力的模拟程序。  
## 题目描述
oxx 和他的小姐姐（们）躺在图书馆前的大草坪上看星星。  
有强迫症的 oxx 想要使得他的小姐姐们正好躺成一块$n×m$的长方形。  
已知小姐姐的形状是$1×p$的长方形（可以横着或竖着躺）。小姐姐从$1$到$nm$编号总共有$nm$个（如果可以的话，绝对够用）。  
P.S. 小姐姐是$1×p$的是因为她们比较苗条。  
<!--more-->
## 输入说明
输入三个整数$n, m, p$($1≤n,m,p≤100$，$p$是质数)。

## 输出说明
如果不行，输出`No`。  
否则输出`Yes`。随后输出$n$行$m$列正整数用空格隔开。同一个小姐姐用相同的数字表示，不同的小姐姐用不同的数字表示。数字应是在$[1，nm]$范围内的正整数。同一个数字至多出现$p$ 次，这$p$次应该在横向连续，或者纵向连续。
如果有多解输出任意一解。  

## 输入输出示例
见原题目（数据太多了，懒得复制）  
[EOJ Monthly 2018.10 - A. oxx 的小姐姐们](https://acm.ecnu.edu.cn/contest/113/problem/A/)

## 满分暴力
我的思路：先放竖着的姐姐，然后扫一遍地图看看还有没有空余，若有空余就放横着的姐姐。由于事先判断了是否能刚好放成$nm$的长方形，因此之后就大胆的摆即可。当然全部摆好了之后也不忘记严谨的扫一遍图，看有没有剩下没放了（虽然没有实际作用了）
```cpp
#include<iostream>
#include<cstdio>
using namespace std;
const int MAXN=110;
int n,m,p;
int map[MAXN][MAXN];
int rewn,rewm,tmpn,tmpm;
int main(){
	scanf("%d%d%d",&n,&m,&p);
	if((n*m)%p){//p不是nm的倍数肯定就放不了了
		puts("No");
		return 0;
	}
	int count=0;//小姐姐计数器
	if(p<=n){//如果竖排放得下
	    int tmp=1;//在当前这一高度时横着摆了几个竖排
	    int h=1;//当前摆到第几行了
	    while(h<=n-p+1){//一直放小姐姐直到当前行放不下为止
	        while(tmp<=m){//在这一行一直放小姐姐直到放满这一列
	            if(!map[h][tmp]){//（其实没啥用）如果当前行为空
	                ++count;//小姐姐计数器++
	                for(int i=h;i<=h+p-1;i++){
	                    map[i][tmp]=count;//纵向放置小姐姐
	                }
	            }
	            ++tmp;
	        }
	        h+=p;//调到下一高度
	        tmp=1;
	    }
	}
	bool flag=true;//flag=false
	for(int i=1;i<=n;i++){
	    for(int j=1;j<=m;j++){
	        flag&=map[i][j];//flag|=map[i][j];
	    }
	}
    //扫一遍图。。。注释里是第一遍时写的，，滑稽地卡在了第七个点
	if(!flag && p<=m){//如果还没放满，那么接着放横向小姐姐
	    int tmp=1;
	    int w=1;
	    while(w<=m-p+1){
	        while(tmp<=n){
	            if(!map[tmp][w]){
	                ++count;
	                for(int i=w;i<=w+p-1;i++){
	                    map[tmp][i]=count;
	                }
	            }
	            ++tmp;
	        }
	        w+=p;
	        tmp=1;
	    }
	}
   flag=true;
	for(int i=1;i<=n;i++){
	    for(int j=1;j<=m;j++){
	        flag&=map[i][j];
	    }
	}
    //再扫一遍图
	if(!flag){//如果仍然没放满那么输出No
	    puts("No");
	    return 0;
	}
	puts("Yes");
	for(int i=1;i<=n;i++){
	    for(int j=1;j<=m;j++){
	        cout<<map[i][j]<<' ';
	    }
	    cout<<endl;
	}
	return 0;
}
```