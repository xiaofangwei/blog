---
title: Oracle存储过程的参数为默认值
date: 2017-12-12 19:47:57
category: 易错集锦
tags: oracle
---
# 当Oracle的存储过程的参数有默认值时
![oraclelogo](https://github.com/No-Sky/storage/raw/master/pic/OracleLogo1.jpg)
                                                           <!-- more -->


**首先看例题**

创建插入雇员的存储过程INSERT_EMP，并将雇员编号等作为参数。
在设计存储过程的时候，也可以为参数设定默认值，这样调用者就可以不传递或少传递参数了。

	 create or replace procedure INSERT_EMP(pempno number,pename varchar2,
	 pjob varchar2,pmgr number,phiredate date default sysdate,
	 psal number,pcomm number default 0,pdeptno number default 10)
	 as
	 begin
	   insert into emp values(pempno,pename,pjob,pmgr,phiredate,psal,pcomm,pdeptno);
	 end;
	 
	--不适用默认值调用方式
	 begin
	   INSERT_EMP(9528,'周星星','CLERK',7839,2500,sysdate,2500,0,20);
	 end;
 	--使用默认值调用
	begin
	  INSERT_EMP(9528,'周星星','CLERK',7839,2500,2500);
	end;

*注意：*使用默认值时，存储过程的参数列表的最后一个参数必须使用默认值，否则无法使用。