title: Eager Reliable Broadcast
date: 2014-03-02 15:14:08
tags: [分布式, broadcast]
categories: [分布式系统]

---

看到这名字我就蛋疼，eager reliable broadcast，这种名字我能怎么翻译？热心可靠的broadcast？这语言的嘈我还是不吐了。

这个broadcast理论上比lazy reliable broadcast要简单。先看看算法：  
![image](http://ww4.sinaimg.cn/large/737bf613gw1ee1rrwqf5uj20i80b4jtf.jpg)


当某节点的eager reliable broadcast收到要发消息的命令时，首先把此消息存在变量delivered中，然后立即发给自己，然后再broadcast给其它人。  
当节点收到其它节点发来的消息时，先查看自己是否已经接收过此消息，如果没有，则先把此消息存在变量delivered中，然后告诉发此消息的人我收到了，然后再转发此消息给其它人。

这个算法其实就是一顿无脑转发消息确保每个correct的节点都能收到broadcast，然后用遍历delivered来控制不重复接收消息，以此来确保这个reliable，这里每个人都转发，似乎就是对与eager的诠释。