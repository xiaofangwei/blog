---
title: Oracle笔记 （五）
date: 2017-11-25 19:20:38
comments: false
tags: oracle
---
# 函数（二
## 一、日期型函数
**Oracle使用内部数字格式来保存时间和日期，包括世纪、年、月、日、小时、分、秒。缺省日期格式为 DD-MON-YY，如“08-05月-03”代表2003年5月8日。**

1、SYSDATE：返回系统日期和时间的虚列函数。

如：返回系统的当前日期。
```SQL
SELECT sysdate FROM dual;
```
2、对两个日期相减，得到相隔天数。

*通过加小时来增加天数，24小时为一天，如12小时可以写成12/24(或0.5)。*
如：例1 假定当前的系统日期是2003年2月6日，求再过1000天的日期。
```SQL
SELECT sysdate+1000 AS "NEW DATE" FROM dual;
```
例2：两个日期相减
```SQL
select to_date('1-1月-2000') - to_date('1-8月-1999') from dual;
```
3、其它日期函数
|函数|功能|实例|结果|
|-|-|-|-|
|months_between|返回两个日期间的月份|months_between ('04-11月-05','11-1月-01')57.7741935|
|add_months|返回把月份数加到日期上的新日期|add_months('06-2月-03',1)，add_months('06-2月-03',-1)|06-3月-03，06-1月-03|
|next_day|返回指定日期后的星期对应的新日期|next_day('06-2月-03','星期一')|10-2月-03|
|last_day|返回指定日期所在的月的最后一天|last_day('06-2月-03')|28-2月-03|
|round|按指定格式对日期进行四舍五入|round(to_date('13-2月-03'),'YEAR')，round(to_date('13-2月-03'),'MONTH')，round(to_date('13-2月-03'),'DAY')|01-1月-03，01-2月-03，16-2月-03(按周四舍五入)|

如：返回2003年2月的最后一天。
```SQL
SELECT last_day('08-2月-03') FROM dual;
```
假定当前的系统日期是2003年2月6日，显示部门10雇员的雇佣天数。
```SQL
SELECT ename, round(sysdate-hiredate) DAYS FROM emp WHERE  deptno = 10;
```
## 二、转换函数

|函数|功能|实例|结果|
|-|-|-|-|
|To_char|转换成字符串类型|To_char(1234.5, '$9999.9')|$1234.5|
|To_date|转换成日期类型|To_date('1980-01-01', 'yyyy-mm-dd')|01-1月-80|
|To_number|转换成数值类型|To_number('1234.5')|1234.5|

1．自动类型转换
```SQL
SELECT '12.5'+11 FROM dual;    //结果为：23.5
SELECT  ‘12.5’||11 FROM dual;    //结果为：’12.511’
```
2．日期类型转换

|代码|代表的格式|例子|
|-|-|-|
|AM、PM|上午、下午|08 AM|
|D|数字表示的星期(1～7)|1,2,3,4,5,6,7|
|DD|数字表示月中的日期(1～31)|1,2,3,…,31|
|MM|两位数的月份|01,02,…,12|
|Y、YY、YYY、YYYY|年份的后几位|3,03,003,2003|
|RR|解决Y2K问题的年度转换| |
|DY|简写的星期名|MON,TUE,FRI,…|
|DAY|全拼的星期名|MONDAY,TUESDAY,…|
|MON|简写的月份名|JAN,FEB,MAR,…|
|MONTH|全拼的月份名|JANUARY,FEBRUARY,…|
|HH、HH12|12小时制的小时(1～12)|1,2,3,…,12|
|HH24|24小时制的小时(0～23)|0,1,2,…,23|
|MI|分(0～59)|0,1,2,…,59|
|SS|秒(0～59)|0,1,2,…,59|
|,./-;:|原样显示的标点符号| |
|'TEXT'|引号中的文本原样显示|TEXT|

如：1、日期型转字符型 

将日期转换成带时间和星期的字符串并显示。
```SQL
SELECT TO_CHAR(sysdate,'YYYY-MM-DD HH24:MI:SS AM DY') FROM dual;
```

将日期显示转换成中文的年月日。
```SQL
SELECT TO_CHAR(sysdate,'YYYY"年"MM"月"DD"日"') FROM dual;
```
2.字符型转日期型

往emp表中插入一条记录
```SQL
insert into emp values(8888,'张三','CLERK',7369,to_date('1-1月-2000'),1000,10,10);
insert into emp values(8889,'李四','CLERK',7369,to_date('2000-01-01','YYYY-MM-DD'),1000,10,10);
```

## 三、其他常用函数
|函数|功能|实例|结果|
|-|-|-|-|
|nvl|空值转换函数|nvl(null, '空')|空|
|decode|实现分支功能|decode(1,1, '男', 2, '女')|男|
|userenv|返回环境信息|userenv('LANGUAGE')|SIMPLIFIED CHINESE_CHINA.ZHS16GBK|
|greatest|返回参数的最大值|greatest(20,35,18,9)|35|
|least|least返回参数的最小值|least(20,35,18,9)|9|

### 1．空值的转换

*如果对空值NULL不能很好的处理，就会在查询中出现一些问题。在一个空值上进行算术运算的结果都是NULL。最典型的例子是，在查询雇员表时，将工资sal字段和津贴字段comm进行相加，如果津贴为空，则相加结果也为空，这样容易引起误解。*

**使用nvl函数，可以转换NULL为实际值。该函数判断字段的内容，如果不为空，返回原值；为空，则返回给定的值。**

如下3个函数，分别用新内容代替字段的空值：
```
nvl(comm, 0)：用0代替空的Comm值。
nvl(hiredate, '01-1月-97')：用1997年1月1日代替空的雇佣日期。
nvl(job, '无')：用“无”代替空的职务。
```

使用nvl函数转换空值。
```SQL
SELECT	ename,nvl(job,'无'),nvl(hiredate,'01-1月-97'),nvl(comm,0) FROM	 emp;
```

### 2．decode函数

*decode函数可以通过比较进行内容的转换，完成的功能相当于分支语句。
在参数的最后位置上可以存在单独的参数，如果以上比较过程没有找到匹配值，则返回该参数的值，如果不存在该参数，则返回NULL。*

将职务转换成中文显示。
```SQL
SELECT	ename,decode(job, 'MANAGER', '经理', 'CLERK','职员', 'SALESMAN','推销员', 'ANALYST','系统分析员','未知') FROM emp;
```

### 3．最大、最小值函数

*greatest返回参数列表中的最大值，least返回参数列表中的最小值。*

**如果表达式中有NULL，则返回NULL。**















