---
title: Oracle笔记 （七）
date: 2017-11-27 19:20:34
comments: false
tags: oracle
---
# 多表连接（二）、组函数、分组查询
## 一、不等连接

**拿一个表作为另一表的查询条件或范围**

如：显示雇员名称，工资和所属工资等级。
```SQL
select e.ename,e.sal,s.grade from emp e,salgrade s where e.sal between s.losal and s.hisal;
```

## 二、自连接

**自连接就是一个表，同本身进行连接。对于自连接可以想像存在两个相同的表(表和表的副本)，可以通过不同的别名区别两个相同的表（其它就是内连接)**

如：显示雇员名称和雇员的经理名称
```SQL
select worker.ename||'的经理是'||manager.ename as 雇员经理 from emp worker,emp manager where worker.mgr=manager.empno;
```

## 三、组函数

**
组函数只能应用于SELECT子句、HAVING子句或ORDER BY子句中。
组函数也可以称为统计函数。
组函数忽略列的空值。
对组可以应用组函数。
在组函数中可使用DISTINCT或ALL关键字。     
ALL表示对所有非NULL值(可重复)进行运算。         
DISTINCT 表示对每一个非NULL值，如果存在重复值，则组函数只运算一次。如果不指明上述关键字，默认为ALL。
**

|函数|说明|
|-|-|
|AVG|求平均值|
|COUNT|求计数值，返回非空行数，*表示返回所有行|
|MAX|求最大值|
|MIN|求最小值|
|SUM|求和|
|SIDDEV|求标准偏差，是根据差的平方根得到的|
|VARIANCE|求统计方差|

## 四、分组查询

**1、如：按职务统计工资总和。**
```SQL
select deptno,job,sum(sal) from emp group by deptno,job;
```

**2、多列分组**

如：按部门和职务分组统计工资总和。:
```SQL
select deptno,job,sum(sal) from emp group by deptno,job;
```

**3、HAVING**
*HAVING从句过滤分组后的结果，它只能出现在GROUP BY从句之后，而WHERE从句要出现在GROUP BY从句之前。*	

如：统计各部门的最高工资，排除最高工资小于3000的部门。
```SQL
select deptno,max(sal) from emp group by deptno having max(sal)>=3000;
```

**4、分组统计结果排序**
*可以使用ORDER BY从句对统计的结果进行排序，ORDER BY从句要出现在语句的最后。*	

如：按职务统计工资总和并排序。
```SQL
select job 职务, sum(sal) 工资总和 from emp group by job order by sum(sal);
```

**5、组函数的嵌套使用**

如：求各部门平均工资的最高值。
```SQL
select max(avg(sal)) from emp group by deptno;
```
