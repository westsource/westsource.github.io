---
layout:  post
title:  CentOS 6.5 下更新Python（转，有改动）
date:  2015-07-23 22:40:00
---

安装完CentOS 6.5后，执行#Python与#python -V，看到版本号是2.6.6，很老了，而且之前写的都是跑在python3.X上面的，需要更新成Python 3.X版本。

Python 3.X和2.X有很多不同，有兴趣的朋友可以参考下这篇文章： 

http://www.jb51.net/article/34011.htm

更新python千万不要把老版本的删除！新老版本是可以共存的，很多基本的命令、软件包都要依赖预装的老版本python的，比如yum。 


以下是更新Python的步骤。

#第1步：更新gcc，因为gcc版本太老会导致新版本python包编译不成功 

`yum -y install gcc` 

#第2步：下载Python-3.4.3软件包 

`wget https://www.python.org/ftp/python/3.4.3/Python-3.4.3.tgz`

注意：按照上述命令下载的软件包会存放在你当前的工作目录下，wget命令是一个从网络上自动下载文件的自由工具，具体用法，请参考这篇文章：http://www.jb51.net/os/RedHat/73089.html 

说明：命令中的数字就是版本号，你也可以把3.4.3换成你需要的版本，截止至我撰稿时，最新可用版本是3.4.3 

#第3步：解压已下载的二进制包并编译安装 

`tar -zxvf Python-3.4.3.tgz`

`cd Python-3.4.3` 

`./configure`

`make all`

`make install` 

`make clean` 

`make distclean` 

`/usr/local/bin/python3 –V `

#第4步：建立软连接指向到当前系统默认python命令的bin目录

让系统使用新版本python 

`mv /usr/bin/python /usr/bin/python2.6` //当前python的版本为2.4所以是python2.4 

`ln -s /usr/local/bin/python3.4 /usr/bin/python` 

输入

`python -V`

即可查看当前默认python版本 

默认的python成功指向3.4.3以后，yum不能正常使用，需要修改yum的配置文件 


#第5步：修改yum配置文件 

`vi /usr/bin/yum` 

把文件头部的`#!/usr/bin/python`改成`#!/usr/bin/python2.6` //改为之前的老版本号 

保存退出，yum即可正常使用。如若有其他命令、软件不能正常使用，仿照yum配置文件的修改方法，修改其配置文件即可。 

至此，更新完毕。

原文：http://www.jb51.net/article/34012.htm