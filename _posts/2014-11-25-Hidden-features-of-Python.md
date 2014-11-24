---
layout: post
title: Python中隐藏特点
description: "python中有许多我们平时很少用到但是很Pythonic的特点，该文章是stackoverflow上的一个问题，个人觉得很不错，就翻译了过来。"
modified: 2014-11-25
tags: [python]
---

###前言
&emsp;&emsp;该篇文章是来自于stackoverflow上的一个问题，[原文地址](http://stackoverflow.com/questions/101268/hidden-features-of-python),个人觉得很不错，就将它翻译了过来。可能有些单词翻译的不太准确，欢迎矫正。

####运算符的链式比较

{% highlight python %}
    >>> x = 5
    >>> 1 < x < 10
    True
    >>> 10 < x < 20 
    False
    >>> x < 10 < x*10 < 100
    True
    >>> 10 > x <= 9
    True
    >>> 5 == x > 4
    True
{% endhiglight %}

&emsp;&emsp;在这个例子中，你可能认为首先运算__1<x__，得到结果为__True__,接着运算__True < 10__,得到结果为__True__.但是，事实并非如此，看最后一个表达式计算，你就知道并非这样，比如第一个表达式，他其实是转化成了__1<x and x < 10__.第二个表达式转化成了__x<10 and 10<x*10 and x*10<100__,为了更少的打印，每个表达式只计算了一次(告诉你，自己玩吧)。评论里有提到，lisp也是这种类似的机制。


