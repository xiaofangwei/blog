---
title: Oracle笔记 （二）
date: 2017-11-22 13:23:45
top: 1
tags: oracle
---
# 使用Oracl创建表空间、用户
![](https://github.com/No-Sky/storage/raw/master/images/Logo/OracleLogo1.jpg)

 <!-- more -->

## 一、 创建表空间

（在SqlServer中称为创建一个是数据库，而在Oracle中则称为创建一个表空间）

*格式： 	create tablespace 表空间名 datafile '文件路径' size 文件大小*

如：
```SQL
cerate tablespace myspace datafile 'D:\myspace.dbf' size10MB;
```

删除表空间：
```SQL
drop tablespace myspace incluiding contents and datafile;
```

## 二、创建用户

*格式： create user 用户名 identified by 密码 default tablespace 默认表空间*

如：
```SQL
create user user1 identified by user1 default tablespace system;
```

**删除用户：**
```SQL
drop user user1 cascade;
```

## 三、给用户授权

*方式一：授予角色*
```
connect     //登录
resource    //普通权限，用于操作
DBA         //管理员权限（慎用）
```
如：

```SQL
grant connect to user1;
grant connect,resource to user1;
```

*方式二：授予单个权限*

如：
```SQL
grant create table to user1;           //授予user1建表的权限
grant drop table to user1;             //授予user1删表的权限
```

*方式三：将某个对象的权限授予用户*

如：
```SQL
grant select on scott.emp to user1;      //将scott用户的emp表的查询权限授予user1
grant all on scott.emp to user1;       //将scott用户的emp表的所有权限授予user1 
```

**收回权限：**

*格式： revoke 权限 from 用户*

如：
```SQL
revoke connect from user1;   //收回user1的connect权限
revoke select on scott.emp from user1;    //收回user1对emp表的查询权限  
```