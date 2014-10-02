title: 【Erlang】Atom, Tuple和List(2)
date: 2014-03-06
tags: [分布式, Erlang]
categories: Erlang

---

首先，这三个词，Atom，Tuple和List，应该翻译成⎡原子⎦，⎡元组⎦和⎡列表⎦，这里面就开始有些很怪异的东西了。元组和列表在python里面好像也有，但我python不是很熟，所以也不多说了。

-----

##**Atom**##

认真看了第一篇文章的同学肯定会注意到一个细节，就是程序里面的变量第一个字母是大写的，如果第一个字母小写了，就变成了我们现在要讨论的一个概念：元组。

以下是一个厘米和英寸的转换函数：

```
-module(test).
-export([convert/2]).

convert(M, inch) ->
    M / 2.54;
convert(N, centimeter) ->
    N * 2.54.
```
我觉得原子这个东西在Erlang里面就是用于模式匹配的，当然我这样说肯定不严谨，但是开始应该是便于理解的。比如对于上面这个module，我们可以这样调用
	
	test:convert(3, inch).
	test:convert(7, centimeter).
	
第一个函数调用通过inch就会匹配convert的第一个子句，返回3/2.54。  
第二个函数调用通过centimeter会匹配convert的第二个子句，返回7*2.54。

如果输入test:convert(3, aa).就会报错，因为无子句匹配，就运行不了了，这几个例子应该足以让同学们了解这个调用的意思了。

<!--more-->

##**Tuple**##

元组是用大括号括起来的一些玩意儿。例如上面的函数我们可以用更好的形式来表示，如下：

```
convert({centimeter, X}) ->
    {inch, X / 2.54};
convert({inch, Y}) ->
    {centimeter, Y * 2.54}.
```

{centimeter, X}表示X厘米，这在语义上也更加understandable。元组里面是可以嵌套元组的，也就是说{A, {B, C}}也是OK的，元组里面的东西叫元素，A是第一个元素，{B, C}是第二个元素。

##**List**##

列表里面的道道很多，有多开始不好理解的地方。先看以下例子：

	[{moscow, {c, -10}}, {cape_town, {f, 70}}, {stockholm, {c, -4}}, {paris, {f, 28}}, {london, {f, 36}}]
	
这就是一个列表，元素是一个个的tuple，每个tuple第一个元素是一个城市，第二个元素又是一个tuple，第一个元素代表是摄氏还是华氏，第二个就是温度的数值。

列表里面有一个操作符⎡|⎦，这个操作符会分割列表的第一个元素和剩余的元素，例如若[First | Rest] = [A, B, C, D]，则First＝A，Rest＝[B, C, D]，这里有两点需要注意：

1. 这个操作符分割的是**第一个**元素和**剩余的**元素。
2. 被分割后Rest是剩余元素组成的列表，而不是仅仅就是剩余元素而已，不要问我为什么是这样，我也不知道，Erlang就是这样规定的。 

在给一个例子加深理解：[E1, E2 | R] = [1,2,3,4,5,6,7]，这里E1=1，E2=2，R＝[3,4,5,6,7]。

就这个⎡|⎦操作符在Erlang里用的非常多，各种递归都要用这个。比如求一个列表的长度，在Erlang里是这样实现的：

```
-module(test).
-export([list_length/1]).

list_length([]) ->
    0;    
list_length([First | Rest]) ->
    1 + list_length(Rest).
```
运行：test:list([1,2,3,4,5,6,7])，结果是7。

这种编程模式刚开始接触的时候确实不好理解，我也只比刚开始接触是理解得多一点点而已。





