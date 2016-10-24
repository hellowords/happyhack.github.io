---
title: Goagent在windows与ubuntu中使用方法
tags:
  - gfw
  - goagent
id: 60
categories:
  - 干货
date: 2015-11-25 14:43:48
---

前几天公司同事帮我在我的虚拟主机上面配置了一下proxy，在本地可以通过Goagent方式畅游互联网！

（第一次上互联网，好紧张，怎么才能表现出经常上，急！在线等！）

先说下载Windows下面使用方法吧：

1、首先你得有一个国外的虚拟主机，然后在其中建一个网站，记住网站路径，并且上传server中的PHP文件夹中的四个文件上去（文末我会给所需软件下载链接）其中上传的文件中你可以对index.php中做相关的一些修改，密码(与local中的proxy.ini 的PHP段的密码要一致。

2、如果你有Firefox或者chrome，太好不过了！在Firefox中修改代理很简单，这个方法在Ubuntu 14-04（15-X）中也适用，主要Ubuntu中不是GNOME的桌面，没有chrome，有一个替代版XXX，不可以修改代理，Ubuntu的系统代理修改了之后我也没有成功过，于是我选择了Firefox。

*   解压下载的Goagent，进入local文件夹，以管理员身份运行goagent.exe，测试能不能运行成功，接下来修改local文件夹中的proxy.ini 里面是你在虚拟主机上配置的一些程序路径，大约在97行PHP段中fetchserver修改成你虚拟主机中上传server中PHP的路径，举个茄子：fetchserver = http://qiezi.com/tudou/index.php  (其中土豆下面就是你放那个四个文件的目录)
*   修改好了之后，重新以管理员身份运行goagent.exe了，设置IE代理为8088的那一个！
*   在Firefox中选项-&gt;高级-&gt;网络-&gt;连接
[![](http://www.hiwud.com/wp-content/uploads/2015/11/L0KSOZJE_FQTH1@F2BDQ.png)](http://www.hiwud.com/wp-content/uploads/2015/11/L0KSOZJE_FQTH1@F2BDQ.png)

修改为手动代理，

[![哈罗](http://www.hiwud.com/wp-content/uploads/2015/11/JWP01NW_7UY4P2MHL5.png)](http://www.hiwud.com/wp-content/uploads/2015/11/JWP01NW_7UY4P2MHL5.png)

<span style="color: #00ccff;">**  在Firefox中输入google.com试试看，是不是阔以”上网“啦~~~**</span>

*   还有一个办法就是更改系统的代理，我之前在安装Windows的时候Internet Explorer被我禁用了。在打开呗~能怎么办呢~~在explorer中工具-&gt;Internet选项-&gt;连接-&gt;局域网配置
[![explorer](http://www.hiwud.com/wp-content/uploads/2015/11/7P9TYZGMOUUZU3_VC4.png)](http://www.hiwud.com/wp-content/uploads/2015/11/7P9TYZGMOUUZU3_VC4.png)

*   这里有一个需要注意，外过大部分网站采用https方式，所以可能Firefox在连接的时候回说没有证书，这是我们要手动导入证书！local文件下面有，<span style="text-decoration: underline;">CA.crt  </span>手动导入到FF中就可以啦~
*   在Ubuntu中的方法唯一不同的就是启动GoAgent的方式不一样了，在终端中cd 到文件所在文件夹，用python 命令启动 python.py （可能要增加一个执行的权限，或者用root）
百度网盘下载链接：<del>http://pan.baidu.com/s/1hqLfFHM</del>

更新上面的链接没用了，要的话评论里面留邮箱吧！

&nbsp;

&nbsp;