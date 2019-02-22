---
title: Oracle笔记 （三）
date: 2017-11-23 18:48:46
tags: oracle
---
# Oracle的基本查询
![](https://github.com/No-Sky/storage/raw/master/images/Logo/OracleLogo1.jpg)
 <!-- more -->

## 一、基本查询
*select格式：*<br>
&nbsp;&nbsp;&nbsp;&nbsp;select 列名 from 表名 ；
&nbsp;&nbsp;&nbsp;&nbsp;where 查询条件
&nbsp;&nbsp;&nbsp;&nbsp;group by 分组列
&nbsp;&nbsp;&nbsp;&nbsp;having 分组后条件
&nbsp;&nbsp;&nbsp;&nbsp;order by 排序列 asc[desc]

如：查询部门10的雇员
	
```SQL
select * from emp where deptno=10;
```

## 二、行号（rownum）
**每个表都有一个虚列ROWNUM，它用来显示结果中记录的行号。我们在查询中也可以显示这个列。**

如：显示emp表的行号
```SQL
select rownum,ename from emp;
```

如：显示前三行
```SQL
select * from emp where rownum<=3;
```
## 三、查询进行计算
如：显示雇员工资上浮20%的结果
	select ename,sal,sal*(1+20%) from emp;
如：显示每个员工的总工资（工资+奖金）
	update emp set comm = o where comm is null;    //因为null的特殊性，它与任何值运算都等于null，所以先要把它更新为0，后面我们会学到一个函数来处理null值
	select ename,sal+comm from emp;
## 四、使用别名
如：在查询中使用列别名
```SQL
select ename as 名称，sal as 工资 from emp; //建议省略as
```
*另，在别名为关键字或有特殊符号时需要加双引号*

如：
```SQL
select ename as "select",sal*12+5000 as "年度工资（加年终奖）" from emp;
```

## 五、连接运算符
**连接运算符是双竖线“||”。通过连接运算可以将两个字符串连接在一起。**

如：在查询中使用连接运算
```SQL
select ename||job as "雇员和职务表" from emp;
```
*注意：'5'||5结果为'55'&nbsp;&nbsp;&nbsp;&nbsp;'5'+5结果为 10 *

## 六、消除重复行（distinct）
**如果在显示结果中存在重复行，可以使用关键字`distinct`消除重复显示**
如：统计职务的数量
```SQL
select count(distinct job) from emp;
```
## 七、排序
**1、升序（默认为升序`asc`,所以可以忽略）**

如：查询雇员姓名和工资，并按工资从小到大排序
```SQL
select ename,sal from emp order by sal asc;
```
**2、降序（`desc`不可忽略）**

如：查询雇员姓名和雇佣日期，并按雇佣日期排序，后雇佣的先显示
```SQL
select ename,hiredate from emp order by hiredate desc;
```

**3、多列排序**

*可以按多列进行排序，先按第一列，然后按第二列、第三列......。*

如：查询雇员信息，先按部门从小到大排序，再按雇佣时间的先后排序
```SQL
select ename,deptno,hiredate from emp order by deptno hiredate;
```
