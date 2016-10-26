---
title: WAMP server PHPMyAdmin 访问错误
tags:
  - wampserve
  - php
id: 102
categories:
  - Solution
date: 2015-12-21 14:33:39
---

重新装了系统之后，在安装了WAMP之后，访问localhost 和 PHPMyAdmin的时候都是403forbidden
这里附上解决办法：

1
####localhost的不能访问解决办法####
修改Apache的`httpd.conf` 文件

``` 
<Directory>
  Options FollowSymLinks
         AllowOverride None
         Order deny,allow
Deny from all
<Directory>
```
修改成下面 ：

```
<Directory>
      Options FollowSymLinks
        AllowOverride None
        Order deny,allow
 Allow from all
 <Directory>
```
2

```
order Deny,Allow
Deny from all
Allow from 127.0.0.1

And replace it with

Order Deny,Allow
Deny from all
Allow from All
```
PHPMyAdmin不能访问解决办法：
>修改wamp文件夹下的alias/phpmyadmin.conf 文件
