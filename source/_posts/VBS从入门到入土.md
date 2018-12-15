title: VBS从入门到入土
author: MarkJuruo
tags: []
categories:
  - 杂记
date: 2018-10-03 17:24:00
mathjax: true
---
首先，想学VBScript的同学们必须知道，如果是真的想要学好编程的，那么VBSCript不该作为长久的选择，学习VBScript为的是基本了解编程语言中的基本语法（所有的程序语言语法都是相通的）。VBScript上手快，成效快，因而很适合程序新手学习。  
开始学习前希望您具备以下基本条件：
- [x]有一定英语基础
- [x]有一定耐心
- [x]Windows操作系统（本人不清楚Linux/Mac是否可以）
- [x]本文针对程序小白而写，请各位大佬快速护目
<!--more-->

## 如何编写/编译VBScript程序
由于我们现在学习的VBScript没有专门的编辑器，于是我们可以选择在记事本/写字板中书写程序。
![在记事本中编写VBScript程序](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS1.jpg)
写好一个VBScript程序之后，只要把该文件的后缀名改为.vbs即可，计算机会自动编译。要注意，这边一定要把文件类型选择为所有类型，要不然会有些不可描述的错误。
![保存VBS文件](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS2.jpg)
如果需要再次编辑同一个VBScript文件的话，可以右击该VBScript文件，然后选择【打开方式】，选择记事本即可。

## 输出
其实这样写的话顺序是有点问题的，理论上应当先讲变量。但是鉴于同学们希望早点看到自己写的程序的作用，就先把最基本的输出讲掉。  
```vbs
Msgbox "Boy♂Next♂Door"
```
同学们吧以上语句复制到文本框中然后按以上讲的方法编译。点开来之后，同学们会发现自己的第一个程序已经可以正常运行了。  
这个程序的意义就是：把"Boy♂Next♂Door"这一串字串放到消息提示框里输出。`Msgbox`（MessageBox)，就是一个保留关键字。

### 什么是关键字？
关键字就是VBScript中可以进行操作的单词/符号。可以理解为英语中的动词，告诉你接下来要干什么。在VBScript中，关键字没有大小写规范。形象一点的说，就是前面的讲到的那个最基本的`Msgbox`有以下几种**正确**写法。  
```vbs
msgbox
MsgBox
msgBox
MSGBOX
and more...
```
在具体的程序实现中，同学们就可以根据自己个人的习惯选择写法。

## 变量
变量很好很好理解，但是很难彻底搞懂它的用途，而且如果是第一次听到的话——  
> **“这种累赘的东西，不如不要”——鲁迅**

![鲁迅](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1538389500476&di=18e7e3aff7fa97b5df2e9682424ffbf2&imgtype=0&src=http%3A%2F%2Fp0.ifengimg.com%2Fpmop%2F2017%2F1117%2FB7DC518906A75A682ECECA24BE5CB857247A9FB1_size17_w500_h321.jpeg)
**先看下面一个例子**  
```vbs
dim a
a="Boy♂Next♂Door"
Msgbox a
```
简单的说，变量就是一个箱子，里面装着一个东西，我们称这个东西为该变量的值。在引用某个变量的时候，事实上用的变量的值，也就是说，上面那个程序里面的`Msgbox a`和直接`Msgbox "Boy♂Next♂Door"`等效。

### 变量的类型

讲到变量就不得不提一个很影响人的心情的知识点，这也是学习过程中的第一个难点——**变量的类型**。变量的类型是由变量里具体存的值决定的。比如我们刚讲到的那个程序：
```vbs
dim a
a="Boy♂Next♂Door"
Msgbox a
```
其中a就是一种**字符串类型**，我们可以简单的理解为它的值是由许多字符组成的。
```vbs
dim a
a=1
Msgbox a
```
此时，`a=1`，那么a就是一个数字类型了。  
由于我们学的VBScript语言没有严格的变量规定，于是我们都用`dim`来声明变量，然后给这个变量赋值后计算机会根据其值自动判断其变量类型。在我的母语C++中，变量有着非常严格的要求，不同的类型有不同的关键字来声明，大家感受一下即可：
```cpp
//这是C++语言，大家不必深入了解
int a;//数字类型
bool b;//布尔类型（false/true）
char c;//字符类型（只有一个字符）
string d;//字符串类型（有多个字符）
```
### 不同类型变量间的运算符
提一句，在VBScript程序中出现`rem`，就便是其后面的语言都是用于注释，也就是不会被执行。
```vbs
dim a,b,c,d rem 声明a,b,c,d四个变量
a=1
b=1
c="Boy♂NexT♂"
d="Door"
Msgbox a+b
Msgbox c+d
Msgbox a&c
```
这个程序中a,b都为数字类型，于是他们进行+运算就是和我们数学上的运算一样，但c和d是字符串类型，所以他们进行+运算就是把这两个字符串连接在一起。最后一句：`Msgbox a&c`这边a和c之间用了&连接（shift+7)，是因为它们的类型不一样，**当类型不同又想在同一个MessageBox中输出，就得用&把他们连接在一起。**当然，在MsgBox中同类型也可以用&链接
```vbs
dim a,b rem 声明a,b两个变量
dim c
a=1
b=2
c=a+b
Msgbox a&"+"&b"="&c
```
此时,我们又声明了一个变量：c来保存a+b的值。因为a和b都是数字类型，于是c也自然而然的为数字类型了。输出的时候注意"+"和"="都是字符（串）类型，所以a,b,c和"+"，"="的类型不同，在Msgbox中得用&连接。

#### 值得思考的问题

```vbs
dim a,b,c rem 声明a,b,c三个变量
a="1"
b="2"
c=a+b
```
在这样一个程序中，c=a+b，由于a,b都是字符（串）类型，导致c也是字符串类型。前面讲到字符串运算中，+表示连接，因此，c的值此时就为"12"。问题来了：**如何让c的值为数字a+数字b呢？（也就是如何令c=1+2的值而非"1"+"2"的值）**。我们有一种特殊的处理方法：
```vbs
c=0+a+b
```
此时，计算机发现0是一个数字，也就会默认把c的值确定为数字类型了。

## 输入
之所以先讲变量，就是为了为这里的输入做铺垫。从这里开始，我们学的程序才真正开始有点意思了。
```vbs
dim a
a=Iputbox("输入些东西。。。")
Msgbox a
```
这个程序里多了个新东西——`Inputbox`。也就是输入框。这个输入框的作用就是让用户输入信息，然后把用户输入信息的值赋值给变量前面的变量。而Inputbox里面的东西就是提示信息。

### 课后练习-小试牛刀
尝试写一个程序，达到以下效果：  
![示例1](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS3.jpg)
![示例2](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS4.jpg)
![示例3](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS5.jpg)
请认真完成后再打开[**正解**](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/first.vbs)

## If-Else判断语句
接下来是更加有意思的了，并且接下来我们学习的程序将会变得长♂起来。  
假如我想实现这么一个程序：让用户输入一个值，假如值为“markjuruo”，则输出“他是个大帅哥”，否则输出“他是头大笨驴”，该如何实现？  
我们先来看一下正解：
```vbs
dim name
name=Inputbox("你叫什么？")
If name="markjuruo" Then
Msgbox "他是个大帅哥"
Else
Msgbox "他是头大笨驴"
End If
```
这里有可能会略微有点难以理解，但不要急，我会慢慢讲。

### If的三种形态
```vbs
If 条件 Then
运行程序
End If
```
当条件成立时，从`Then`到`EndIf`间的所有程序才会被执行。
```vbs
If 条件 Then
运行程序
Else
运行程序
End If
```
如果条件成立，那么运行从`Then`到`Else`之间的所有程序。如果条件为假，那么运行从`Else`到`EndIf`之间的所有程序。
```vbs
If 条件1 Then
运行程序
ElseIf 条件2 Then 
运行程序
Else
运行程序
End If
```
**这是if语句的究极形态，学会了它就可以为♂所♂欲♂为！**。如果条件1成立，那么执行从`If 条件1 Then`到`ElseIf`之间的内容，如果条件2成立，那么执行从`ElseIf 条件2 Then`到`Else`之间的内容。如果条件1和条件2都不成立，那么就
执行从`Else`到`EndIf`之间的内容。  
需要注意的是，这里可以有多个`ElseIf`出现，比如：
```vbs
If 条件1 Then
运行程序
ElseIf 条件2 Then 
运行程序
ElseIf 条件3 Then
运行程序
ElseIf 条件4 Then
Else
运行程序
End If
```
并且，在`If,ElseIf`的结构中可以省略`Else`，比如：
```vbs
If 条件1 Then
运行程序
ElseIf 条件2 Then 
运行程序
End If
```
**注：以上所有`If`，`End If`，`Else`，`ElseIf`的大小写全部可以自己决定**
这边确实有一点难以理解，但是稍加思考，肯定可以成功的。  
至此，我们再回头看那刚开始学的程序：
> 假如我想实现这么一个程序：让用户输入一个值，假如值为“markjuruo”，则输出“他是个大帅哥”，否则输出“他是头大笨驴”，该如何实现？  

```vbs
dim name rem 声明变量name
name=Inputbox("你叫什么？") rem 输入name的值
If name="markjuruo" then
Msgbox "他是个大帅哥" 
Else
Msgbox "他是头大笨驴"
End If
```
需要注意的是，`If`中的`=`表示判断两个数是不是相等，如果相等，那么条件式成立。例如该程序中`If name="markjuruo" Then`，就是当name的值为"markjuruo"时，条件式成立。**当然喽，这里的比较只是同类型之间的，如果类型不相同的话就肯定不相同，比如13不等于“13”**

### 课后练习-自制计算器问题
首先输入两个数，然后再输入计算的符号（中文的“加”，“减”，“乘”，“除”），最后输出计算的结果。同时判断如果输入的计算符号不合法（就是不为“加”，“减”，“乘”，“除”中的任何一个），那么输出“Are You Killing Me ? ? ? ”    
由于可能~~对你们来说~~难度有点大，我给出一点提示：
**写法1** | 先把答案保存在变量ans中，最后输出（这是一种好习惯）
```vbs
dim num1,num2,task,ans rem 数字1，数字2，计算符号，结果
num1=inputbox("输入数字1")
num2=inputbox("输入数字2")
task=inputbox("输入计算符号")
if task="+" Then
ans=0+num1+num2 rem 前面加上0，强制转为数字相加
ElseIf task="-" Then
...
...
Else
Msgbox "Are You Killing Me ? ? ? "
EndIf
Msgbox num1&task&num2&"="ans rem 想一想这一句的效果
```
**写法2** | 算出来就输出
```vbs
dim num1,num2,task rem 数字1，数字2，计算符号
num1=inputbox("输入数字1")
num2=inputbox("输入数字2")
task=inputbox("输入计算符号")
if task="+" Then
Msgbox num1&task&num2&"="&(0+num1+num2)
ElseIf task="-" Then
...
...
Else
Msgbox "Are You Killing Me ? ? ? "
EndIf
```
**[正解](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS.vbs)**  
**超高难度变式** | 前面不变，但是除法要算出整除得到的商和余数。（取余符号：mod，比如8 mod 3=2，7 mod 2=1）

## 阶段性技巧总结
计算符号问题：在VBScript中，基本的加减乘除四则运算符号为`+,-,*,/`不可以用×或÷。求余符号为`mod`。其它的运算暂时不学习。
****
用了一段时间的`Inputbox`之后同学们会发现，不论用户往`Inputbox`中输入什么数据，它总会把这个数据变成字符串类型的。比如你输入一个13，它却会给你“13”，在之后运算的过程中还得+0处理才能当做数字使用，十分不雅观。于是我们可以在输入的时候就处理掉：
```vbs
dim number
number=Inputbox("输入一个数字")+0
```
这时，number就可以直接作为数字使用。这里可能有同学不明白了,`Inputbox()`怎么还能+0呢？这里要好好理解，`Inputbox()`其实也是有值的，它的值就是用户输入的值。接下来学到函数就会明白，`Inputbox()`是一个函数，它是有返回值的，当然现在不必深究，了解即可，应当循序渐进。
****
在`Msgbox`中如何换行？？很简单，只要在需要换号的地方加上`chr(13)`就行了。
```vbs
Msgbox "Hello!"&chr(13)&"How are you?"
```
这种方法当然在`Inputbox`中等效：
```vbs
dim ipt
ipt=Inputbox("Hello!"&chr(13)&"What's your name?")
```
****
`If`中的判断语法有哪些？上次我们在讲到`If-Else`语法中只提到一种判断方式：判断两个（同类型的）变量的值是否相同。此处拓展一些：
```vbs
dim a,b
a=1
b=2
If a=b then rem 判断是否相等
ElseIf a<b then rem 判断a是否小于b
ElseIf a>b then rem 同理
ElseIf a<>b then rem 判断a是否不等于b
```
别的当然还有很多，但此处不做拓展。

## 循环语句

请务必掌握了前面讲到的所有语法后再学习本节内容  
很好，你已经学会了许多VBScript用法了！但是光凭以上提到的语法，程序的质量和作用都不会很高。现在，我们要来学习一个分重要的语法：循环语句。  
先来看一个小问题：让你将1~10分别输出，该怎么做？  
凭借我们之前学习的语法，自然而然就会想到：
```vbs
Msgbox "1"
Msgbox "2"
Msgbox "3"
...
Msgbox "9"
Msgbox "10"
```
那么，如果我要输出1~100呢？  
甚至是1~1e7呢？
很明显，上述方法的效率就显得过于低下了。我先给出正解，之后会细讲语法，这一块内容有很强的综合性，我们将会结合之前学过的`If`语法进行学习。  
在这一节，我们将会接触两种循环方式，以上题为例：  
**For循环**
```vbs
For i=1 To 10
Msgbox i
Next
```
**Do-Loop循环**
```vbs
dim i
i=0
Do
i=i+1
Msgbox i
If i=10 Then
Exit Do
End If
Loop
```
### Do-Loop循环
鉴于这个循环使用到了我们之前所学习的`If`语法，可能会更好理解些，故而先讲。  
```vbs
Do
运行程序（循环体）
Loop
```
`Do-Loop`循环十分好理解，就是把`Do`和`Loop`中间的程序一直重复执行。鉴于这样一直运行会让人不爽，我们就需要在一定的条件下终止运行。  
```vbs
Do
运行程序（循环体）
If 条件满足 Then
Exit Do
End If
Loop
```
这时候，我们原先学习的`If`语法就起了极大的作用。当满足条件时，执行语句`Exit Do`。`Exit Do`，顾名思义，就是退出循环。例如我们刚看到的那个示例程序：
```vbs
dim i
i=0
Do
i=i+1 rem 自增1，这个好好理解下
Msgbox i rem 输出i
If i=10 Then rem 当i等于10时，退出循环
Exit Do 
End If
Loop
```
这里面有一个语法：`i=i+1`，这个需要好好理解，有的同学可能会这样想：比如i=1，那岂不是`1=1+1`了吗？**注意，i是一个变量！在执行`i=i+1`时是把`i+1`的值赋给了i。**当然如果实在理解不了，那就这样写：
```vbs
dim x
x=i
i=x+1
```
这样分开写就更好理解了。

### For循环
这个循环将会非常常用，但理解起来可能有点复杂。
```vbs
For i=1 To 10
运行程序（循环体）
Next
```
在这个程序中，i充当了计数器的作用。`For i=1 To 10`也就是i从1到10循环，每次循环都把`For`与`Next`之间的内容执行一遍。应当注意这里的i不需要提前声明，在`For i=1 To 10`中，i已经被系统自动声明了。  
这里有一个重点是：这个程序中i只能在循环体内使用（涉及到了全局变量和局部变量，比较复杂，好好理解）。也就是：
```vbs
For i=1 To 10
Msgbox i rem For中声明的i变量只能在For-Next之间用
Next
Msgbox i rem 在这里用i系统会报错，因为For结束后i变量会被自动清空
```

**拓展** | 如何才能让i每一次递增k  
之前讲到的For循环，每一次都是默认递增1的，但是如何做到每一次都递增k（即任意实数）呢？  
举一个比较实际的例子：试写一个程序，输出0~n之间的偶数。我这里直接放出正解：  
```vbs
dim n
n=Inputbox("输入n的值")
For i=0 To n Step 2
Msgbox i
Next
```
注意到了吗？`For i=0 To n`的后面加上了`Step 2`,这就表示每次都递增2。  
当然喽，这种`For-Next`语句中很多量都是可以自定义的：
```vbs
For OK=OK To OK Step OK
```
其中标注`OK`的都是可以自己调控的量。  

**接着拓展** | 如何倒过来输出0~n的偶数？即以n,n-2,n-4...4,2,0的顺序输出  
这个问题乍一看摸不着头脑，但只要掌握了前面讲的“`For-Next`循环很多量都是可以自己定义的”就不难解决了。  
```vbs
dim n
n=Inputbox("输入n的值")
For i=n To 0 Step -2
Msgbox i
Next
```
怎么样？是不是恍然大悟？  

**再来个拓展** | 如何根据特殊条件终端For循环？  
还记得`Exit Do`吗？相信同学们猜得到结束For循环就是`Exit For`了。而根据特殊条件嘛，就是一个简单的If判断，轻松解决，此处不多赘述了。

### 课后练习-求和问题
首先用户输入一个n，接下来输入n个数，最后请输出这n个数的和。**温馨提示**：别忘了强制转为数字类型  
[**正解**](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS求和问题.vbs)

**超高难度变式** | 这个若是~~对你们来说~~能自己做出来就不得了了
前面都一样，但是最后输出的形式为$x_1+x_2+x_3+...+x_n=sum$，其中$x_i$表示输入的第i个数，$sum$表示它们的和。例如：“1+4+5+2=12”  
> 提示一下，还记得字符串怎样相加的吗？可以设置一个字符串，每一次循环都在该字符串的最后加上“+$x_i$”，最后就可以得到一个字符串形如"$x_1+x_2+x_3...+x_n$"  

[**正解**](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS求和问题-变式.vbs)

### 课后练习-求最大值问题
首先用户输入一个n，接下来输入n个数，最后请输出这n个数中的最大数为多少。（这道题~~对你们来说~~也蛮难的）
> 提示一下，可以设置一个最大值max，然后先让这个max等于第一个输入的数，然后之后每一次输入都比较max和输入的数，如果max比输入的数小就更新max的值。  

[**正解**](https://coding.net/u/linzhihan/p/blogimg/git/raw/master/VBS求最大值问题.vbs)

## 函数
大部分语言都有函数，VBScript也不例外。当然，这里讲的函数和我们数学上的不大一样。**在VBScript中，函数就是定义一段过程**。我们来放轻松，看一段简单的函数代码：
```vbs
Function AWC()
Msgbox "Ass We Can"
End Function
```
这就是一个函数。简洁，清晰。那么写好这个函数之后，在程序中我们就可以这样用它：
```vbs
AWC()
```
我来举个十分有意思的比喻：我们一般的写程序，可以理解为堂食，买来了就吃，而函数就类似于打包，买来了，要吃的时候再拿出来吃，而且想吃多少次就吃多少次。

### 带参数的函数
讲到函数就不得不将参数。参数是什么？我们来看看下面这个简单的例子：
```vbs
Function Say(dim sb,dim say) rem 这个s.b.是somebody的意思
Msgbox sb+"说"+say
End Function
```
这里，sb和say就是参数。参数就相当于函数运算过程中需要用到的变量，可以有，也可以没有，可以多，也可以少。那么带参数的函数，我们在引用的时候是不能加上括号的：
```vbs
Say "我","AssWeCan"
```
（这里比较难理解，我也不知道我讲清楚了没，如有不懂的在下面提问即可）

### 课后练习-求阶乘和问题
这道题是我们信奥生在初学的时候的必做题，~~对我们来说~~十分简单，至少它没有涉及算法内容，放到这里来，给大家练练手。  
让用户输入一个n，然后输出$1!+2!+3!+4!+...+n!$的值。
>提示：计算每个数的阶乘的过程可以写成函数。

## 数组
很好，你已经学到数组了。  
这是一个很有趣的东西，而且我懒的解释，大家自己看程序就能立马明白它的用途：
```vbs
dim array(3)
array(1)="我"
array(2)="是"
array(3)="你爸爸"
Msgbox array(1)+array(2)+array(3)
```
是不是很简单？不就是几个量的集合嘛！

### 动态数组

//不行，我实在写不动了。。。


（持续更新中。。。）