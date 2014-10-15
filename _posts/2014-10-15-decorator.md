---
layout: post
title:  python装饰器与ANSI C 的宏
descripthon: "python装饰器是一个很强大的东西，既可以用它来实现装饰器模式，也可以当作宏来用"
modified: 2014-10-15
tags: [python]
---


###前言
&emsp;&emsp;在读python代码的时候，经常会遇到带有**@**修饰符号的关键字,比如__@classmethod__
, __@staticmethod__ 这两个关键字修饰类里的函数，表示为类方法和静态方法，可以无需实例化，
直接调用。这个就是python的装饰器，python装饰器是一个很强大的功能，Tornado里的coroutine,
Flask的route都用到它。所以我想对平时在编程中遇到的类似机制进行总结。


###装饰器模式
&emsp;&emsp;以前看过[Headfirst设计模式](http://book.douban.com/subject/2243615/ "设计模式")，
讲到装饰模式能够实现动态的为对象添加功能，是从一个对象外部来给对象添加功能。
通常给对象添加功能，要么直接修改对象添加相应的功能，要么派生对应的子类来扩展，
抑或是使用对象组合的方式。由于直接修改对应的类这种方式并不可取。
在面向对象的设计中，而我们也应该尽量使用对象组合，
而不是对象继承来扩展和复用功能。装饰器模式就是基于对象组合的方式，
可以很灵活的给对象添加所需要的功能。__装饰器模式的本质就是动态组合
。动态是手段，组合才是目的__。总之，装饰模式是通过把复杂的功能简单化，
分散化，然后再运行期间，根据需要来动态组合的这样一个模式。书里面举了一个例子，就是对象村的咖啡店，
比如，我有一杯苦咖啡，当我需要加糖的时候，直接使用糖装饰器添加一勺即可。当需要加牛奶的时候，使用牛奶
装饰器即可，无需重新换杯。这就是面向对象的基本原则之一：__多用组合，少用继承__。

###ANSI C的宏
&emsp;&emsp;为什么会想到宏，因为c语言的宏替换可以将一个表达式替换成另外一个表达式，在替换的时候便可以
对表达式进行装饰。
下面看代码：
{% highlight c %}
    #include<stdio.h>
    #define Macro1(x) (printf("This is first macro:%d\n",x))
    #define Macro2(x) (printf("This is second macro:%d\n",x))
    
    int main(){
        int x = Macro1(3);
        int y = Macro2(4);
        return 0;
    }
{% endhighlight %}
这里结果的输出是

![result](http://ralefile.qiniudn.com/c_macro.png)

这里__Macro1__修饰了数字3,  __Macro2__修饰了数字4,  功能是修饰中加入了打印语句，但宏不能返回值，只能进行替换，
但也很强大，[assert.h]("http://zh.wikipedia.org/wiki/Assert.h")标准库就是用
宏实现的。专门用来帮忙检查bug。初学C的时候往往会忽略他的强大之处。

###python装饰器

python装饰器他也可以用来实现装饰器模式，但比装饰器模式灵活，他也可以当作宏来运用，下面看代码：
{% highlight python %}
    def decoration1(func):
        def wrapper(func):
            print "this is decoration1"
            return func
        return wrapper(func)

    def decoration2(func):
        def wrapper(func):
            print "this is decoration2"
            return func
        return wrapper(func)
    
    @decoration1
    @decoration2
    def test():
        print "you're good guy."
    
    if __name__ == "__main__":
        test()
{% endhighlight %}
程序运行截图如下：

![result](http://ralefile.qiniudn.com/python_decorator.png)

它看起来效果和宏一样，这里的 __decoration1__ 和 __decoration2__ 函数都是在不改变 __test__ 函数内部结构的情况下
修饰 __test__ 函数。

###总结
python装饰器是一个很强大的东西，既可以用它来实现装饰器模式，也可以当作宏来用。下面附一段缓存装饰器的
代码：
{% highlight python %}
    import time

    def cache(fn):
        cache = {}
        miss = object()
        def wrapper(*argc):
            # print "Use a cache decorators:"
            result = cache.get(argc,miss)
            if result is miss:
                # print "Not hitting"
                result = fn(*argc)
                cache[argc] = result
            return result
        return wrapper
    
    @cache
    def fab(n):
        if n < 2:
            return n
        return fab(n-1)+fab(n-2)
    
    print "clock1:%s" % time.clock()
    value=fab(35)
    print value
    print "clock2:%s" % time.clock()
{% endhighlight %}    

执行时间如下图：

![result](http://ralefile.qiniudn.com/python_decorator_mem.png)

这样一个计算Fibonacci序列的递归在不用修饰器的cache修饰器的情况下，
linux下我是在3分钟左右，windows会更长。

    












