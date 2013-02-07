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

------

## Git 快速使用


### 基本安装
 * ubuntu : sudo apt-get install git git-core
 * windows: [msysgit](https://code.google.com/p/msysgit/)

### 最简单的使用

 * 初始化Git本地数据库   
   **git init**
 
 * 增加修改文件  
   **git add .**
     * . 增加当前目录下所有子文件和子目录
     * 可以在具体文件夹下增加，不一定在根目录执行这个命令

 * 删除文件  
   **git rm file**
     * 删除提交文件，但并没有永久的删除库文件
     * 该命令执行时，本地文件系统也对文件进行rm命令

 * 本地提交  
   **git commit -m "some code comments"**
     * 上面的提交是将add的内容，加入注释后提交到本地代码仓库中
     * 若加参数a是将所有的代码更改都加入到代码仓库中，即使不在add中添加，这个要注意，尽量少用这个参数，容易造成change set不明晰。
     * 修改刚才提交的注释，使用**git --amend**
 
 * push  
   **git push**
     * 将所有代码库提交的都push到远端代码仓库中,同时分支也是默认的
     * -u origin branch 是指具体提交到那个分支，比如**git push -u origin master** 提交到主分支 

 * pull 获取服务端最新的代码  
   **git pull**
 
 * clone 分支  
   **git clone https://xx.git -b 分支名**

至此，完成了最基本的代码提交与管理，可以进行最简单的push和pull了，是不是非常简单 *_*

------

## 实用技巧


### git 使用配置
 * git config –global user.name “tianwei”
 * git config –global user.email liutianweidlut@qq.com
 * HTTPS 免密码登陆:
   * $HOME 创建 .netrc 文件
   * 填入 machine github.com login [username] password [password]

### 版本控制
 * 查看历史 
  * git log 基本提交历史
  * git log -p 详细提交，包括代码文件变更情况（时间，版本，人员）
 
 * 查看当前工作
  * **git diff**
 
 * 会滚操作
  * **git revert 版本hash编号** 还原一个版本的修改
  * **git reset 版本号**  将当前的工作目录完全回滚到指定的版本号 （全部回滚）

 * tag 标签
  * **git tag revert_version hashcode**
  * 想查看该版本时，就可以使用 revert_version标签名，而不是哈希值了

 * 缓存当前工作区间
  * **git stash** 将当前未条件到本地和服务器的代码推入到git的栈中，此时工作区间和上一次提交的内容是完全一样的
  * **git stash apply** 可以将前一半的工作应用回来。 同时支持栈的多次提取
  * **git stash clear** 清空栈

### branch 分支
git 最强的功能就是创建分支与合并分支，进行快速干净的开发。
 * 创建分支 **git branch tianwei**
 * 查看分支 **git branch**
 * 查看远程分支 **git branch -r**
 * 切换工作分支 **git checkout tianwei**
   * checkout可以在不同分支切换
   * 还原代码，可以从上一个已提交的版本中更新和，未提交的内容全部回滚
 * 创建分支对于master的补丁文件 **git format patch master tianwei**
 * 强制删除分支 **git branch -D new_branch**
 * 获取远程分支 **git checkout -b 本地分支名 远程分支名**
 * 切换分支 **git rebase branchC branchG** 将分支从C移动到G

------
## 高级技巧


### 如何从版本库中永久删除文件?

有时候不小心将特别大的文件引入版本库中，只简单的用git rm不能将该文件在版本库中删除掉，会使其臃肿不堪。
我们需要将某些文件永久的删除，不留下任何痕迹，可以使用下面的方法。

    {% highlight python %}
        # 察看文件大小 
        du -hs
        # 寻找testme.txt文件
        git filter-branch --tree-filter 'rm -f testme.txt' HEAD
        # 列出所有的HEAD文件
        git ls-remote .
        # 删除相应的文件
        git update-ref -d refs/original/refs/heads/master bb383961a2d13e12d92be5f5e5d37491a90dee66
        # 删除log
        rm -rf .git/logs
        git reflog --all
        git prune
        git gc
        # check file size
        du -hs {% endhighlight %}

(to be continued...)
