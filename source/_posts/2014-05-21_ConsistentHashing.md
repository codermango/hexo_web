title: 一致性哈希(Consistent Hashing)
date: 2014-05-21 13:01:19
tags: [分布式, 一致性哈希]
categories: [分布式系统]

---

在分布式系统里面，一致性哈希是一个非常重要且随处可见的概念。

###**产生缘由**###

我们首先来考虑以下这种场景，如果你懂Chord，肯定会很容易理解。

假设我们有N台Server，每台Server有个id，现在有一个资源R，需要存储到Server上去，如何做？如果你懂Chord，那你肯定知道一个通用的方法是利用Hash，比如hash(R)%N后能得到一个计算后的结果，比如rid，然后就可以映射到Server上，这样一来大量资源就可以通过一个哈希计算自动分配到不同的Server上了。

但是在分布式系统中，Server挂掉是一件很常见也必须考虑的事情，那么如果我们的N台Server里面如果挂掉一台，我们该怎么办？为了保证整个系统正常的运行，我们当然首先要把这台Server剔除掉，这时候Server的数量就是N-1，以上映射资源的公式就变成了hash(R)%(N-1)。同理，如果我们需要加入一台Server，公式就变成了hash(R)%(N+1)。

这时问题就来了，每一次的剔除和加入Server，所有的资源都要重新分配，这对于大规模的系统来说是一件非常expensive的事情。为了解决这个问题，我们就要用上一致性哈希(consistent hashing)了。

<!--more-->

###**基本原理**###

consistent hashing的基本原则是：在移除或添加一个 Server 时，它能够尽可能小的改变已存在的映射关系。

hash算法一般都是把value映射到一个32位的key值，这个数值空间可以想象成一个0到2^32-1的首尾相连的环。如下图：
![image](http://ww4.sinaimg.cn/large/737bf613gw1egm3ru7hbij20bb0a9jrf.jpg)


很萌有木有！！

接下来把4个资源R1，R2，R3，R4映射到hash空间。

	hash(R1) = key1
	hash(R2) = key2
	hash(R3) = key3
	hash(R4) = key4

![image](http://ww1.sinaimg.cn/large/737bf613gw1egm3tp4gxaj209h09aaa4.jpg)

然后把3个Server(SA, SB, SC)映射到hash空间，和上面使用一样的hash函数，这个是consistent hashing的基本思想。
	
	hash(SA) = keyA
	hash(SB) = keyB
	hash(SC) = keyC

在这个环形空间中，沿着顺时针方向从资源的key值出发，直到遇见第一个Server，就将该资源存储在该Server上。在此例子中，映射如下图所示：

![image](http://ww1.sinaimg.cn/large/737bf613gw1egm3vlfhouj209l096weq.jpg)
 

按照这个原理，如果我们此时需要剔除一个Server，比如SB，那么变化的仅仅是R4存储到SC，其它的所有分配模式都不用改变，这就比避免了大量重新的hash计算和资源分配。增加一个Server也是同样的道理。

最后引用别人总结的一致性哈希的作用：

>在一致性哈希算法中，如果一台 Server 不可用，则受影响的数据仅仅是此 Server 到其环空间中前一台 Server（即顺着逆时针方向行走遇到的第一台服务器）之间数据，其它不会受到影响。
在一致性哈希算法中，如果增加一台 Server ，则受影响的数据仅仅是新 Server 到其环空间中前一台Server （即顺着逆时针方向行走遇到的第一台服务器）之间数据，其它不会受到影响。
综上所述，一致性哈希算法对于节点的增减都只需重定位环空间中的一小部分数据，具有较好的容错性和可扩展性。

下一篇文章讨论一致性哈希另一个指标：平衡性(Balance)。
