---
layout:  post
title:  CentOS 6.5 以单机模式安装Spark
date:  2015-07-19 22:18:00
---

安装过程包含三步：安装Oracle Java、安装Scala和安装Spark。

#1.安装Oracle Java

##第一步 卸载原来的Java

输入如下命令，查看是否安装了Java及Java的版本信息：

`java -version` 

以下是我的服务器的返回结果：

> java version "1.7.0_55"

>  OpenJDK Runtime Environment (rhel-2.4.7.1.el6_5-x86_64 u55-b13)

> OpenJDK 64-Bit Server VM (build 24.51-b03, mixed mode)

接下来看JDK信息：

`rpm -qa|grep java`

以下是我的服务器返回的结果：

> tzdata-java-2014e-1.el6.noarch

> java-1.7.0-openjdk-1.7.0.55-2.4.7.1.el6_5.x86_64

> java-1.6.0-openjdk-1.6.0.0-5.1.13.3.el6_5.x86_64

接下来需要卸载已安装的Java:

`yum -y remove tzdata-java-2014e-1.el6.noarch`

`yum -y remove java-1.7.0-openjdk-1.7.0.55-2.4.7.1.el6_5.x86_64`

`yum -y remove java-1.6.0-openjdk-1.6.0.0-5.1.13.3.el6_5.x86_64`

返回一堆卸载的过程日志信息。

##第二步 安装Oracle JDK

首先下载JDK包

`wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F;oraclelicense=accept-securebackup-cookie" "http://download.oracle.com/otn-pub/java/jdk/7u60-b19/jdk-7u60-linux-x64.tar.gz"`

然后建立放置JDK的文件夹，并将刚才下载的包拷贝到此文件夹中

`mkdir /usr/local/java`

`cp jdk-7u60-linux-x64.tar.gz /usr/local/java`

到/usr/local/java目录下，解压此压缩包，并修改解压生成的文件夹名

`cd /usr/local/java`

`tar -xzvf jdk-7u60-linux-x64.tar.gz`

`mv jdk-7u60-linux-x64 jdk1.7`

最后，设置环境变量

`vi /etc/profile`

注释掉以下行：

> export PATH USER LOGNAME MAIL HOSTNAME HISTSIZE HISTCONTROL

在其下增加JAVA_HOME、CLASSPATH，修改PATH，如下：

> export JAVA_HOME=/usr/java/jdk1.7

> export PATH=$JAVA_HOME/bin:$SCALA_HOME/bin:$PATH

> export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar

然后执行配置文件，令其立刻生效，输入如下命令： 

`source /etc/profile`

最后测试一下，在命令行中输入

`java -version` 

现在我们可以看到新安装的Java：

>java version "1.7.0_60"

>Java(TM) SE Runtime Environment (build 1.7.0_60-b19)

>Java HotSpot(TM) 64-Bit Server VM (build 24.60-b09, mixed mode)

#2.安装Scala

首先下载Scala最新版本：

`wget http://downloads.typesafe.com/scala/2.11.7/scala-2.11.7.tgz`

然后建立放置Scala的文件夹，并将刚才下载的包拷贝到此文件夹中：

`mkdir /usr/local/scala`

`cp scala-2.11.7.tgz /usr/local/scala`

解压：

`tar -zxvf scala-2.11.7.tgz`

再修改profile文件：

`vi /etc/profile`

增加SCALA_HOME，PATH：

> export SCALA_HOME=/usr/local/scala/scala-2.11.7

> export PATH=$JAVA_HOME/bin:$SCALA_HOME/bin:$PATH

然后执行配置文件，令其立刻生效，输入如下命令： 

`source /etc/profile`

最后，我们测试一下，输入以下语句：

`scala`

以下是我的服务器的返回：

> Welcome to Scala version 2.11.7 (Java HotSpot(TM) 64-Bit Server VM, Java 1.7.0_60).

> Type in expressions to have them evaluated.

> Type :help for more information.

> scala> 

OK，Scala安装成功了。

#3.安装Spark

同样的，首先下载Spark for Hadoop的最新版本（单机）：

`wget http://d3kbcqa49mib13.cloudfront.net/spark-1.4.1-bin-hadoop2.6.tgz`

然后建立放置Spark的文件夹，并将刚才下载的包拷贝到此文件夹中，解压：

`mkdir /usr/local/spark`

`cp spark-1.4.1-bin-hadoop2.6.tgz /usr/local/spark`

`tar -zxvf spark-1.4.1-bin-hadoop2.6.tgz`

编辑profile文件，增加SPARK_HOME：

`vi /etc/profile`

> export SPARK_HOME=/usr/spark/spark/spark-1.4.1-bin-hadoop2.6

然后，我们到Spark目录中执行一下Spark Shell交互脚本：

`cd /usr/local/spark/spark-1.4.1-bin-hadoop2.6`

`bin/spark-shell`

然后，编写一点测试的代码：

> scala> val textFile = sc.textFile("README.md")

> scala> textFile.count()

看看我的执行结果：

> xxx
>
> xxx
> 
> res0: Long = 127

OK，大功告成了。

咱还可以看看WEB监控页面：

![WEB监控台](/images/2015-07-20_22-32-12.png)