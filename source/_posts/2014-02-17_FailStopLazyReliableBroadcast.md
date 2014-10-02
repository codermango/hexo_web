title: Broadcast
date: 2014-02-17 18:20:43
tags: [分布式, broadcast]
categories: [分布式系统]

---
分布式的学习真是任重而道远，很多概念没有理解，更别谈记住了，所以我需要做一些笔记了。

我这里说的broadcast的概念是分布式系统中的，和其它领域的相不相同我不知道。

我关注的broadcast总体分三种：  
1. Best-effort Broadcast  
2. Reliable Broadcast  
3. Uniform Reliable Broadcast  
4. Causal Reliable Broadcast  

这些broadcast都有三个相同的property：  
1. Validity: If pi and pj are correct, then any broadcast by pi is eventually delivered by pj.  
2. No duplication: no messages delivered more than once.  
3. No creation: no messages delivered unless broadcast.

Reliable Broadcast增加一个Agreement: if a correct node delivers m, then every correct node delivers m.

Uniform Reliable Broadcast增加一个Uniform Agreement: for any message m, if a process delivers m, then every correct process delivers m.
<!--more-->
Best-effort broadcast是最基本的broadcast，它只考虑节点正确的情况，这里需要说明的是，**节点正确的意思是在一次执行中没有crash**，有时候英文资料费劲地方就在这里，所以在这里提出重点强调。

但是网络传输中存在各种各样的问题，首先能想到的就是节点crash，所以这就衍生出了reliable broadcast，这里首先要说的是Fail-Stop 
Lazy Reliable Broadcast。

关于这个名字也是不好理解的地方，我查过中文资料，但是关于这个的好像很少，基本查不到，按照我的理解，fail-stop的意思应该是当遇到fail，这个fail的节点就当它挂了，不在管了，所以也就是这个算法比较lazy，我只能这样理解了，由此也可以看出我们中国学生在语言上的障碍对学习的阻碍有多大了，这个也根本不是单词懂不懂的问题，实在是语言的思维方式问题。

Fail-stop lazy reliable broadcast的思路很简单，看以下两个图：  
![image](http://ww4.sinaimg.cn/large/737bf613gw1ee1qn4izmuj20lg093q3h.jpg)  
这个图是第一种情况，当p2先接收到了p1发的消息，然后发现p1 crash，则把从p1收到的消息再broadcast出去。

![image](http://ww3.sinaimg.cn/large/737bf613gw1ee1qm89qgjj20m008m0ta.jpg) 
这个图是第二种情况，当p2在收到p1 broadcast之前就发现了p1 crash，则依然把从p1接受到的消息broadcast出去。 

具体算法会考虑到重复性之类的问题，这里不祥述了，主要是没时间详述。




