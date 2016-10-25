---
title: 使用Git管理hexo博客
date: 2016-10-25 15:29:16
updated: 2016-10-25 17:52:32
tags:
categories:
permalink:
---

将博客放到Github上，通过Github实现多终端管理博客 

#### 搭建步骤
1. [**安装必要软件**](https://hexo.io/zh-cn/docs/)

	##### [Node.js](https://nodejs.org/zh-cn/)
> 开个VPN访问更快  使用`nvm` 来安装`node.js` 

	1. Windows 
		* 安装 nvm
		讲道理nvm是没有Windows版本的，我从github上找到一个windows安装的方法 [点我！](https://github.com/coreybutler/nvm-windows) 
		[下载链接](https://github.com/coreybutler/nvm-windows/releases) 选`nvm-setup.zip` 安装完成之后，记得增加环境变量，`win+r` 输入`cmd` 打
		开命令终端，在终端输入`nvm` 出现信息则安装成功。
		* 安装 node.js
		Node.js的版本很多，选择安装版本号，在命令行中`nvm install 4.6.1` （**开个VPN~**）
		耐心等待一段时间...
		![nvm安装By happyhack.cn](http://offt6br14.bkt.clouddn.com/bolg/img/nvm.png)
		× 号表示当前使用的版本，`nvm use 4.6.1` 使用4.6.1版本。
	2. Linux
		* 安装 nvm
		* 安装 node.js
	3. Mac
		(木有Mac~)
	
##### [Git](https://git-scm.com/)
1. Windows
	[下载链接](https://git-scm.com/) 可以无脑下一步...配置帐号之类请看这篇 [博文](http://happyhack.cn/2015/12/21/2015-12-21-%E5%B8%B8%E7%94%A8Git%E5%91%BD%E4%BB%A4.html)
2. Linux(Ubuntu)
	`$ sudo apt-get install git`

##### 安装hexo
```
$ npm install -g hexo-cli
$ hexo init 		#当前目录下初始化一个博客
$ npm install	 	# 安装必要的包
```
#### 通过git管理hexo
在themes目录中下面删除`.git` 文件夹(如何你下载过主题)
```
$ cd hexo		#博客所在目录
$ git init		#初始化git仓库
$ git add . 	#写入快照
$ git commit -m '第一次提交' 	#commit
$ git add remote origin git@github.com:xx/xx.github.io.git  #xx是你github用户名
$ git push origin git_hexo   	# 分支名自定义
```
> 通过以上几步就将本地博客放到Github进行管理了

**通过另外一台电脑进行博客管理**
```
$ cd <folder> 			# 进入要存放博客的目录
$ git clone git@github.com:xx/xx.github.io.git 	# xx是你github用户名
$ cd xx.github.io 		# 进入博客目录
$ git checkout git_hexo 	# 切换分支,`master`分支为博客静态页面,`git_hexo`你博客代码分支
$ hexo new draft 使用Git管理hexo博客	# 新建一篇文章草稿 进行编写～
$ hexo publish 使用Git管理hexo博客		#放入_post中
$ hexo g d			#生成静态文件，部署到Github
$ git status		# 查看文件状态
$ git add . 		# 将变化文件写入快照
$ git commit -m '在另外一台电脑中写了一篇文章' # 提交
$ git push origin git_hexo 		# 推到Github
```
#### 切记
每次编辑时候切记一定要在**git_hexo**分支下编辑
使用Git管理hexo博客之后，每次都要先切到git_hexo所在分支拉取最新文件，以免覆盖之前文件
```
$ git fetch
$ git pull 	
```





