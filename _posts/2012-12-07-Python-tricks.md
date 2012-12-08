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
