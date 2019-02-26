---
title: Maven与eclipse
date: 2018-04-04 22:23:49
top: 1
tags: Maven
---
<h1>Maven配置与eclipse配置Maven仓库</h1>

# 下载与安装

1、前往 https://maven.apache.org/download.cgi 下载最新版的Maven程序：

![](https://github.com/No-Sky/storage/raw/master/images/maven/mavendownload.png)
<!-- more -->
2、将文件解压到F:\Program Files\Apache\apache-maven-3.5.3（此处是我的安装路径）目录下:
​	
![](https://github.com/No-Sky/storage/raw/master/images/maven/maveninstaller.png)

3、新建环境变量MAVEN_HOME，赋值F:\Program Files\Apache\apache-maven-3.5.3
​	
![](https://github.com/No-Sky/storage/raw/master/images/maven/mavenpath1.png)

4、编辑环境变量Path，追加`%MAVEN_HOME%\bin\;`

![](https://github.com/No-Sky/storage/raw/master/images/maven/mavenpath2.png)

5、至此，maven已经完成了安装，我们可以通过DOS命令检查一下我们是否安装成功
​	
	mvn -v

![](https://github.com/No-Sky/storage/raw/master/images/maven/mavenversion.png)

# 配置Maven本地仓库

1、在F:\Program Files\Apache\目录下新建maven-repository文件夹，该目录用作maven的本地库。

2、打开F:\Program Files\Apache\apache-maven-3.5.3\conf\settings.xml文件，查找下面这行代码：
​	
	<localRepository>/path/to/local/repo</localRepository>
*localRepository节点默认是被注释掉的，需要把它移到注释之外，然后将localRepository节点的值改为我们在3.1中创建的目录F:\Program Files\Apache\maven-repository*

3、localRepository节点用于配置本地仓库，本地仓库其实起到了一个缓存的作用，它的默认地址是 C:\Users\用户名.m2。

当我们从maven中获取jar包的时候，maven首先会在本地仓库中查找，如果本地仓库有则返回；如果没有则从远程仓库中获取包，并在本地库中保存。

此外，我们在maven项目中运行mvn install，项目将会自动打包并安装到本地仓库中。

4、运行一下DOS命令

	mvn help:system
*如果前面的配置成功，那么F:\Program Files\Apache\maven-repository会出现一些文件。*

# 配置Eclipse的Maven环境

1、Eclipse Oxygen，打开Window->Preferences->Maven->Installations，右侧点击Add。

![](https://github.com/No-Sky/storage/raw/master/images/maven/maven-eclipse01.png)

2、设置maven的安装目录，然后Finish

![](https://github.com/No-Sky/storage/raw/master/images/maven/maveneclipse02.png)

3、 选中刚刚添加的maven，并Apply。

![](https://github.com/No-Sky/storage/raw/master/images/maven/maveneclipse03.png)

4、 打开Window->Preferences->Maven->User Settings，配置如下并Apply：

![](https://github.com/No-Sky/storage/raw/master/images/maven/maveneclipse04.png)

至此，Maven的安装和配置全部结束。