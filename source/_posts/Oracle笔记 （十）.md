---
title: Oracle笔记 （十）
date: 2017-11-30 21:10:31
comments: false
tags: oracle
---
# 建表

## 一、建表

格式：

<pre>
create table 表名
      (
	  列名1   类型   约束,
	  列名2   类型   约束,
	  ......			
      );
</pre>

如：

--创建出版社表
```SQL
	create table 出版社（
		编号 varchar2(2),
		出版社名称 varchar2(30),
		地址 varchar2(30),
		联系电话 varchar2(20)
	）;
```
--创建图书表
```SQL
	create table 图书 (
		图书编号 VARCHAR2(5),
		图书名称 VARCHAR2(30),
		出版社编号 VARCHAR2(2),
		作者 VARCHAR2(10),
		出版日期 DATE,
		数量 NUMBER(3),
		单价 NUMBER(7,2)	
	);
```
## 二、通过子查询建表

步骤1：完全复制图书表到“图书1”
```SQL
create table 图书1 as select * from 图书;
```
步骤2：创建新的图书表“图书2”，只包含书名和单价
```SQL
create table 图书2（书名，单价） as seelct 图书名称，单价 from 图书；
```
步骤3：创建新的图书表“图书3”，只包含书名和单价，不复制内容
```SQL
create table 图书3（书名，单价） as select 图书名称，单价 from 图书 where 1=2；
```
## 三、添加表的约束

<pre>
主键     primary key      PK
唯一     unique           UQ
默认值   default          DF
检查约束 check            CK
外键约束 foreign key      FK
</pre>

### 方法一：建表的同时添加约束 

如：

<pre>
create table stuinfo(
 sno int primary key not null,       --主键
 sname varchar2(10) unique not null,       --唯一
 sex char(2) default '男' check(sex='男' or sex = '女') not null,   --默认及检查
 saddress varchar2(50) not null,
 phone char(11),
 email varchar2(50)
);
create table stumarks(
 marksId int,
 sno int references stuinfo(sno) not null,     --外键
 score number(5,1),
 examDate date default sysdate
);
</pre>

### 方法二:建表完成后，再添加约束

如：（之前已建好了出版社表及图书表）


--主键约束
```SQL
alter table 出版社 add constraint PK_编号 primary key (编号);
```
--唯一约束
```SQL
alter table 出版社 add constraint UQ_地址 unique (地址);
```
--检查约束
```SQL
alter table 出版社 add constraint CK_联系电话 check (联系电话 like '1%');
```
--默认值
```SQL
alter table 出版社 modify 地址 default '湘潭';
```
--外键约束
```SQL
alter table 图书 add constraint FK_图书编号 foreign key (图书编号) references 出版社(编号);
```
--外键约束
```SQL
alter table 图书 add constraint FK_图书编号 foreign key (图书编号) references 出版社(编号);
```
## 四、查看约束条件

**数据字典`USER_CONSTRAINTS`中包含了当前模式用户的约束条件信息。其中，CONSTRAINTS_TYPE 显示的约束类型为：**

<pre>		
C：CHECK约束。

P：PRIMARY KEY约束。
​		
U：UNIQUE约束。
​		
R：FOREIGN KEY约束。
</pre>	

*其他信息可根据需要进行查询显示，可用DESCRIBE命令查看`USER_CONSTRAINTS的`结构。*

如:检查表的约束信息：
```SQL
SELECT CONSTRAINT_NAME,CONSTRAINT_TYPE,SEARCHCONDITON
FROM USER_CONSTRAINTS
WHERE TABLE_NAME='图书';
```

## 五、删除约束条件
```SQL
ALTER TABLE 表名 DROP CONSTRAINT 约束名;
```

## 六、表的操作

<pre>
1、删除表    drop table 表名
2、重命名表  RENAME 表名 TO 新表名;
3、查看表
</pre>

*可以通过对数据字典`USER_OBJECTS`的查询，显示当前模式用户的所有表。*
​	
如： 显示当前用户的所有表。
```SQL
SELECT object_name FROM user_objects WHERE object_type='TABLE';
```

## 七、修改表

### 1、增加新列:

如： 为“出版社”增加一列“电子邮件”：
```SQL
ALTER TABLE 出版社		
ADD 电子邮件 VARCHAR2(30) CHECK(电子邮件 LIKE '%@%');
```
### 2、修改列
修改列定义有以下一些特点：

<pre>
(1) 列的宽度可以增加或减小，在表的列没有数据或数据为NULL时才能减小宽度。
(2) 在表的列没有数据或数据为NULL时才能改变数据类型，CHAR和VARCHAR2之间可以随意转换。
(3) 只有当列的值非空时，才能增加约束条件NOT NULL。
(4) 修改列的默认值，只影响以后插入的数据。如：修改“出版社”表“电子邮件”列的宽度为40。
</pre>	

```SQL
ALTER TABLE 出版社 MODIFY 电子邮件 VARCHAR2(40);
```
### 3、删除列
如：删除“出版社”表的“电子邮件”列。
```SQL
ALTER TABLE 出版社 DROP COLUMN 电子邮件;
```