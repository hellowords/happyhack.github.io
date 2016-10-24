---
title: 在Windows中使用Gitlab
tags:
  - Gitlab
id: 44
categories:
  - Naive
date: 2015-11-17 16:34:25
update: 2016-10-23 03:17:43
---

在服务器上配置好了Gitlab之后，一开始只有系统用户，如果要添加用户有两种方法：

>   管理员手动添加

>   自己申请注册，填好信息通知管理员同意。
接下来就是要在本地进行文件的init,add,commit,push,clone.....不过在这些之前需要在网页上配置一下下。
<!-- more -->
首先登陆你的Gitlab账户，创建一个项目，这时候会有提示：如下图

![截图] (http://offt6br14.bkt.clouddn.com/bolg/img/QQ%E5%9B%BE%E7%89%8720151117162251-150x85.png)

打开之后会发现需要提供ssh密钥，这时候我们需要在Windows下有一个git客户端Git bash (自行下载)

下载安装之后鼠标右键会发现有gitbash，打开之后输入：<!--StartFragment -->
```bash
$ ssh-keygen -t rsa -C "happyhack@hehe.com"  //你在Gitlab注册时候的邮箱。
```
这时候会生成一个ssh密钥，你会发现在C:/user/.ssh这个文件夹下面，id_rsa.pub 这个文件就是密钥（yue）刚发现- -！
打开之后复制到网页上的ssh区域，title随便写~
接下来回到gitbash中，要配置一下你的user.email  user.name

```
git config --global user.name "happyhack"
git config --global user.email "happyhack@hehe.com"

```