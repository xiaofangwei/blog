---
title: Git连接github超时问题解决方案
date: 2018-03-17 11:24:34
tags: git 超时
comments: false
---
因为电脑重新装了几次了导致  Git连接github时出现 了超时问题 ，如下：
``ssh: connect to host github.com port 22: Connection timed out    #使用ssh连接github时,出现timeout``
（Windows下）解决方案也很简单，修改ssh的配置文件。关于修改配置，存在两种解决方法，一种是C:\Program Files\Git\etc\ssh/ssh_config（你自己安装git的文件夹下面找）中修改全局配置，一种是在用户主目录下.ssh/中添加配置文件，这里我选择的后者（前者也可以解决问题，其实都可以）
<!--more-->
在Administrator/.ssh文件夹下新建`config`文件，注意，文件名就是config，没有后缀名
添加一下内容（在全局文件中同样是这个方案）：
```Bash
	Host github.com

	User git

	Hostname ssh.github.com

	PreferredAuthentications publickey

	IdentityFile ~/.ssh/id_rsa

	Port 443
```
