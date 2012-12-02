---
layout: post
title: Google Protobuf 使用笔记
categories:
- Tech
tags:
- Google
- Open Source
- Networks Tools
comments: yes
---

从做[Google Summer of Code 2012](http://www.google-melange.com/gsoc/homepage/google/gsoc2012)项目时候开始接触Google Protobuf, 当时的项目需求是实现Deskop,Android,IOS mobile client 与服务器Django实现通讯,发送一些验证消息，获取探测任务报文，返回Report一类需求，详见[OpenMonitor](http://www.openmonitor.org/). 实际使用发现这东西真好用，特别适合团队内不同平台协作通讯,
实现对象序列化非常轻松，这次做[新东西](https://github.com/chemistryTools)，就想接着用这个实现C# client 与Django的通讯，但经过两天的努力，测试了多个Protobuf 的C#实现，彻底放弃了这一想法，原因是C#性能太差，bug非常多，我在Stackoverflow问了作者，[在这里](http://stackoverflow.com/questions/13469452/overflowexception-and-endofstreamexception-in-protobuf-net)结果答非所问，所以真正的项目不要用Protobuf的C#实现，耽误时间。

下面是使用中，一些心得和使用技巧，内容很少，但足够了，看[官方文档](https://code.google.com/p/protobuf/)能解决所有的疑问，局限于Python,JAVA,C++这三种语言。

1. 处理repeated 方法: repeated 实际就是数组，[这个](http://honghao460886.iteye.com/blog/1537553)写的很好.

2. 编译方法，python用

    protoc messages.proto –python_out=.

为了方便，我写了一个脚本来处理，可以参考[Github上的代码及例子](https://github.com/chemistryTools/ChemCommon).

3. TimeStamp 通常用int64类型。

4. 字段解析：
    - required 初始值必须提供，否则字段是未初始化的，导致序列化失败
    - optional 可选字段，系统会提供默认值
    - repeated 是数组字段，当然可以出现0次

5. C# Port
经过一天的实验证明，[protobuf-csharp-port](https://code.google.com/p/protobuf-csharp-port/)这个第三方库，只实现了最基本的Protobuf，当repeated字段增加，单个字段长度增长，这个第三方库会抛出各种异常，很忧伤…. 同时,[Protobuf-net](code.google.com/p/protobuf-net)这个库虽然接口写的很好，作者在社区也很活跃，但同样的问题。
    - 要不修改他的源代码
    - 要不寻找其他的三方库
    - 最后的办法是，改回了JSON，Python端明显不如Protobuf好.


