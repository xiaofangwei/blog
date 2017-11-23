---
title: JSP前端与后台的时间处理
date: 2017-11-22 13:44:32
categories: JSP
tags: jsp
---
# JSP中的时间处理方法
![JSPLogo](https://github.com/No-Sky/storage/raw/master/pic/JSPLogo.jpg)
                                                                 <!-- more -->

首先说一下问题，在JAVA中关于时间主要的是`java.util.Date`类与`java.sql.Date`,如果你用任何一个类获取时间如何存进数据库，再把时间拿出来，你得到的只能是像`2017-11-22`,而没有了后面的时分秒，这显然是不合理的。因为`java.sql.Date`类只提供到年月日的解析，所以我们需要另辟他径。

这里我们介绍数据库的另一个时间类型`Timestamp`时间戳类型，它具体是什么就不一一解释了，我们只需要它的可以完美解决时间问题的功能。

*我的上一篇文章所获得的时间格式就是为它而准备的*

	Timestamp ts = Timestamp.valueOf(time);   //time为前端获取的时间，必须满足"yyyy-MM-DD HH:mm:ss"格式，不然就会报解析错误

然后可以用Timestamp格式直接存进数据库，去除的时候使用`rs.getTimestamp()`就行了

显示到前端使用一下代码：

		SimpleDateFormat sdf = new SimpleDateFormat("yyyy-MM-dd HH:mm" );
		String s = sdf.format(createtime);
*格式可以随便定义，把它当普通的时间格式就行了*