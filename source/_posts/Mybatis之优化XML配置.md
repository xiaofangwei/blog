---
title: Mybatis之优化XML配置
date: 2018-02-03 21:04:38
category: Java
tags: mybatis
---
<h1>Mybatis详解第三弹——优化xml配置使用</h1>

** 数据库配置优化 **

原：
```XML
    <environments default="development">
	    <environment id="development">
	            <transactionManager type="JDBC" />
	            <dataSource type="POOLED">
	                <property name="driver" value="oracle.jdbc.driver.OracleDriver" />
	                <property name="url" value="jdbc:oracle:thin:@127.0.0.1:1521:ORCL" />
	                <property name="username" value="scott" />
	                <property name="password" value="tiger" />
	        </dataSource>
	    </environment>
    </environments>
```
 <!--more-->

把对应的数据库配置信息写到一个properties文件中去

*db.propertise*

 ```properties
        driver=oracle.jdbc.driver.OracleDriver
        url=jdbc:oracle:thin:@127.0.0.1:1521:ORCL
        username=scott
        password=tiger
```

改：

```properties
    <properties resource="db.properties" />     
        <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC" />
            <dataSource type="POOLED">
                <property name="driver" value="${driver}" />
                <property name="url" value="${url}" />
                <property name="username" value="${username}" />
                <property name="password" value="${password}" />
            </dataSource>
        </environment>
    </environments>
```

** 为实体类定义别名,简化sql映射xml文件中的引用 **
```xml
	<typeAliases>
	    <typeAlias type="com.model.Emp" alias="_Emp"/>
	</typeAliases>  
```
注意： 将要使用的别名配置写在mybatis配置文件中而不是实体映射文件

添加log4j.jar，打印日志信息

在src目录下以下文件

log4j.properties
```properties
    log4j.rootLogger=DEBUG, Console
    #Console
    log4j.appender.Console=org.apache.log4j.ConsoleAppender
    log4j.appender.Console.layout=org.apache.log4j.PatternLayout
    log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n
    log4j.logger.java.sql.ResultSet=INFO
    log4j.logger.org.apache=INFO
    log4j.logger.java.sql.Connection=DEBUG
    log4j.logger.java.sql.Statement=DEBUG
    log4j.logger.java.sql.PreparedStatement=DEBUG
```
log4j.xml
```xml
    <?xml version="1.0" encoding="UTF-8" ?>
    <!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
    <log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
        <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
            <layout class="org.apache.log4j.PatternLayout">
                <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
            </layout>
        </appender>
        <logger name="java.sql">
            <level value="debug" />
        </logger>
        <logger name="org.apache.ibatis">
            <level value="debug" />
        </logger>
        <root>
            <level value="debug" />
            <appender-ref ref="STDOUT" />
        </root>
    </log4j:configuration>
```
** 解决表的字段名与实体类的属性名不相同的冲突 **

a. 准备表和数据：
```SQL
    CREATE TABLE orders(
        order_id INT PRIMARY KEY AUTO_INCREMENT,
        order_no VARCHAR(20), 
        order_price FLOAT
    );
    INSERT INTO orders(order_no, order_price) VALUES('aaaa', 23);
    INSERT INTO orders(order_no, order_price) VALUES('bbbb', 33);
    INSERT INTO orders(order_no, order_price) VALUES('cccc', 22);
```
b. 定义实体类：
```java
    public class Order {
        private int id;
        private String orderNo;
        private float price;
    }
    ```
c. 实现getOrderById(id)的查询

* c1. 通过在sql语句中定义别名
```xml
    <select id="selectOrder" parameterType="int" resultType="_Order">
        select order_id id, order_no orderNo,order_price price from orders where order_id=#{id}
    </select>
```
* c2. 定义<resultMap>
```xml
    <select id="selectOrderResultMap" parameterType="int" resultMap="orderResultMap">
        select * from orders where order_id=#{id}
    </select>

    <resultMap type="_Order" id="orderResultMap">
        <id property="id" column="order_id"/>
        <result property="orderNo" column="order_no"/>
        <result property="price" column="order_price"/>
    </resultMap>
```
