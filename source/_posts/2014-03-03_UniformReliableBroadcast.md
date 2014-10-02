title: Uniform Reliable Broadcast
date: 2014-03-03 16:58:19
tags: [分布式, broadcast]
categories: [分布式系统]

---

这个uniform应该是一致性的意思，这个一致性应该是一个非常重要的概念，不管是在分布式系统里面还是在网络中，比如很简单的一个例子，读取数据库，如果一个用户正在修改数据库中的某数据，而另一个人正要读取这个数据，如何保持数据的一致性就是我们应该考虑的事情了。

在分布式系统里，具体到broadcast，我们之前的broadcast都只保证节点correct的情况，但是节点fail的情况没有考虑。考虑下面这个情况：  
![image](http://ww2.sinaimg.cn/large/737bf613gw1ee1ura4zmpj20bu07lmxa.jpg)

当p发了消息之后，p和q都接到了，但是在他们把消息转发给r和s之前，就挂了，那么r和s就接收不到消息，所以，uniform reliable broadcast就是要保证：当消息m被某节点接收到，不管这个节点是correct还是挂了，m最后还是要被每一个correct节点接收到。

这就是uniform agreement要干的事。

实现这个的基本方法是，一种叫All-Ack Uniform Reliable Broadcast，需要借助perfect failure detector；另一种叫Majority-Ack Uniform Reliable Broadcast，只需要用eventually failure detector。

第一种方法是：如果p1 broadcast了一个消息m，那么每一个接收到此消息的节点都要返回一个通知（ack），当p1接收到所有correct节点发回的ack时，才能算这个消息broadcast成功。  
第二种方法和第一种方法一样，只是大多数节点接收到ack就可以了，这里“大多数”的意思是Majority=n/2+1, i.e. 6 of 11, 7 of 12....

All-Ack Uniform Reliable Broadcast是Fail-stop模型，Majority-Ack Uniform Reliable Broadcast是Fail-silent模型，这个怎么翻译我也不知道，这玩意儿只能意会了。

