title: 如何用 Python 实现基于 UDP 协议的聊天工具？
author: markjuruo
date: 2018-12-09 12:14:23
tags:
---
## 前言

最近刚自学的 Python ，有 C++ 的基础上手还算快。Python 的网络库使用十分简便，此文简单记录基于 UDP 协议的聊天程序编写过程。  
相比 TCP-IP 协议， 使用 UDP 协议写出的程序在我看来结构更加清晰，且也比较方便之后多线程的加入。

> **主要思路**  
服务器端负责处理信息转发，将某个客户端发来的消息转发到别的客户端，以达到信息交换的功能，即简单的聊天功能  
客户端需要做到接收消息和发送消息，并且这两个进程必须是同时进行的。  

<!--more-->

## 尝鲜

[**服务器下载**](https://github.com/markjuruo/Python-UDP-Chating/blob/master/server.exe?raw=true)  
[**客户端下载**](https://github.com/markjuruo/Python-UDP-Chating/blob/master/client.exe?raw=true)

效果图：

![](https://img2018.cnblogs.com/blog/1532635/201812/1532635-20181215122336324-329967802.jpg)

## 简单架构

![结构图](https://img2018.cnblogs.com/blog/1532635/201812/1532635-20181212212525334-1172894300.jpg)

### 基于 UDP 协议编写服务器端

不得不说，使用 UDP 协议会比 TCP-IP 协议简洁很多，最核心的部分便是 `recvfrom()` 和 `sendto()` 。与 TCP-IP 协议下的 `recv()` 和 `send()` 不同，UDP 下能够更加明确的获取收到信息的内容和来源。  
以下便是一个简单的基于 UDP 协议的服务器模型。


```python
import socket # 引用网络库
from sys import exit # 从 System 借一个 exit() 函数

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # 声明对象 s

HOST = '127.0.0.1' # 服务器 IP 地址
PORT = 10888 # 服务器端口

s.bind((HOST, PORT)) # 启动服务器

while True: # 进入消息循环
    (data, addr) = s.recvfrom(1024) # 接收消息
    print("Recv-data", data.decode('utf-8')) # 将收到的消息转码并输出
    s.sendto(data, addr) # 将消息发回

```

需要注意的是 HOST 里的值是需要改的。把它改成你自己这台电脑的 ip 地址（至于如何查询本机 IP 地址请自行百度，或者你可以在 cmd 中输入 ipconfig 找到 “IPv4地址” 一项）

不难看出，`recvfrom()` 函数获取的是一个元组，包含了数据和地址两个信息，因此我们使用 `data,addr` 将其保存。  
在输出时，`data.decode('utf-8')` 的操作与下面客户端 `data.encode('utf-8')` 的操作相对应。在信息传输的过程中，我们需要声明用哪一种编码进行传输和保存。

> utf-8 是目前较为常用的包含汉字的编码。其他常见编码还有 unicode，ascii，utf-16 等。

而服务器接收到的消息是经过 `encode('utf-8')` 处理的，于是此时需要 `decode('utf-8')` 进行解码。

### 基于 UDP 协议编写客户端

有服务器进行接收了，就必须得有客户端来发送。  
一下展示了一个简单的具有传输和接收信息功能的服务器。

```python
import socket # 引用网络库
from sys import exit # 从 System 借一个 exit() 函数

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # 声明对象 s

HOST = '127.0.0.1' # 服务器 IP 地址
PORT = 10888 # 服务器端口

s.connect((HOST, PORT)) # 连接到服务器

while True: # 进入消息循环
    data = input("Input Data: ") # 输入待发送的数据
    s.sendto(data.encode('utf-8'), (HOST,PORT)) # 发送到服务器
    (data, addr) = s.recvfrom(1024) # 从服务器接收信息
    print("Recv-Data: ", data.decode('utf-8')) # 输出接收到的信息
```

此处的 `s.connect()` 与服务器中的 `s.bind()` 略有不同，~~至于为什么我正在研究~~  
由此我们可以了解，`sendto()` 函数的两个参数含义：前一个是设置了编码格式的信息内容，后一个是一组元组，包含 HOST 和 PORT ，即需要发送到的主机的 IP 地址（一般用 IPv4 ）和端口号（注意端口号必须是数字类型）。不难看出，`s.sendto()` 是具有方向性的，说清楚发给谁，就只能谁接收到。  
同时可以发现，`s.recvfrom(1024)` 是不具有方向性的，只要是发到我的，不管是谁，我都接收。  
`s.recvfrom()` 是开放的，`s.sendto()` 要是开放的话可能会出事。

## 利用服务器作为中转站

上面给出了一幅基本结构图，是为了简单叙述 socket 库中每个函数的作用。  
下面这张图是为了体现服务器的基本作用：中转交接。  

![](https://img2018.cnblogs.com/blog/1532635/201812/1532635-20181215102600901-282617736.jpg)

从图中我们不难发现无论是你还是其它用户，一旦需要信息交换，就必须经过服务器。  
事实上，你可以选择不经过，但是为了让程序更有条理，我选择利用服务器作为中转站。  

那么问题来了，如果要做到信息交换，就必然涉及到两台及以上的主机。此时，前面我们所写的服务器模型似乎无法完成交换任务，因为它只有接收的功能，那么该如何解决？  

> 以下服务器称为 server，客户端称为 client

我的思路是这样的：  
利用 python 的字典（dict）储存每一个加入到 server 的 client。然后在发送的时候，遍历整个 dict ，把所有储存的主机都发送一遍。  
（讲的什么乱七八糟的，直接上程序）    
至于不知道 Python 字典是什么，请自行学习：[Python 字典(Dictionary) | 菜鸟教程](http://www.runoob.com/python/python-dictionary.html)

```python
# server 2.0 实现信息交换

import socket # 引用网络库
from sys import exit # 从 System 借一个 exit() 函数

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM) # 声明对象 s

HOST = '127.0.0.1' # 服务器 IP 地址
PORT = 10888 # 服务器端口

s.bind((HOST, PORT)) # 启动服务器

user = {} # 设置一个空的 dict 

while True: # 进入消息循环
   (data, addr) = s.recvfrom(1024) # 接收到消息
   if user.get(addr, False) == False: # 利用 get 函数来确定是否是新 client
      user[addr] = data.decode('utf-8') # 如果是的话，把它第一个发来的信息默认为用户名
      print("IP(%s) NickName(%s) Join" % (addr, data.decode('utf-8'))) # 输出用户加入信息
   print(addr," : ", data.decode('utf-8')) # 输出：谁发的，发了什么
   for key, value in user.items(): # 遍历已经储存的 client
      s.sendto(data, key) # 向这些 client 传输某个用户发出的信息。
```

这串代码码风并不是很和谐，但是条理还算清晰。重点：

```python
if user.get(addr, False) == False: 
      user[addr] = data 
      print("IP(%s) NickName(%s) Join" % (addr, data.decode('utf-8'))) 
```

首先 Python 中 dict 的 get 函数用法请自行学习。  
我在这里把 addr ，即用户的 IP 地址作为关键字，而它发送的第一条信息作为值。  
```python
if user.get(addr, False) == False: 
```
这一句话便起到了去重的作用，如果该 addr 关键字已经被包含的话，就不再把它重复纳入 dict 中。但是如果原先没有这个关键字，那么需要把它加入到 dict 中。并且，以用户发的第一条信息作为该关键字的值，即用户名，相当于构成了一个 `{ IP 地址 : 用户名 }` 的 dict。  

客户端的程序基本不变，就是在进入消息循环之前（就是 `while True:` 之前）需要加上一段，提示用户输入用户名，并且作为第一条消息发出。即：
```python
NickName = input("Please input your nick-name : ")
s.sendto(NickName.encode('utf-8'), (HOST, PORT))
```
另外，这里的最后一段代码也可以适当修改一下：
```python
for key, value in user.items():
      s.sendto(data, key)
```
改成：
```python
for key, value in user.items():
         if key != addr:
            s.sendto(data.encode('utf-8'), key)
```
addr 就是这条信息的发出者，既然是他发出的，那他自己的客户端必定显示了他输入的内容，如果再显示一遍就显得累赘了。

## 如何实现多线程

这一段我挣扎了很久，因为线程编程对于一个长期使用低版本 C++ 的孩子来说十分陌生。  

在使用之前我们先得明确，为什么我们要用线程？  

在之前 “利用服务器作为中转站” 中我们编写了一个支持信息交换的程序，但是使用时我们不难发现，这个程序有一个很大的弊端：输发接收不同步，即只能发一条，接收一条，不发就接收不到。  

遇到这个问题的时候我第一时间想到了利用缓冲区解决，以前用 C++ 写 《Tank War》 的时候不会多线程，就是每 5 ms 刷新一次缓冲区，检测是否输入字符，如果输入就执行相应的操作，否则程序按部就班的进行。但是用在这里肯定不现实，5 ms 可能连个 “f__k” 都打不完。

于是只好去简单学习下多线程，我利用的是[这篇教程](https://www.cnblogs.com/yeayee/p/4952022.html) ，简单明了，但是折腾了半天才应用到我的程序上来。  

另外要讲的是，需要多线程的事实上仅有 client ，因为 client 需要同时做两件事：发送和接收，而服务器似乎没有这个负担，它仅需转发。

所以，我们聚焦到客户端来：
```python
import socket
import threading, time # 包含 threading（线程控制）和 time （时间控制）
from sys import exit

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

HOST = '192.168.1.2'
PORT = 10888

s.connect((HOST, PORT))

# 一下定义两个函数 RECV() 和 SEND()，分别进行收、发
def RECV():
   while True:
      (data, addr) = s.recvfrom(1024)
      print("HOST: ", data.decode('utf-8'))
      time.sleep(1)
      
def SEND():
   while True:
      data = input("")
      s.sendto(data.encode('utf-8'), (HOST, PORT))
      time.sleep(1)

t1 = threading.Thread(target=RECV) # 将 RECV() 放入线程 t1
t2 = threading.Thread(target=SEND) # 将 SEND() 放入线程 t2

t1.start() # 启动线程 t1
t2.start() # 启动线程 t2
t1.join()
t2.join()
# join所完成的工作就是线程同步
# 即主线程任务结束之后，进入阻塞状态
# 一直等待其他的子线程执行结束之后，主线程再终止
```

我们把 client 的两个任务：收、发打包成了两个函数，放入两个不同的进程中，再让这两个进程同步运行，就做到了收、发同时进行的目的。  
至于每个函数中出现的 `time.sleep(1)`，是推迟时间，相当于暂停一秒。如果不写上会出现一种很玄学错误，就是收发都变得不完整，可能你发的是 “1234567” ，server 接收到的却是 “12”；server 发来了 “kkksc03” ，结果 client只接收到了 “kkk”。`time.sleep(1)` 的操作就是为了让它有充足的时间完成自己的任务，慢下来，也许能做的更好，生活也豁然开朗。  

至此，你已经完成了一个客户端的基本操作了。

## 让我知道你是谁

注意，如果使用以上 client ，还有一点每种不足，那就是不论谁发来的信息，都是如下显示：
```nohightlight
HOST: 123
HOST: hello!
HOST: ...
```
这样似乎并不是很友好。所以，为了让它成为一个合格的通讯工具，我们还要再进行一点小小的修改：  

首先，找到 client 中的 RECV() 函数，把 `print` 中恼人的 `"HOST"` 删去：
```python
def RECV():
   while True:
      (data, addr) = s.recvfrom(1024)
      print(=data.decode('utf-8'))
      time.sleep(1)
```

然后，找到 server 中的这一段：
```python
for key, value in user.items():
         if key != addr:
            s.sendto(data, key)
```
还记得我们之前建立的 user 字典是以 `{ IP 地址 : 用户名 }` 的格式来储存的吗？  
我们现在手上有 addr ，想要知道其用户名，只要用 `user[addr]` 就可以获取了。  
然后，我们可以把用户名和其发出信息整合，如下：  
```python
data = user[addr] + " : " + data.decode('utf-8')
      for key, value in user.items():
         if key != addr:
            s.sendto(data.encode('utf-8'), key)
```
OK，大功告成！再次运行，你就能看到每个人的用户名了！
```nohightlight
老王: Hello!
小明: Hi!
小红: Have a nice day!
```

## 最后说一句

写了这么多，你难道没发现什么问题吗？  
没错，我们在 server 和 client 的程序开头总有一段：
```python
HOST = '127.0.0.1'
PORT = 10888
```
那么问题来了，不论是针对 server 还是 client，在不同的电脑上打开，IP 地址（也就是其 HOST 的值）都是不同的。所以我们需要进行再一次修改：
```python
HOST = input("Input Server IP address: ")
PORT = 10888
```
把 server 和 client 都改成以上格式，便大功告成了。使用的时候只要查询一下本机（服务器） IP 地址，输入即可。

## 源码
（不要脸的放上了版权信息）
### server 服务器
```python
print("Hosted By markjuruo(Linzhihan)")
print("To view more, please visit")
print("https://github.com/markjuruo/Python-UDP-Chating\n")

import socket
from sys import exit

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

HOST = input("Please input IP address: ")
PORT = 10888

s.bind((HOST, PORT))

user = {}

while True:
   (data, addr) = s.recvfrom(1024)
   if user.get(addr, False) == False:
      user[addr] = data.decode('utf-8')
      print("IP(%s) NickName(%s) Join" % (addr, data.decode('utf-8')))
   else:
      print(addr," : ", data.decode('utf-8'))
      data = user[addr] + " : " + data.decode('utf-8')
      for key, value in user.items():
         if key != addr:
            s.sendto(data.encode('utf-8'), key)
```
### cilent 客户端
```python
print("Hosted By markjuruo(Linzhihan)")
print("To view more, please visit")
print("https://github.com/markjuruo/Python-UDP-Chating\n")

import socket
import threading, time
from sys import exit

s = socket.socket(socket.AF_INET, socket.SOCK_DGRAM)

HOST = input("Please input IP address: ")
PORT = 10888

NickName = input("Please input your nick-name : ")
s.sendto(NickName.encode('utf-8'), (HOST, PORT))

def RECV():
   while True:
      (data, addr) = s.recvfrom(1024)
      print(data.decode('utf-8'))
      time.sleep(1)
      
def SEND():
   while True:
      data = input("")
      s.sendto(data.encode('utf-8'), (HOST, PORT))
      time.sleep(1)

t1 = threading.Thread(target=RECV)
t2 = threading.Thread(target=SEND)

t1.start()
t2.start()
t1.join()
t2.join()
   

```

<br/>
<div class="tip">
个人整理，实属不易。<br/>
本文作者：markjuruo<br/>
转载注明出处：<a>https://www.cnblogs.com/markjuruo/p/10101625.html</a><br/>
备注：若有任何问题，可以联系 markjuruo@163.com ，谢谢！
</div>