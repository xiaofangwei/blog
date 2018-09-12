---
title: Mysql高版本连接问题
date: 2017-11-17 21:28:50
category:
   易错集锦
tags: mysql
---

## Java使用mysql-jdbc连接MySQL出现如下警告：

　　Establishing SSL connection without server's identity verification is not recommended. According to MySQL 5.5.45+, 5.6.26+ and 5.7.6+ requirements SSL connection must be established by default if explicit option isn't set. For compliance with existing applications not using SSL the verifyServerCertificate property is set to 'false'. You need either to explicitly disable SSL by setting useSSL=false, or set useSSL=true and provide truststore for server certificate verification.
　　

在mysql连接字符串url中加入ssl=true或者false即可，如下所示。

	　　url=jdbc:mysql://127.0.0.1:3306/framework?characterEncoding=utf8&useSSL=true