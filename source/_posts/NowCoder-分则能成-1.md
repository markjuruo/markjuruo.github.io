title: '[NowCoder] 分则能成(模拟)'
author: MarkJuruo
tags:
  - 模拟
categories:
  - 学习
date: 2018-09-22 09:23:00
---
## 题目描述
牛牛刚开始有一个正整数n。
每次操作牛牛可以选择一个自己有的数字x，把x分为两正整数y和z，需满足x=y+z，然后获得y×z的收益。  
（当然，在这个过程中，牛牛会失去x这个数字，并且获得y和z这2个数字。）  
牛牛一共可以分k次，牛牛希望最大化这k次的收益之和。  
因为分割的结果y和z是正整数，所以选择的x必须>=2。  
<!--more-->
对于100%的数据，1 <= k < n <= 109  
对于40%的数据，1 <= k < n <= 10  
对于70%的数据，1 <= k < n <= 100  

### 输入描述
> 输入只有一行，包含用空格分开的两个整数，表示n和k。

### 输出描述
> 输出一行一个整数，表示答案。

### 示例数据
输入
> 9 2

输入
> 27

说明
>刚开始牛牛有{9}
把9分为3和6，牛牛现在有{6, 3}
把6分为3和3，牛牛现在有{3, 3, 3}
总收益为6*3+3*3 = 27。

## 解题报告
第一次提交，我想都没想就写了个全部平均分
```cpp
#include<iostream>
#include<cstdio>
#include<vector>
using namespace std;
vector<int> vec;
int findmax(){
    int maxn=-1;
    for(int i=0;i<vec.size();i++){
        if(maxn<vec[i]){
            maxn=vec[i];
        }
    }
    for(int i=0;i<vec.size();i++){
        if(vec[i]==maxn){
            vec[i]=0;
        }
    }
    return maxn;
}
int main(){
    int tasknum,n;
    int ans=0;
    cin>>tasknum>>n;
    vec.push_back(tasknum);
    for(int i=0;i<n;i++){
        int x=findmax();
        int y,z;
        if(x%2==0){
            y=z=x/2;
        }
        else{
            y=x/2+1;
            z=x/2;
        }
        cout<<y<<","<<z<<endl;
        ans+=y*z;
        vec.push_back(y);
        vec.push_back(z);
    }
    cout<<ans;
}
```

（持续更新中...）