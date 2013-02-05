---
date: 2013-01-16 18:15:00
layout: post
title: Git && Github 使用技巧
categories:
    - Tech
tags: 
    - Git
comments: yes
---

## Git && Github 是什么

 * Git: 非常好用的源代码控制软件，Linux Kernel这样的超大型开源软件的源代码管理工具，MS的TFS跟这个比不是一个量级的...
 * Github: 最好的源代码交流社区，最大的代码托管社区，代表了一种文化，让代码交流变得非常酷。Github不止能托管代码，还能写Blog，发代码状态，还有找工作哎。
 我一直认为招聘靠谱的程序员只需要看看他在Github上的代码就行，胜过各种XX面试，呵呵。

 ![Github Contributor](http://tianwei-wordpress.stor.sinaapp.com/uploads/2013/02/blog-contributor.png)

 上面这个图是我的账户过去一段时间编码情况，其实可以用它来监督自己是不是每天都编码。

## Git 快速使用

### 最简单的使用
 * 初始化Git本地数据库   
   **git init**
 
 * 增加修改文件  
   **git add .**
     * . 增加当前目录下所有子文件和子目录
     * 可以在具体文件夹下增加，不一定在根目录执行这个命令
 
 * 本地提交  
   **git commit -m "some code comments"**
     * 上面的提交是将add的内容，加入注释后提交到本地代码仓库中
     * 若加参数a是将所有的代码更改都加入到代码仓库中，即使不在add中添加，这个要注意，尽量少用这个参数，容易造成change set不明晰。
 
 * push  
   **git push**
     * 将所有代码库提交的都push到远端代码仓库中,同时分支也是默认的
     * origin branch 是指具体提交到那个分支，比如**git push origin master** 提交到主分支 

 * pull 获取服务端最新的代码  
   **git pull**
  

至此，完成了最基本的代码提交与管理，可以进行最简单的push和pull了，是不是非常简单 *_*
(to be continued...)
