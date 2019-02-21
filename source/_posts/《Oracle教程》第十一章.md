---
title: 《Oracle教程》第十一章
date: 2017-12-02 17:43:26
tags: oracle
---
# 分区、视图
![](https://github.com/No-Sky/storage/raw/master/images/Logo/OracleLogo1.jpg)

 <!-- more -->

## 一、分区表

**在某些场合会使用非常大的表，比如人口信息统计表。如果一个表很大，就会降低查询的速度，并增加管理的难度。一旦发生磁盘损坏，可能整个表的数据就会丢失，恢复比较困难。根据这一情况，可以创建分区表，把一个大表分成几个区(小段)，对数据的操作和管理都可以针对分区进行，这样就可以提高数据库的运行效率。分区可以存在于不同的表空间上，提高了数据的可用性。例：创建和使用分区表。**
		
如：创建按成绩分区的考生表，共分为3个区：
<pre>		
CREATE TABLE 考生 (
		
	考号 VARCHAR2(5),
		
	姓名 VARCHAR2(30),
		
	成绩 NUMBER(3)
	
	)
PARTITION BY RANGE(成绩)
		
(PARTITION A VALUES LESS THAN (300)
		
TABLESPACE USERS,
		
PARTITION B VALUES LESS THAN (500)
		
TABLESPACE USERS,
		
PARTITION C VALUES LESS THAN (MAXVALUE)
		
TABLESPACE USERS
		
);
</pre>

步骤3：检查A区中的考生：
```SQL
SELECT *  FROM  考生 PARTITION(A);
```	
步骤4：检查全部的考生：
```SQL
SELECT *  FROM  考生;
```

## 二、视图

### 1、视图的概念

 视图不同于表，视图本身不包含任何数据。而视图只是一种定义，对应一个查询语句。视图的数据都来自于某些表，这些表被称为基表。    视图可以在表能够使用的任何地方使用，但在对视图的操作上同表相比有些限制，特别是插入和修改操作。对视图的操作将传递到基表，所以在表上定义的约束条件和触发器在视图上将同样起作用。2、视图的创建
 
### 2、格式：
```SQL
create [or replace] view 视图名 
as
select 语句;
```

例：创建图书作者视图：
```SQL
CREATE VIEW 图书作者(书名,作者) 		
AS SELECT 图书名称,作者 FROM 图书;
```

查询视图全部内容
```SQL
SELECT * FROM 图书作者;    
```

查询部分视图：
```SQL
SELECT 作者 FROM 图书作者;
```
		
删除视图：
```SQL
DROP VIEW 清华图书;
```

### 3．创建只读视图
		
创建只读视图要用`WITH READ ONLY`选项。
		
例：创建emp表的经理视图：
```SQL
CREATE OR REPLACE VIEW manager 
	AS SELECT * FROM emp WHERE job= 'MANAGER'
	WITH READ ONLY;
```

### 4．使用WITH CHECK OPTION选项
		
使用`WITH CHECK OPTION`选项。使用该选项，可以对视图的插入或更新进行限制，即该数据必须满足视图定义中的子查询中的WHERE条件，否则不允许插入或更新。

例：
```SQL
CREATE OR REPLACE VIEW 清华图书 		
	AS SELECT * FROM 图书 WHERE 出版社编号= '01'
```
		
*WITH CHECK OPTION;注：插入数据时，由于带了with check option的选项，则只能插入出版社编为'01'的数据*

### 5．来自基表的限制
		
   除了以上的限制，基表本身的限制和约束也必须要考虑。如果生成子查询的语句是一个分组查询，或查询中出现计算列，这时显然不能对表进行插入。另外，主键和NOT NULL列如果没有出现在视图的子查询中，也不能对视图进行插入。在视图中插入的数据，也必须满足基表的约束条件。

### 6.视图的查看
		
`USER_VIEWS`字典中包含了视图的定义。
		
`USER_UPDATABLE_COLUMNS`字典包含了哪些列可以更新、插入、删除。
		
`USER_OBJECTS`字典中包含了用户的对象。
		
可以通过`DESCRIB`E命令查看字典的其他列信息。

例：查看用户拥有的视图：
```SQL
SELECT object_name FROM user_objects WHERE object_type='VIEW';
```
