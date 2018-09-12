---
title: 《Oracle教程》第九章
date: 2017-11-29 18:49:15
category: Oracle
tags: oracle
---
# Oracle第九章——增删改、序列、事务

![](https://github.com/No-Sky/storage/raw/master/images/Logo/OracleLogo1.jpg)

 <!-- more -->
## 一、增删改
增： `insert into 表名(列名) values (值)；`

删： `delete from 表名 where 条件； `

改： `update 表名 set 列名1=值1，列名2=值2... where 条件；`

## 二、复制数据
**1、通过一条查询语句创建一个新表(要求目标表不存在)**

	create table manager as select empno,ename,sal, from emp where job= 'CLERK';

**2、通过一条查询语句复制数据(要求目标表必须已建好)**

	insert into manager select empno,ename,sal from emp where job = 'CLERK';

## 三、序列

### 1、创建序列

如：创建从2000起始，增量为1 的序列abc：

	create sequence abc increment by 1 start with 2000
	maxvalue 99999 cycle nocache;

### 2、使用序列

序列名.nextval: 代表下一个值

序列名.currval: 代表当前值

如：

	insert into manager values(abc.nextval,'小王',2500);
	insert into manager values(abc.nextval,'小赵'，2800);

## 三、事务
&nbsp;&nbsp;&nbsp;&nbsp;**两次连续成功的COMMIT或ROLLBACK之间的操作，称为一个事务。在一个事务内，数据的修改一起提交或撤销，如果发生故障或系统错误，整个事务也会自动撤销**<br>
&nbsp;&nbsp;&nbsp;&nbsp;**数据库事务处理可分为隐式和显式两种。显式事务操作通过命令实现，隐式事务由系统自动完成提交或撤销(回退)工作，无需用户的干预。**

1、隐式提交的情况包括：

&nbsp;&nbsp;&nbsp;&nbsp;当用户正常退出SQL*Plus或执行CREATE、DROP、GRANT、REVOKE等命令时会发生事务的自动提交。

2、显示事务:
<pre>
COMMIT	        数据库事务提交，将变化写入数据库
ROLLBACK	数据库事务回退，撤销对数据的修改
SAVEPOINT	创建保存点，用于事务的阶段回退
</pre>