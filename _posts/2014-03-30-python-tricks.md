---
date: 2014-03-30 18:15:00
layout: post
title: Python 学习笔记 
categories:
    - Tech
tags: 
    - Python 
---


1. class
    1. __x 私有变量， 私有变量和公共变量可以同名
    2. def __x(): 私有函数
    3. __x__  就不是私有变量，一般是类的方法
    4. 但是如果是对象实例化之后添加的__属性, 就不是私有变量，可以获取(应为该特性是在编译时发生的)

2. class
    1. cls 和 self 并没有任何实际含义，只是通用命名
    2. self 只是一个占位符
3. '{2}-{0}-{1}'.format(2009, 4, 100)
4. 直接使用对象的bool 值
5. descriptor 数据描述符
    1. 对属性做一些额外的工作
    2. staticmethod 和 classmethod 区别
6. class 继承
    1. 菱形继承  -->  C3 算法
7. python 中任何函数都是有返回值的，及时没有写任何东西，默认会返回None
8. bugs:
    1. if a: --> 这个有时候没法判断到底是None，还是空list，还是Fasle，所以应该明确指定类型
    2. a = []; 如果 if a == []: 这个时可以进入if, 但是 if a is []: 这个就不会， if not a: 这个也可以进入判断
    3. is 与 == 的区别：
      1. is 是比较相同的对象实例
      2. == 完全看 __eq__() 函数的实现

9. isinstance 判断对象的类型
10. 三元表达式技巧
    1. a = b if conf else c
    2. a = b or c
11. 当发生异常是，打印异常堆栈信息：
    1. import traceback
    2. traceback.print_exc()
12. with 语句
    1. with 包裹的任何内容，如果发生异常，自动调用f.close() 来关闭文件
13. os.environ 反应系统环境变量，返回字典
14. SIGTERM 与 SIGKILL
    1. SIGTERM： 友好关闭新号，进程可以捕捉这个新号，根据需要来关闭进程，在关闭前可以执行默写清楚程序，包括完成正在执行的进程，关闭打开的文件等；同时进程是可以忽略这个新号的
    2. SIGKILL：进程不能忽略这个新号
15. '\r\n' 与 '\n': http://stackoverflow.com/questions/1761051/difference-between-n-and-r
16. 命名空间问题
    1. Python使用命名空间来实现对变量轨迹
    2. 名字空间实际上是一个dict类型
    3. 在一个 Python 程序中的任何一个地方，都存在几个可用的名字空间。每个函数都有着自已的名字空间，叫做局部名字空间，它记录了函数的变量，包括函数的参数和局部定义的变量。每个模块拥有它自已的名字空间，叫做全局名字空间，它记录了模块的变量，包括函数、类、其它导入的模块、模块级的变量和常量。还有就是内置名字空间，任何模块均可访问它，它存放着内置的函数和异常。
    4. 查找顺序
      1. 局部名字空间――特指当前函数或类的方法。如果函数定义了一个局部变量 x，或一个参数 x，Python 将使用它，然后停止搜索。
      2. 全局名字空间――特指当前的模块。如果模块定义了一个名为 x 的变量，函数或类，Python 将使用它然后停止搜索。
      3. 内置名字空间――对每个模块都是全局的。作为最后的尝试，Python 将假设 x 是内置函数或变量。
    5. 如果 Python 在这些名字空间找不到 x，它将放弃查找并引发一个 NameError 异常，同时传递 There is no variable named 'x'
    6. 在运行时可以对名字空间进行访问，局部locals(), 全局globals(),返回的都是字典
    7. locals（）返回的实际上是局部变量字典的一个拷贝，也就是说如果locals()['x'] = 1 这是对locals()局部变量字典没有影响的，但globals()则可以改变
17. built-in 自省函数
    1. getattr 返回属性值, getattr(Instance, "attr", "default") 对对象Instance获取attr属性，如果有就返回该属性的值，如果没有就返回default. 可以用getattr实现工厂模式
    2. 使用技巧: a = ["a", "b"]; a.pop, a.pop() # 一个返回built-in对象，一个返回弹出的值；getattr(a,"pop")() 等价于弹出一个值； getattr(a, "append")("c") 添加一个值到末尾
18. __import__ 动态导入模块
    1. http://blog.donews.com/limodou/archive/2005/06/10/422024.aspx
19. print "aa|bb|cc|dd|ee|ff".split('|',1) 结果是['aa', '。。。'] 最多分割几次
20. getpass 可以在终端或tty中动态的获取输入密码，这个时subprocess.call做不到的
21. round(n, 2) 四舍五入保留两位小数
22. callable() 检测该对象是否可以调用，也就是说类实现了__call__， 但是调用可能会失败
23. import this
    1. beautiful is better than ugly
      1. 不要过多的嵌套使用map, filter
    2. explicit is better than implicit
      1. import 某个包的时候，一定禁止使用from xxx import *, 推荐的做法是能有多少个就写多少个
      2. 如: from corelib.consts import (K_SAYING, K_NOTE, K_PHOTO, K_TOPIC)
    3. simple is better than complex
      1. 尽可能简单的表达意图
      2. for i in resered(seq): foo(i);
      3. for i in seq[::-1]: foo(i)
    4. zip 对两个集合同时迭代
      1. for i,j in zip(seq1, seq2): print  i,j
    5. 多用列表推倒式，能使表达更加简洁
    6. flat is better than nested
      1. 用小函数进行分解，避免过多的if 嵌套关系
    7. sparse is better than dense
      1. 使用适当的空格、空行来使表达更加清晰
    8. 一些习惯
      1. if x != []: pass (bad); if not x: pass; (good)
      2. for i in range(len(seq)): print i, seq[i] (bad); for i, item in enumerate(seq): print i, item (good)
    9. Error should never pass silent!
      1. 对于异常，一定不能简单的pass；要么写logger，要不就raise
    10. 如果一个implementation很容易解释，那么这是一件好事
    11. KISS: keep it simple and stupid
    12.  多使用decorator, 这样能够简化表达，提取公共部分
    13. 对于一些使用后就废弃的变量和函数名，可以用_ 来表述
24. iterators
    1. 可以将序列转化成iter，调用next方法就调用输入一次，如果没有可以输出的了就会抛出StopIteration
    2. 但是在使用中没必要总是监控这个异常，可以像循环使用
    3. 一个iterator描述一个数据流，支持的next方法表达下一个读取的元素是什么
    4. 生成器是创建迭代器的简单方法，生成器运行到yield会被简单的挂起，程序运行状态暂停，下次调用next时恢复执行
    5. all 和 any 是对迭代器快速判断的一个方法
25. Class
    1. __new__ 与 __init__ : 对象创建的时候一定会调用__new__,无论__init__是否被调用
    2. __new__ 调用发生在 __init__ 调用前， __new__ 用来检查对象的初始化情况，或进行一些更底层的初始化工作
26.  简单地讲，yield 的作用就是把一个函数变成一个 generator，带有 yield 的函数不再是一个普通函数，Python 解释器会将其视为一个 generator，调用 fab(5) 不会执行 fab 函数，而是返回一个 iterable 对象！在 for 循环执行时，每次循环都会执行 fab 函数内部的代码，执行到 yield b 时，fab 函数就返回一个迭代值，下次迭代时，代码从 yield b 的下一条语句继续执行，而函数的本地变量看起来和上次中断执行前是完全一样的，于是函数继续执行，直到再次遇到 yield。
27. Popen 可以利用stdin, stdout使用PIPE的方式来控制输出和输入，实现对一个cmd应用的控制
28. 本地配置和线上配置，通过在config.py 底部写入try: from local_config import * except: pass 来覆盖config实现本地的切换
29. urlparse 可以用来解析网址
30. glob 用来进行文件查找，可以使用正则表达式
31. Python Generator
    1. iterating object , python 中可迭代类型，也就是可以for的object，包括list, tuple, set, dict, string, File
    2. Consume iterable object
      1. reduction:  sum(s), min(s), max(s)
      2. constructor: list(s), tuple(s), set(s), dict(s)
      3. in operator
    3. Iteration Protocol
      1. iter() --> 就让对象遵循iteration protocol 协议，能进行iteration操作
      2. next() 获取当前迭代对象，当迭代完成时，raise StopIteration 异常
      3. 让一个class符合iteration协议，需要实现__iter__() 和 next()方法，当进行for迭代时，不会抛出StopIteration异常，只有用next()方法结束迭代才会抛出StopIteration异常
    4. generator 产生a sequence of results 代替单一值，关键是使用yield
      1. generator 对象不会马上执行，只有调用next才开始执行，next的调用可以直接调用该函数，也可以用for的方式
      2. 调用该函数时，会在yield产生数值，然后函数suspend，保存状态，停止运行，直到下次再调用
      3. A generator is a one-time operation. You can iterate over the generated data once, but if you want to do it again, you have to call the generator function again.
      4. list comprehension 是 generator, 使用(i for i in range(10))(generator), 不是[i for i in range(10)](list)
      5. generator 一旦被消费就不能再使用
      6. 标准语法： (expression for i in s if cond1 for j in t if cond2 … if condfinal) --> for i in s: if cond1: for j in t: if condo:… if condfinal: yield expression
      7. generator 是可以连接起来，类似于pipeline的方式
      8. 优点：避免产生大的临时文件或list，尤其是处理大文件时
      9. 原理：base on the concept of pipelining data between different components
      10. os.walk 是一个generator
      11. 典型使用：
        1. Generate a sequence of TCP connections
        2. Generate IO events on a set of sockets
32. python uuid http://www.douban.com/note/69073375/
33. cPickle http://docs.python.org/2/library/pickle.html#module-cPickle
34. os.rmdirs 只能删除目录中为空的，如果要彻底阐述使用shutil.rmtree(path)
35. nose test python 的测试框架 http://nose.readthedocs.org/en/latest/
36. os.path.join 是不能路径进行实际转化的，如os.path.join(“../../“) -》显示的就是../../， 显示实际的应该使用os.path.abspath(‘../../‘)
37. try .. except … finally 结构中无论中间try中间发生什么，包括return，finally 都将会执行， 有时会造成潜在bug
38. a=[] a[0] 会产生错误；但可以用a[:1] 来进行，如果为空则返回[]
39. {} 形成字典 比 dict() 形成字典要更快速
40. python 的序列解包 unpacking
    1. a,(b,c),d=[1,(2,3),4]
    2. a, b = b, a swap
41. zip
    1. a = [1,2,3]  b=[a,b,c]  c = zip(a,b) —> c :[(1,a), (2,b), (3,c)]; —> zip(*c) : [(1,2,3), (a,b,c)]
42. flat list
    1. a=[[1,2],[3,4],[5,6]] —> [j for i in a for j in i]
    2. list(itertools.chain.from_iterable(a))
43. set
    1. a = {1,2,3}  === a = set([1,2,3])
44. defaultdict collections.defaultdict
    1. collections.defaultdict(int), 即使字典中没有响应的元素，也可以获取值，默认为0 ，如 d[‘a'] : 0
    2. collections.defaultdict(str),
    3. collections.defaultdict(lambda : ‘[tttt]’)
    4. https://gist.github.com/hrldcpr/2012250
    5. tree = lambda: collections.defaultdict(tree); root = tree()  python的树结构
45. 集合相乘
    1. forpinitertools.product([1,2,3],[4,5])
    2. forpinitertools.product(*([[0,1]]*4)):  0000 —> 1111
46. python trick http://sahandsaba.com/thirty-python-language-features-and-tricks-you-may-not-know.html
47. 使用列表推导式的一个问题是，会将所有的数据都载入内存，为了避免这种情况，使用generator 进行改造，(x**2 for x in a_list if x > 0) ,应该经常在代码中使用生成器表达式
48. 装饰器是一个包装了另一个函数的特殊函数：主函数被调用，并且其返回值将会被传给装饰器，接下来装饰器将返回一个包装了主函数的替代函数，程序的其他部分看到的将是这个包装函数.  @decorator 意味着分开执行了两部函数，如下：
    ```
    @timethis
    def run():
        pass
    等价于 run = timethis(run)
    ```
49. 上下文管理: contextlib 上下文管理器和with声明相关的工具
    1. 上下文管理器，需要定义一个类具有`__enter__`和`__exit__`方法。上下文管理器会被with所激活，对with块的入口点和出口点进行相应得处理。`__enter__`进入的时候会返回一个在上下文使用的对象。`__exit__`常做资源清理使用，如`with open('f.txt') as f: pass`
    2. 同时存在`@contextmanager`,可以将一个类快速变成`context manager object`,如下：
        ```python
        @contextmanager
        def demo(label):
            start = time.time()
            try:
                yield
            finally:
                end = time.time()
                print 'Finish the task, use %s' % (end-start)
        # 也就是将yield之前的作为__enter__, yield之后的作为__exit__。
        ```

50. 描述器决定了对象属性是如何被访问的。描述器的作用是定制当你想引用一个属性时所发生的操作.`__get__`, `__set__`, `__del__` 三个操作
51. MetaClass: 提供一个改变Python类行为的有效方法。MetaClass定义的是一个类的类。`type`就是一个元类，而且是python中最常见的元类，因为它使python中所有类的默认元类。
52： http://coolshell.cn/articles/11265.html ，注意其中fib的cache，cache只会初始化一次，每个fib返回的都是wrapped，所以不用global变量