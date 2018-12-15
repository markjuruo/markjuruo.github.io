title: '[Rewrite] 洛谷博客上的存档(桶排序/Stack/模拟/打表)'
author: MarkJuruo
tags:
  - 桶排序
  - Stack
  - 模拟
  - 打表
categories:
  - 学习
  - 归档
date: 2018-07-01 19:29:00
mathjax: true
---
## 题解 P1554 【梦中的统计】

看了这么多大佬的代码之后我实在忍不住了，决定站出来给各位同学讲一讲一种可以稍微提升逼格的做法(大佬们可以自顾自跳过了)，这道题其实就是桶排序，用一个数组比如Count[10]来登记0~9每个数字出现的个数。在这边为了看起来比较美观，可以用函数来写。函数是C++中比较重要的东西了，我们写的int main()或int WINAPI WinMain()其实就是一个函数，我们的程序一般习惯先运行main()函数。我们也可以自己来写一个函数，函数的用法详见：[【菜鸟教程】C++函数教程](http://www.runoob.com/cplusplus/cpp-functions.html)。这里我们可以写一个CntPlus函数，把每一个数中0~9的个数都放到一个专门的数组里就可以了，所以和桶排序是密不可分的，关于桶排序的用法详见：[C++桶排序教程](https://blog.csdn.net/u010884939/article/details/40709039)（该文章出处：`https://blog.csdn.net/u010884939/article/details/40709039`）。
<!--more-->
### 函数体

我个人是这样来构造函数的：
```cpp
void CntPlus(int Num,int Cnt[10])
{
    while(Num!=0)
    {
        Cnt[Num%10]++;//体现桶排序的关键语句
        Num/=10;
        /*
         *以上这种算法在提取一个数的每一位时
         *经常用到，用while循环反复对Num进行
         *Num/=10,Num%10的处理，就可以提取出
         *数的每一位，%是求余，/是整除，同学
         *不理解的话可以自己手算一下。
         */
    }
}
```
### 题解源码

好了，既然有了函数，就可以用了。在主函数中，从n到m循环，然后对每一个数都进行此操作，整个算法就变得十分简单轻松愉快了。最后代码奉上：
```cpp
#include<iostream>
using namespace std;
void CntPlus(int Num,int Cnt[10])
{
    while(Num!=0)
    {
        Cnt[Num%10]++;
        Num/=10;
    }
}
int main()
{
    int n,m;
    int Cnt[10]={0};//数组清零
    cin>>n>>m;
    for(int i=n;i<=m;i++)
        CntPlus(i,Cnt);//对该区间每个数都进行操作
    for(int i=0;i<=9;i++)
        cout<<Cnt[i]<<" ";//直接输出
}
```

***

## 题解 P1469 【找筷子】
### 【STL大发好】栈解法
看到这道~~水~~题我第一眼就想到了栈，可惜还是需要用到排序，纯栈的话会很难~~`也有可能是我太蒻了`~~

**简单讲一下思路**
1. 输入
2. 排序
3. 边入栈边去重
4. 输出
6. ~~提交等待AC~~

**关于栈**
- [一个大佬写的栈的教程](https://www.cnblogs.com/QG-whz/p/5170418.html)

**Code**
```cpp
#include<iostream>//C++标准输入输出流
#include<algorithm>//算法库，包含排序函数
#include<stack>//STL大法之栈的专用库
using namespace std;
int main()
{
    stack<int>st;//创建栈st
    int n;//有n只筷子
    cin>>n;//读入n
    int kuaizi[n];//每个筷子的长度
    for(int i=0;i<n;i++)
        cin>>kuaizi[i];//读入每个筷子的长度
    sort(kuaizi,kuaizi+n);//排序
    for(int i=0;i<n;i++)
    {
        if(i==0)//如果是第一只筷子就直接入栈
            st.push(kuaizi[i]);
        else
        {
            if(!st.empty() && kuaizi[i]==st.top())
                st.pop();//匹配到相同筷子，就出栈
            else
                st.push(kuaizi[i]);//没找到就先入栈
        }
    }
    cout<<st.top();//没被匹配到的筷子就会被留在栈顶
}
```

***

## 题解 P1319 【压缩技术】

这样一道入门题目，本来可以用for循环直接操作，但作者异想天开(xian de dan teng)地把所有数据登记在一个数组里面，然后再统一按格式输出。也就是定义一个数组Map，大小为n成n，然后按照输入数据，把Map中每一个点改为0或者是1，然后根据题目要求的格式输出。比较简单，就直接贴代码了。
### 代码部分
```cpp
#include<iostream>
#include<cstring>//事实上没有用
using namespace std;
int main()
{
    int n;
    cin>>n;
    //矩阵长宽
    int Map[n*n+10];
    //设置一个登记用的数组，其实n*n就够了，+10是为了防爆
    int Full;//输入要用的
    bool Key=false;
    //判断当前输入的是1的数量还是0的数量，初始为0
    int p=0;
    //已经登记到第几个数，类似于指针
    while(cin>>Full)//持续输入
    {
        int i;
        for(i=p;i<p+Full;i++)
            Map[i]=Key;//把这一块区域登记为Key，就是0或1
        p=i;
        Key=!Key;
        //本文唯一难点，
        /*
         *“!”表示相反，
         *如果原先为true就变为false
         *如果原先为false就变为true
         *这里就把“0和1的数量交替输入”体现出来了
         */
    }
    p=0;
    //指针归零，下面要开始从头开始输入
    for(int i=0;i<n;i++)
    {
        for(int j=0;j<n;j++)
        {
            cout<<Map[p];
            p++;
            //输出
        }
        cout<<endl;
        /*按格式输出*/
    }
        
}
```
### 知识点
```cpp
Key=!Key
```
这是C++中独有的运算符，C++党们注意了，这是本文中唯一的高逼格用法了，“!”的作用就在于可以翻转原先数据的值，真变为假，假变为真。而C++中非0即为真，这样在程序中就把“其余各位表示交替表示0和1 的个数”体现的淋漓尽致(he he he he)了。

***

## 题解 P1603 【斯诺登的密码】

我个人认为这道题的重点在于打表，所以这边就教大家（大佬们可以跳过了）一个打表高颜值的打表方式：struct(结构体)。

结构体(struct)是由一系列具有相同类型或不同类型的数据构成的数据集合，也叫结构。一般我们会这样声明：
```cpp
struct Box;
```
在C语言中则一般是：
```c
typedef struct Box;
```
然后你可以往里面放东西，比如：
```cpp
struct Student
{       
 int num;         //声明一个整形变量num 
 char name[20];   //声明一个字符型数组name 
 char sex;        //声明一个字符型变量sex 
 int age;         //声明一个整形变量age 
 float score;     //声明一个单精度型变量 
 char addr[30];   //声明一个字符型数组addr 
};
```

建立好结构体了，就要试着用你的结构体定义，像这样：
```cpp
Student XiaoMin;
```
此时就定义了一个结构体变量：XiaoMin。如果你需要调用其中的的某个值得话，可以像这样操作：
```cpp
XiaoMin.name="XiaoMin";
XiaoMin.score=13.5;
```
或者这样：
```cpp
XiaoMin={13,"XiaoMin",'M',14,13.5,"hello"};
```
接下来请各位同学打开C++手打一遍自行领会。

更多关于Struct的知识可以学习：
[C++Struct](https://blog.csdn.net/steft/article/details/54944790)

### 如何打表

讲了struct，就可以来进行高级打表了，首先建立一个结构体：
```cpp
struct Table
{
    string Word;//单词内容
    int value;//该单词的值
};
```
然后定义一个表：
```cpp
Table Key[40];
```
然后就要使用你灵敏的小手指......
```cpp
Key[40]={
    {"one",1},{"two",2},{"three",3},
    {"four",4},{"five",5},{"six",6},
    {"seven",7},{"eight",8},{"nine",9},
    {"ten",10},{"eleven",11},{"twelve",12},
    {"thirteen",13},{"fourteen",14},{"fifteen",15},
    {"sixteen",16},{"seventeen",17},{"eighteen",18},
    {"nineteen",19},{"tewnty",12},
    //此上为正规部分
    {"a",1},{"both",2},{"another",1},
    {"first",1},{"second",2},{"third",3}
    //此上为非正规部分
};
```
### 如何查找
打完表之后，每输入一个单词，就对这个单词进行搜索，搜到的话，就按题目要求进行计算，否则可以直接跳过接下来的步骤。在这里作者采用的是普通(ruo zhi)的遍历查找，如下：
```cpp
bool SearchRight=false;
cin>>Full;
int j=-1;
do//在范围内查找该单词
{
	j++;
	if(Full==Key[j].Word)//如果查找到了
	{
        SearchRight=true;//,登记查找到了
   	 break;
	}
}while(j<26);//在规定范围内查找
if(!SearchRight)
/*
 *如果未查找到，
 *效果与 if(SearchRight==false) 相同*/
	continue;//直接返回，输入下一个单词
```
接下来的操作相信大家都是没有困难的了。那么最后源码给上。

### 题解源码（抄袭者后果自负）

```cpp
#include<iostream>
#include<cstdio>
#include<cstring>
using namespace std;
struct Table;//简单的建立一个结构体用来将单词与值对应
//C++中 typedef struct可以简写为 struct
struct Table
{
    string Word;//单词内容
    int value;//该单词的值
}Key[40]={
    {"one",1},{"two",2},{"three",3},
    {"four",4},{"five",5},{"six",6},
    {"seven",7},{"eight",8},{"nine",9},
    {"ten",10},{"eleven",11},{"twelve",12},
    {"thirteen",13},{"fourteen",14},{"fifteen",15},
    {"sixteen",16},{"seventeen",17},{"eighteen",18},
    {"nineteen",19},{"tewnty",12},
    //此上为正规部分
    {"a",1},{"both",2},{"another",1},
    {"first",1},{"second",2},{"third",3}
    //此上为非正规部分
};//简单的打表
int main()
{
    string Full;
    bool FirstPrint=true;//登记是否是第一次输出
    bool SearchRight=false;//登记是否查找到
    for(int i=0;i<7;i++)
    {
        SearchRight=false;//初始设置为未查找到
        cin>>Full;
        int j=-1;
        do//在范围内查找该单词
        {
            j++;
            if(Full==Key[j].Word)//如果查找到了
            {
                SearchRight=true;//登记查找到了
                break;
            }
        }while(j<26);//在规定范围内查找
            
        if(!SearchRight)
            continue;//直接返回，输入下一个单词
        int PrintAns=(Key[j].value*Key[j].value)%100;
        if(FirstPrint)//效果等同于 if(FirstPrint==true)
        {
            /*检测是否是第一次输出，如果是的话，就不保留前导0*/
            printf("%d",PrintAns);
            FirstPrint=false;//登记，第一次输出结束
        }
        else
            printf("%02d",PrintAns);//保留前导0
    }
    if(FirstPrint)
    /*因为凡是输出过的都会登记FirstPrint为false，
     *所以如果FirstPrint为真(true)
     *就说明此时还没有输出
     *根据题目要求，要输出0*/
        cout<<0;
    return 0;
}
```