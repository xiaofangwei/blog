---
title: Oracle笔记 （八）
date: 2017-11-28 19:10:30
comments: false
tags: oracle
---
# 子查询、集合运算

## 一、子查询

**通过把一个查询的结果作为另一个查询的一部分,子查询一般出现在SELECT语句的WHERE子句中，Oracle也支持在FROM或HAVING子句中出现子查询。
子查询比主查询先执行，结果作为主查询的条件，在书写上要用圆括号扩起来，并放在比较运算符的右侧。**

### 1、单行子查询

如：查询比SCOTT工资高的雇员名字和工资。
```SQL 
select ename,sal from emp where sal>(select sal from emp where empno=7788);
```

### 2、多行子查询*

**如果子查询返回多行的结果，则我们称它为多行子查询。多行子查询要使用不同的比较运算符号，它们是IN、ANY和ALL。**

如:查询工资低于任意一个“CLERK”的工资的雇员信息。
```SQL
select empno,ename,job,sal from emp where sal < any (select sal from emp where job = 'CLERK') and job <> 'CLERK';
```

如：	查询工资比所有的“SALESMAN”都高的雇员的编号、名字和工资。
```SQL
select empno,ename,job from emp where job in (select job from emp where deptno = 10) and deptno = 20;
```

### 3.多列子查询
**如果子查询返回多列，则对应的比较条件中也应该出现多列，这种查询称为多列子查询。以下是多列子查询的训练实例。
**
如： 查询职务和部门与SCOTT相同的雇员的信息。
```SQL
select empno, ename,sal from emp where (job,deptno) = (select job,deptno from emp where empno = 7788);
```
### 4．在FROM从句中使用子查询
**在FROM从句中也可以使用子查询，在原理上这与在WHERE条件中使用子查询类似。有的时候我们可能要求从雇员表中按照雇员出现的位置来检索雇员，很容易想到的是使用rownum虚列。比如我们要求显示雇员表中6～9位置上的雇员，可以用以下方法**

如：查询雇员表中排在第6～9位置上的雇员。
```SQL
select ename, sal, from (select rownum as num,ename,sal from emp where rownum<=9) where num>=6;
```

## 二、集合运算

|操作|描述|
|-|-|
|union|并集，合并两个操作的结果，去掉重复的部分|
|union all|并集，合并两个操作的结果，保留重复的部分|
|minus|差集，从前面的操作结果中去掉与后面操作结果相同的部分|
|intersect|交集，取两个操作结果中相同的部分|

如：查询部门10和部门20的所有职务。
```SQL
select job from emp where deptno = 10 
union
select job from emp where deptno = 20;
```

如：查询只在部门表中出现，但没有在雇员表中出现的部门编号。

```SQL
select deptno from dept
minus
select deptno from emp;
```