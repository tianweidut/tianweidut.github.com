---
date: 2012-12-07 18:15:00
layout: post
title: Python 语言技巧
categories:
    - Tech
tags: 
    - Python
comments: yes
---

# 列表高级操作

## filter

过滤器，调用一个bool函数来迭代遍历每个seq中的元素，返回一个bool_seq返回值为true的元素序列。

    {% highlight python %}
        b = filter(lambda x:x>5,xrange(0,10))
        #当filter 参数为None，会调用identity()函数，list中
        #为假的元素将被过滤
        b = filter(None,xrange(0,10)) {% endhighlight %}

## map

map的第一个参数作用与给定序列的每一个元素，并用一个list做为返回值。

    {% highlight python %}
        b = map(lambda x:x+10,xrange(1,10)) {% endhighlight %}
  
## reduce

reduce的第一个参数func为一个二元参数；func将作用于seq的每一个元素，
**每次携带一对（previous result 和current value）,连续的将现有结果和下一个值func预算，作用于随后的结果上**，
最后归约成一个值进行返回。

    {% highlight python %}
        b = reduce(lambda x,y:x+y,xrange(1,10))
        #求阶乘
        f = lambda N:reduce(lambda x,y:x*y, xrange(1,N+1))
        print f(5) {% endhighlight %}

## list comprehensions(列表推导式)

举例:
    
    {% highlight python %}
        l = [i for i in range(10) if i % 2 == 0] {% endhighlight %}

优点：
  * 不需要使用计数器
  * 不要重复确定列表的某个元素要进行使用，省去索引过程，更加高效
  * Python对其进行特殊的优化，执行效率非常高
  
## enumerate(简洁索引)

举例：
    
    {% highlight python %}
        # 注意并没有声明 i 为0和对其进行迭代累加
        seq = ["one", "two", "three"]
        for i, element  in enumerate(seq):
            seq = '%d: %s' % (i, element)
    {% endhighlight %}

------

# lambda表达式

匿名函数，lambda中的所有指针全部失效，python解释器将不再创建属于该函数的stack区间
保留临时变量，注意这与函数的压栈截然不同。实际上lambda完全是一个表达式，与C中
macro类似。可以将lambda赋予外部变量或指针，可以接受参数，也可以用于传递返回结果。

一些例子：

    {% highlight python %}
        true = lambda :True
        false = lambda :False
        sqrt = lambda x:x**0.5 {% endhighlight %}

资源参考，[这里](http://www.secnetix.de/olli/Python/lambda_functions.hawk)


## def 与 lambda 区别

  * def 是语句
  * lambda 是表达式

------

# closure 闭包

在GUI和DB事务设计中，闭包被广为使用，用来限制变量的越权读写。
闭包函数可以自由的操作上层函数的变量，最高至global域。反之，上层函数
无法操作闭包内的变量 ，因为闭包是在上层函数创建之后产生的。
当内外冲突时，closure将忽略上层变量保留内部变量。

代码参考,[这里](https://github.com/tianweidut/CookBook/blob/master/python/closuree.py)

------

# 格式化字符串

## %s, %r 区别

  * %s 表示对字符串进行str()操作，转化为str类型，创建了一个新的对象
  * %r 表示repr()操作，是object的字符串形式，并没有进行转化

    {% highlight python %}
        d = datetime.date.today()
        str(d) --> show like '2012-12-07'
        repr(r) --> only for string 'datetime.date(2012,12,7)'
        d = 'test'
        a = 'result:%s'%d --> result:test
        a = 'result:%r'%d --> result:'test' {% endhighlight %} 

# Python Package

## __init__.py:
  * __init__.py 掌握这Python的包机制，控制包的导入行为
  * 当为空时，仅仅导入包，不做额外的操作。
  * 当不为空时，会在该包导入时执行相关内容。
     * __all__ : 将其列表中的子模块和子包导入到当前作用域中。可以突破文件系统限制。如果不定义__all__，无法实现all import。参见[这里](http://docs.python.org/2/tutorial/modules.html#importing-from-a-package)。
     使用时，代码如下
    {% highlight python  %}
        __all__ = ['pack1','module1','module2']
        # import all modules
        from testpack import *
    {% endhighlight %}
    
------

# 常见陷阱

## tuple陷阱

* 良好的写作方式是（item1,item2,） 
* 当单个元素是若写成（item1）就各种错误，所以一定要（item1,）明确的告诉这是元组

------

# Design Pattern in Python

## Signleton

[__new__实现单例](http://www.cnblogs.com/lovemo1314/archive/2011/05/03/2034927.html)

[metaclass实现单例](http://code.activestate.com/recipes/102187-singleton-as-a-metaclass/)

[多种方法](http://my.oschina.net/u/140191/blog/51295)

------

# 异常与with

## with...as 

替代try...finally语句，实现发生错误时候，清理资源，应用在一下场合：
* 关闭一个文件
* 释放一个锁
* 创建一个临时的代码补丁
* 在特殊环境中运行受保护的代码
简单的说就是能在**代码块的前和后调用一些代码**提供了方便。举例：
    
    {% highlight python %}
        with file('/etc/hosts') as hosts:
            for line in hosts:
                print line {% endhighlight %}

当类实现with协议，即实现__exit__和__enter__两个函数时，就可以使用with。
举例：
    
    {% highlight python %}
        Class Context(object):
            def __enter__(self):
                print "enter init"
            def __exit__(self, exception_type, exception_value, 
                        exception_traceback):
                # 当发生错误时，在三个参数会被填充，默认为None。
                # 当with中抛出异常，context不应该再次抛出异常，应该进行处理。 
                print "exit value"
                if exception_type is None:
                    print "None exception"
                else:
                    print "with an error  (%s) " % exception_value
        with Context() as c:
            print "out of the context"
            raise TypeError("here is a bug!")
    {% endhighlight %}

上段代码执行顺序如下：

1. 执行Context()表达式，返回Content Manager子类给c，Content Manager规定__enter__和__exit__。
2. 调用c的__enter__方法
3. 执行with中的内容
4. 如果with中代码抛出异常，执行c的__exit__函数，同时传递了异常的类型，值和调用堆栈。
5. 若with中未抛出异常，执行__exit__函数，不传递参数。

------

# unknow features

## __new__ 与 __init__

* new的优先级更高一些
* __new__ 一般用于继承内置类，返回的是一个对象，可以应用在单例模式中；相当与JAVA里面的构造器
* __init__ 只做初始化工作，没有返回值。
* 如果重写了__new__，而在__new__中没有调用__init__,则__init__将不起作用。
* 参考[这里](http://docs.python.org/reference/datamodel.html#object.__new__)，还有[这里](http://www.cnblogs.com/lovemo1314/archive/2011/05/03/2034927.html)

------
(to be continued...)
