---
title: 《Oracle教程》第一章
date: 2017-11-21 18:33:44
category: Oracle
tags: oracle
---
# Oracle第一章
![oraclelogo](https://github.com/No-Sky/storage/raw/master/pic/OracleLogo1.jpg)
                                                                      <!-- more -->

**oracle是什么就不介绍了，懂得人自然知道，不懂得不必知道。**

 1 首先打开Oracle服务<br>
 2 配置监听器（这个是因为教室的电脑Oracle安装有问题，没有配置好监听器）<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;开始菜单中找到net configration assistant添加一个监听器<br>
 3 用system用户登录sqlplus<br>
 4 解锁scott用户 :（也是因为教室的Oracle安装问题导致scott账户未解锁）<br>
	alter user scott account unlock;<br>
 5 修改scott密码:<br>
	alter user scott identified by tiger;<br>
 6 使用scott登录sqlplus<br>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;scott是oracle自带的一个实例账户，它带有四个实例表,其中重要的就是`emp`员工表与`dept`部门表
 7 安装PL/SQL第三方工具<br>
   因为Oracle没有自带的图形化界面管理器，所以我们需要安装PLSQL，它是oracle的一个第三方GUI工具

---
***介绍一下Oracle的命令***

 - 连接数据库：<br>
	connect scoott/tiger@orcl;         --用户名为scott，密码为tiger,数据库名为orcl
 - 显示当前用户：<br>
	show user;<br>
也可使用查询语句：<br>
	select USER from dual;              --dual是oracle的一个虚拟表
 - 显示表结构(以emp表为例)：<br>
	describe emp;<br>
可简写为：<br>
	desc emp;

