---
date: 2014-04-20 21:00:00
layout: post
title: Python program performance 
categories:
    - Tech
tags: 
    - Python 
    - Performance
---

Python 程序的性能问题饱受诟病，按个人理解，首先要保证算法的复杂度在可以接受的范围，
这个也是影响性能的关键，其次当代码变得越来越长，模块越来越多，快速的得到性能指标数据，
分析瓶颈，并做出针对性的优化变得十分重要。

性能指标:
  - 运行速度：包括程序运行时间，各个函数调用用时，运行速度的瓶颈等
  - 内存：总体占用内存的多少，是否有内存泄露等

--------------------------------------------------------------------

## 运行速度

### Linux Time Command
`time` 命令能够快速的掌握程序运行时间，

    {% highlight python %}
        time python hello.py
        real    0m1.028s
        user    0m0.001s
        sys     0m0.003s
    {% endhighlight %}

输入的含义：

  - real: 实际程序运行的用时，从开始到结束，包括等待其他进程的时间（如等待IO） 
  - user: user mode，非kernel的cpu time spend，可以认为是该进程真正运行时间，排除其他干扰。
  - sys: ,kernel的cpu time spend

其中 `user+sys != real`, 很容易理解，程序执行分配的cpu时间片加上等待时间才是真正程序运行，外界
看到的时间消耗。当`user+sys`比`real`小得多时，应该是系统load比较高或改程序io wait较多。


### Python code 
可以写一个`Timer` Class，作为`context manager`，自己定义`__enter__`和`__exit__`，分别标记开始和结束，
是`with`或`decorator`来进行标记和计算。


### line profiler 
[line profile](https://pythonhosted.org/line_profiler/), 是测试script每行执行情况的工具，使用的时候，
只需要对所需观察的函数添加`@profile`的`decorator`，并用`kernprof.py -l -v xxx.py`运行该程序，然后就会得到
每行的分析数据。

--------------------------------------------------------------------

## 内存使用

### memory profiler
[memory profiler](https://github.com/fabianp/memory_profiler)，能够得到script每行的内存占用情况，使用时
对所需观察的函数添加`@profile`的`decorator`，并用`python -m memory_profiler xxx.py`运行该程序。

### objgraph memory leak
CPython的编译器实现使用引用计数的方式对object的内存进行管理，对象保存但计数为0，就会发送memory leak.
[objgraph](http://mg.pov.lt/objgraph/)，可以得到object的reference关系以及添加或删除的操作等，
使用的时候通过`pdb`来注入`objgraph`。

--------------------------------------------------------------------

## Refer
- [Real, User and Sys process time statistics](http://stackoverflow.com/questions/556405/what-do-real-user-and-sys-mean-in-the-output-of-time1)
- [A guide to analyzing Python performance](http://www.huyng.com/posts/python-performance-analysis/)
