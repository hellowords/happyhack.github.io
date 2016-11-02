---
title: 编译安装Gitlab
date: 2016-11-02 13:32:19
updated: 2016-11-02 15:17:31
tags:
 - Git
 - Gitlab
 - Ubuntu
categories:
 - Solution
permalink:
---

记录下在VPS主机上搭建Gitlab过程中遇到的一些问题

#### 编译安装Gitlab
* [国内安装手册](https://bbs.gitlab.cc/topic/3/gitlab-ce-8-1-3-%E5%AE%89%E8%A3%85%E6%89%8B%E5%86%8C-debian-ubuntu)
* [官方安装手册](https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md)

> 两个手册一起看，会有很大帮助

<!-- more -->
#### 安装过程遇到的问题

##### unicorn web 启动失败 #####
查看unicorn日志`/home/git/gitlab/log/unicorn.stderr.log`，我遇到两个问题*端口被占用*,*内存不足*。
1. 端口占用
	修改`$ vim /home/git/gitlab/config/unicorn.rb` 文件中的 `listen` 改成一个未使用的端口，如果
	端口是这种格式 `0.0.0.0:8888` 只能内网访问了，外网访问直接写 `8888` 端口号。

2. 内存不足
	这个是硬伤~VPS主机只有768的RAM，Gitlab需要2G的内存，另外分配一个2G左右的交换空间(swap)出来

	```bash
	$ free -m 	#查看系统Swap大小
	$ mkdir /swapfile && cd /swapfile	# 创建一个Swap文件
	$ sudo dd if=/dev/zero of=swap bs=1024 count=2000000  #分配2G空间
	$ sudo mkswap -f  swap	#将生成文件转换成Swap文件
	$ sudo swapon swap 	#激活Swap文件
	$ free -m 	#再次查看Swap大小
	$ sudo swapoff swap 	#卸载这个swap文件
	#如果要保持这个swap重启之后不失效,编辑 /etc/fstab 加入下面一行
	$ sudo vim /etc/fstab 	末尾加入： /swapfile       none    swap    sw      0       0 
	```

##### Nginx 代理出错 #####
拷贝Gitlab下面的nginx配置文件
```bash
sudo cp /home/git/gitlab/lib/support/nginx/gitlab /etc/nginx/sites-available/gitlab
```
修改`gitlab`配置文件
1. server_name  	test.example.com;
2. 增加监听端口
	在`upstream gitlab-workhorse` 块中增加一行 `server 127.0.0.0:8888` *这边的端口要和unicorn.rb中的端口一致*
3. 在`nginx.conf`中引入`gitlab`配置文件;`$ vim /etc/nginx/nginx.conf` 在server块最后加入
	`include /etc/nginx/sites-available/gitlab`

##### Git push 出错 #####
如果在Git操作中出现 GitLab: API is not accessible 错误，修改`$ vim /home/git/gitlab-shell/config.yml`
将gitlab_url: http://localhost/ 修改成配置的域名 gitlab_url: http://test.example.com/
同时`/home/git/gitlab/config/gitlab.yml`中的`host` 也不要忘记！

#### 重启Nginx,Gitlab
```bash
$ /etc/init.d/nginx restart
$ /etc/inid.d/gitlab restart
```
两个服务启动成功，Bingo！

#### 参考资料
> https://github.com/gitlabhq/gitlabhq/blob/master/doc/install/installation.md
https://bbs.gitlab.cc/topic/3/gitlab-ce-8-1-3-%E5%AE%89%E8%A3%85%E6%89%8B%E5%86%8C-debian-ubuntu
http://blog.csdn.net/wang_quan_li/article/details/49813279
https://www.digitalocean.com/community/tutorials/how-to-add-swap-on-ubuntu-12-04
http://www.joomla178.com/joomla-share/research-and-development/650-esolve-gitlab-api-is-not-accessible-when-pushing-to-a-new-repository.html

