---
layout: post
title: Python中的隐藏特点
description: "python中有许多我们平时很少用到但是很Pythonic的特点，该文章是stackoverflow上的一个问题，个人觉得很不错，就翻译了过来。"
modified: 2014-11-25
tags: [python]
---

###前言
&emsp;&emsp;该篇文章是来自于stackoverflow上的一个问题，[原文地址](http://stackoverflow.com/questions/101268/hidden-features-of-python),个人觉得很不错，就将它翻译了过来。可能有些单词翻译的不太准确，欢迎矫正。

####运算符的链式比较

{% highlight python %}
   
    Example:
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

{% endhighlight %}

&emsp;&emsp;在这个例子中，你可能认为首先运算 __1<x__，得到结果为 __True__ ,接着运算 __True < 10__ ,得到结果为 __True__ .但是，事实并非如此，看最后一个表达式计算，你就知道并非这样，比如第一个表达式，他其实是转化成了 __1<x and x < 10__ .第二个表达式转化成了 __x<10 and 10<x*10 and x*10<100__ ,为了更少的打印，每个表达式只计算了一次(告诉你，自己玩吧)。评论里有提到，lisp也是这种类似的机制。


####函数参数解包

&emsp;&emsp;使用`*`和`**`作  为函数参数时，你可以传入list和dict类型数据，意思就是当你使用 `*` 前缀时可以代表list，tuple等序列数据，而 `**` 前缀可以表示字典类型数据。
例子:
{% highlight python %}
    def draw_point(x, y):
        # do some magic

        point_foo = (3, 4)
        point_bar = {'y': 3, 'x': 2}

        draw_point(*point_foo)
        draw_point(**point_bar)


{% endhighlight %}

字典、列表被广泛用于容器后，非常方便。


####就地交换
{% highlight python %}
    for example:
    >>> a = 10
    >>> b = 5
    >>> a, b
    (10, 5)

    >>> a, b = b, a
    >>> a, b
    (5, 10)

{% endhighlight %}
这个指令(`a,b = b,a`)右边的表达式创建了一个新元组，左边的立马解包到变量a,b。在这条指令过后，a,b的值已经被交换，而 `a,b`这个新元祖也未被GC标记和引用，所以未被回收。


####for...else语句

&emsp;&emsp;

{% highlight python %}
    for i in foo:
        if i == 0:
            break
    else:
        print('i was never zero.)

{% endhighlight %}
这个程序在 `break` 没有被调用的时候会执行，这段代码还可以使用下面这段通俗代码来理解：
{% highlight python %}
    found = False
    for i in foo:
        if i == 0:
            found = True
            break
    if not found:
        print('i was never zero.')

{% endhighlight %}
不过这个特点还是少用比较好，放在for循环内部就可以了，放在外部有可能和其他的if语句引起bug.

####指定名字的格式化

`%`可以像字典一样对参数进行格式化：
{% highlight python %}
    for example:
    >>> print "The %(foo)s is %(bar)i." % {'foo': 'answer', 'bar':42}
    The answer is 42.

    >>> foo, bar = 'question', 123

    >>> print "The %(foo)s is %(bar)i." % locals()
    The question is 123.

{% endhighlight %}
`locals()`也是字典，你可以使用`locals()` 和 `%` 替换本地变量。

新风格的格式化：
{% highlight python %}
    >>> print("The {foo} is {bar}".format(foo='answer', bar=42))
{% endhighlight %}
推荐新风格，新风格跟有`pythonic`的感觉。

####使用.pth文件添加路径

&emsp;&emsp;添加python包路径的最好办法是使用.pth文件进行添加路径，每一行只能包含一条路径，这些路径会添加到`sys.path`中，而且安装的目录不会覆盖原来的标准模块，这个机制意味着你不能安装标准模块的固定版本(因为都会存在).


------


&emsp;&emsp;未完待续。


