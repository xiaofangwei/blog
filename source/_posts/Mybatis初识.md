---
title: Mybatis初识
date: 2018-02-03 20:55:54
top: 1
tags: mybatis
---
<h1>Mybatis详解第一弹——初识</h1>

**Mybatis初识**
*
​	MyBatis是支持普通SQL查询，存储过程和高级映射的优秀持久层框架。MyBatis消除
​	了几乎所有的JDBC代码和参数的手工设置以及对结果集的检索。MyBatis可以使用简单的
​	XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java Objects，普通的
​	Java对象）映射成数据库中的记录。
*
Mybatis简单使用

1、引入jar包
*
​	mybatis-3.1.1.jar
​	log4j-1.2.16.jar
​	ojdbc5.jar
*
2、创建数据库和表

为了更简单的创建一个实例，我使用了Oracle的scoot账户，
直接使用emp表作为示例

3、 在示例项目下添加mybatis配置文件 mybatis-conf.xml (命名随意)
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" "http://mybatis.org/dtd/mybatis-3-config.dtd">
	<configuration>

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
	<mappers>
	    <mapper resource="com/mapper/EmpMapper.xml"/>          //在mybatis-cong.xml中注册EmpMapper.xml文件
	</mappers>
	</configuration>
```
4、 创建数据表对应的实体 如：

Emp.java
```java
	    public class Emp {
	        private int empno;
	        private String ename;
	        private String job;
	        private int mgr;
	        private Date hiredate;
	        private float sal;
	        private float comm;
	        private int deptno;

	        //get与set方法省略
	    }
```
5、 创建实体对应的sql映射文件EmpMapper.xml
```xml
	<?xml version="1.0" encoding="UTF-8"?>
	<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" 
	"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

	<mapper namespace="com.mapper.EmpMapper">     //Mappper映射文件所在位置
	    <select id="getEmps" resultType="com.model.Emp">     //xml方式配置sql语句
	        select * from emp
	    </select>
	</mapper>
```
6、 在mybatis-cong.xml中注册EmpMapper.xml文件
```xml
	 <mappers>
	    <mapper resource="com/mapper/EmpMapper.xml"/>
	</mappers>
```
7、 编写测试代码，执行预定义的select语句
```java
	public class EmpMapperTest {
	    public static void main(String[] args) {
	        String resource = "conf.xml"; 
	        //加载mybatis的配置文件（它也加载关联的映射文件）
	        Reader reader = Resources.getResourceAsReader(resource); 
	        //构建sqlSession的工厂
	        SqlSessionFactory sessionFactory = new SqlSessionFactoryBuilder().build(reader);
	        //创建能执行映射文件中sql的sqlSession
	        SqlSession session = sessionFactory.openSession();
	        //映射sql的标识字符串
	        String statement = "com.mapper.EmpMapper.getEmps";
	        List<Emp> list = new ArrayList<Emp>();
	        list = session.selectList(statement);
	        System.out.println(list);
	    }
	}
```
