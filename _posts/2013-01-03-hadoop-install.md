---
date: 2013-01-03 21:00:00
layout: post
title: MapReduce之hadoop：安装与配置
categories:
    - Tech
tags: 
    - MapReduce
    - OpenSource
    - Python
    - Dumbo
comments: yes
---

实习回来给创新院配置hadoop集群环境，总共27台机器。

1. 硬盘分区

 * 第一块硬盘 2TB 为 sda （引导磁盘）（EXT4）
  * /boot 2GB (主分区)
  * / 550GB
  * swap  4G
  * /usr    500GB （将/usr 不易变化独立出来）
  * /tmp   450GB （将tmp,var 经常变化的分区独立出来，提高磁盘效率）
  * /var    450GB
 * 第二块硬盘 2TB 为 sdb （EXT4）
  * /home 1.8TB (主分区)
  * swap   4GB（两个磁盘都加入swap分区）  

2. ssh 配置
 * ssh hadoop@192.168.2.91 “mkdir .ssh;chmod 0700 .ssh”   #创建相应的文件夹
 * scp  ~/.ssh/* hadoop@192.168.2.88:~/.ssh/    #拷贝认证文件
 * 当出现无法免密码登陆时，卸载openssh-server,删除~/.ssh，重新建立.ssh，设定权限为0700,权限对RSA登陆非常重要。
 
3. 配置hostséç½®hosts
