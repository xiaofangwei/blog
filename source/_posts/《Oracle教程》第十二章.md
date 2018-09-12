---
title: 《Oracle教程》第十二章
date: 2017-12-05 18:40:55
category: Oracle
tags: oracle
---
# Oracle第十二章——索引、同义词、数据库链接、PL/SQL语句

![](https://github.com/No-Sky/storage/raw/master/images/Logo/OracleLogo1.jpg)

 <!-- more -->

## 一、索引

**索引(INDEX)是为了加快数据的查找而创建的数据库对象，特别是对大表，索引可以有效地提高查找速度，也可以保证数据的惟一性**

**创建索引一般要掌握以下原则：只有较大的表才有必要建立索引，表的记录应该大于50条，查询数据小于总行数的2%～4%。虽然可以为表创建多个索引，但是无助于查询的索引不但不会提高效率，还会增加系统开销。因为当执行DML操作时，索引也要跟着更新，这时索引可能会降低系统的性能。**

创建索引：  
         
     CREATE INDEX 索引名 ON 表名(列名);

删除索引：

      DROP INDEX 索引名；

## 二、同义词

**   同义词(SYNONYM)是为模式对象起的别名，可以为表、视图、序列、过程、函数和包等数据库模式对象创建同义词。**

创建私有同义词：

      CREATE SYNONYM BOOK FOR 图书；

创建公有同义词(先要获得创建公有同义词的权限)：

      CREATE PUBLIC SYNONYM BOOK FOR SCOTT.图书；

删除同义词：

	DROP SYNONYM 同义词名；

## 三、数据库链接
 ** 数据库链接(DATABASE LINK)是在分布式环境下，为了访问远程数据库而创建的数据通信链路。**

格式：

     CREATE DATABASE LINK 链接名 CONNECT TO 账户 IDENTIFIED BY 口令 USING 服务名;

  数据库链接一旦建立并测试成功，就可以使用以下形式来访问远程用户的表。

		表名@数据库链接名

## 四、PL/sql

### 1、块结构和基本语法要求

块中各部分的作用解释如下：

		(1)  DECLARE：声明部分标志。

		(2)  BEGIN：可执行部分标志。

		(3)  EXCEPTION：异常处理部分标志。

		(4)  END；：程序结束标志。
### 2、输出

第一种形式：

		DBMS_OUTPUT.PUT(字符串表达式)；

第二种形式：

		DBMS_OUTPUT.PUT_LINE(字符串表达式)；

第三种形式：

		DBMS_OUTPUT.NEW_LINE；

### 3、变量赋值：

第一种形式：
	SELECT 列名1，列名2... INTO 变量1，变量2... FROM 表名 WHERE 条件；第二种形式：变量名:=值

例：查询雇员编号为7788的雇员姓名和工资。

	SET SERVEROUTPUT ON		--在命令行界面必须写
	DECLARE--定义部分标识		
	 v_name  VARCHAR2(10);	--定义字符串变量v_name		
	 v_sal   NUMBER(5);	--定义数值变量v_sal		
	BEGIN			--可执行部分标识SELECT	 ename,sal INTO v_name,v_sal 
	 FROM emp 
	 WHERE empno=7788;--在程序中插入的SQL语句
	  		
	DBMS_OUTPUT.PUT_LINE('7788号雇员是：'||v_name||'，工资为：'||to_char(v_sal));
			--输出雇员名和工资		
	END;	

### 4、结合变量的定义和使用（即全局变量）

 **  该变量是在整个SQL*Plus环境下有效的变量，在退出SQL*Plus之前始终有效，所以可以使用该变量在不同的程序之间传递信息。结合变量不是由程序定义的，而是使用系统命令VARIABLE定义的。**

例：定义并使用结合变量

步骤1：输入和执行下列命令，定义结合变量g_ename：
			
	--SET SERVEROUTPUT ON 
		VARIABLE  g_ename VARCHAR2(100)		
	BEGIN
		:g_ename:=:g_ename|| 'Hello~ ';			--在程序中使用结合变量
		DBMS_OUTPUT.PUT_LINE(:g_ename);                --输出结合变量的值
	END;

### 5．记录变量的定义
**还可以根据表或视图的一个记录中的所有字段定义变量，称为记录变量。记录变量包含若干个字段，在结构上同表的一个记录相同，定义方法是在表名后跟%ROWTYPE。记录变量的字段名就是表的字段名，数据类型也一致。**

如：
	 v_name emp.ename%TYPE;