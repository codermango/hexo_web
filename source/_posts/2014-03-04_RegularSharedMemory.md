title: Regular Shared Memory
date: 2014-03-04
tags: [分布式, shared memory]
categories: [分布式系统]

---

Shared memory在很多领域都有，如果我们不考虑分布式系统，单纯从普通软件开发来考虑，一个类的成员变量实际上就可以看成是该类所有成员函数的shared memory。在分布式系统中，由于是基于事件的（event-based），所以要用消息传递的模式来实现shard memory。

在我所学的教程里，用register来表示一个memory，register是一个对象，节点能够读取和写入数据到register。为了之后的讲解比较容易，这里做两个假设：  

1. 节点是连续的，也就是说一次只做一个操作，不是读取就是写入。  
2. 读取和写入的值是正的，而且从零开始。 

这里有个概念，⎡execution⎦，我们就把它叫做一次执行吧，在一次执行中，一次操作（operation）中invocation和response都出现了，那么称这次operation是complete，如果只有invocation，没有response，那么是fail的。

---

考虑第一种register，英文叫做regular register，原谅我实在不知道这玩意儿翻译成中文叫什么，如果非要找个中文翻译，我觉得只能是叫⎡常规⎦register。这种register有一个writer，多个reader，记作(1, N)。

在regular register中，read操作会返回的值有两种情况：

1. 写操作和读操作没有并行发生，会返回最后写入的值，英文中叫**last value writen**。
2. 写操作和读操作concurrent，也就是并行发生时，会返回正在写入的值或最后写入的值，对于这个的理解我们需要借助下图：  

![image](http://ww4.sinaimg.cn/large/737bf613gw1ee38w19r07j20fp05it8v.jpg)

这个图是一个regular register，但是我刚开始对于p2的第二个read操作不是很理解，后来反复看教材，终于明白是怎么回事了。<font color="red">**这我们要记住，一个读操作或写操作，包含invocation和response，途中每一个w或r的开头是invocation，最后是response，也就是说在response没有到来之前，都不能算作操作完成，所以p2第二个read的时候，属于以上所说的第二种情况，会返回正在写入的值或最后写入的值，在那个时候，正在写入的值是5，最后写入的值是0。**</font>

**以上对红色部分的理解对后面的学习至关重要！！！**