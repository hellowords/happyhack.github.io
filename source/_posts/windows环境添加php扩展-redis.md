---
title: Windows环境添加PHP扩展--Redis
date: 2016-12-05 14:41:06
updated: 2016-12-05 15:35:17
tags:
	- php
	- redis
categories:
	- Solution
permalink:
---
##### 所需文件下载
　选择对应版本下载
* [Windows-Redis](https://github.com/MSOpenTech/redis/releases)
* [PHP-Redis](http://windows.php.net/downloads/pecl/snaps/redis/)

<!-- more -->
##### 安装
1. Redis
　安装过程中注意添加环境变量，安装完成后启动Redis时候，需要带配置文件，举个茄子
` redis-server.exe redis.windows.conf` 显示一个大图标安装成功

2. PHP-Redis扩展
　将下载扩展文件 `php_redis.dll` 放入php安装文件夹`ext`下，EX:`E:\wamp64\bin\php\php7.0.10\ext`
修改`php.ini`文件，加入 `extension=php_redis.dll`
　完成以上步骤，将php安装文件夹中的以下文件复制到`C:\Windows\System32` 中
```files
icudt57.dll
icuin57.dll
icuio57.dll
icule57.dll
iculx57.dll
icuuc57.dll
lib*.dll
```
> 重启Wampserver！Bingo！
