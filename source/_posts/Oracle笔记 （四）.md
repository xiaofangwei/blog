---
title: Oracle笔记 （四）
date: 2017-11-24 18:02:50
comments: false
tags: oracle
---
# 条件查询、函数（一）

## 一、 条件查询
**1、模糊查询(between、in、like)**
* A、between：在某某之间。如,显示工资在1000~2000之间的雇员
```SQL
select * from emp where sal beteween 1000 and 2000;
```
* B、in：在某某之间。如，显示职务为“SALMAN”，“CLEARK”和“MANAGER”的雇员信息
```SQL
select * from emp where job in ('SALMAN','CLERK','MANAGER');
```
* C、like：与通配符使用
*通配符：% 代表0个或任意个字符&nbsp;&nbsp;&nbsp;  —_ 代表1个字符*
如：显示姓名以“S”开头的雇员信息。
```SQL
select * from emp where ename like 'S%';
```
显示姓名第二个字符为“A”的雇员信息
```SQL
select * from emp ename like '_A%';
```
**2、空值查询**
*空：is null  &nbsp;&nbsp;&nbsp; 非空： is not null*
如：查询奖金为空的雇员信息
```SQL
select * from emp where comm is null;
```
## 二、函数
**1、数学函数**
<table><tr><td>函数</td><td>功能</td><td>实例</td><td>结果</td></tr><tr><td>abs</td><td>求绝对值函数</td><td>abs(-5)</td><td>5</td></tr><tr><td>sqrt<td><td求平</td><td>sqrt(2</td><td>1.4142...</td></tr><tr><td>power</td><td>求幂函数</td><td>power(2,3)</td><td>8</td></tr></table>

练习：

使用求绝对值函数abs
```SQL
select abs(-5) from dual;
```
使用求平方根函数sqrt。
```SQL
select sqrt(2) from dual;
```
使用ceil函数。
```SQL
select ceil(2.35) from dual;
```
使用floor函数。
```SQL
select floor(2.35) from dual;
```
**2、使用四舍五入函数round** <small> 格式：round(数字，保留的位数)</samll>
例：
```SQL
select round(45.923,2), round(45.923,0), round(45.923,-1) from dual;
```
**3、字符型函数**	
<table><tr><td>ascii</td><td>返回与ASCII码相应的字符</td><td>Ascii('A')</td><td>65</td></tr><td>char</td><td>返回与ASCII码相应的字符</td><td>char(65)</td><td>A</td></tr><tr><td>lower</td><td>将字符串转换成小写</td><td>lower ('SQL Course')</td><td>sql course</td></tr><tr><td>upper</td><td>将字符串转换成</td><td>upper('SQL Course')</td><td>SQL COURSE</td></tr><tr><td>initcap</td><td>将字符串转换成每个单词以大写开头</td><td>initcap('SQL course')</td><td>SQL Course</td></tr><tr><td>concat</td><td>连接两个字符串</td><td>concat('SQL', ' Course')</td><td>SQL Course</td></tr><tr><td>substr</td><td>给出起始位置和长度，返回子字符串</td><td>substr('String',1,3)</td><td>Str</td></tr><tr><td>length</td><td>求字符串的长度</td><td>length('Wellcom')</td><td>7</td></tr><tr><td>trim</td><td>在一个字符串中去除另一个字符串</td><td>trim('S' FROM 'SSMITH')</td><td>MITH</td></tr><tr><td>replace</td><td>用一个字符串替换另一个字符串中的子字符串</td><td>replace('ABC', 'B', 'D')</td><td>ADC</td></tr></table>

练习：
如果不知道表的字段内容是大写还是小写，可以转换后比较。
```SQL
select empno, ename,deptno from emp where lower(ename)='blake';
```
显示名称以“W”开头的雇员，并将名称转换成以大写开头。 
```SQL
select empno,initcap(ename),job from emp wher substr(ename,1,1)='W';
```
显示雇员名称中包含“S”的雇员名称及名称长度。
```SQL
select empno,ename,legth(ename) from emp where instr(ename,'S',1,1)>0;
```

## 4、日期型函数
**Oracle使用内部数字格式来保存时间和日期，包括世纪、年、月、日、小时、分、秒。缺省日期格式为 DD-MON-YY，如“08-05月-03”代表2003年5月8日。SYSDATE是返回系统日期和时间的虚列函数。**
待续。。。




