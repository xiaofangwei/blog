---
title: Mybatis之CRUD操作
date: 2018-02-03 21:00:13
category: Java
tags: mybatis
---
<h1>Mybatis详解第二弹——编写基于mybatis的操作Emp表的CRUD操作的dao类</h1>

**用xml配置的方式**

1、定义sql映射的xml文件 如：

	id: 是sql语句在xml文件的位置
	parameterType: 需要传递参数的类型
	resultType: sql语句执行后返回的类型 

	<select id="getEmpByName" parameterType="string" reaultType="com.model.Emp" >
	    select * from emp where ename = #{ename};
	</select>

	<insert id="insertEmp" parameterType="com.model.Emp" resultType="int">
	    insert into emp values (#{empno,#{ename},#{job},#{mgr},#{hiredate},#{sql},#{comm},#{deptno})
	</insert>            

	<delete id="deleteByNo" parameterType="int">
	    delete from emp where empno = #{empno}
	<delete>

	<update id="updateByNo" parameterType="com.model.Emp" resultType="int">
	    update emp set ename=#{ename},job=#{job},... where empno=#{empno} 
	</update>
<!--more-->
2、在mybatis配置文件中注册实体对应的SQL映射

	<mappers>
	    <mapper resource="com/mapper/EmpMapper.xml"/>
	</mappers>
3、在dao中使用
```java
	public Emp getEmpByName(String ename) {

	        创建SessionFactory的代码省略

	        SqlSession session = sessionFactory.openSession();
	        User user = session.selectOne("com.mapper.EmpMapper.getEmpByName", ename);
	        return Emp;
	    }
	    .........
```
**用注解的方式**

1、 定义SQL映射的接口
```java
	public interface EmpMapper {

	    @Select("select * from emp where ename = #{ename}")
	    public Emp getEmpByName(String name);

	    @Delete("delete from emp where empno = #{empno}")
	    public int deleteByNo(int empno);

	    @Insert("insert into emp values (#{empno,#{ename},#{job},#{mgr},#{hiredate},#{sql},#{comm},#{deptno})")
	    public int insertEmp(Emp emp);

	    @Update("update emp set ename=#{ename},job=#{job},... where empno=#{empno} ")
	    public int updateBuNo(Emp emp);
	}
```
2、在mybatis配置文件中注册实体对应的SQL映射

	<mappers>
	    <mapper resource="com/mapper/EmpMapper.xml"/>
	</mappers>
	
3、在dao中使用
```java
	public Emp getEmpByName(String ename) {

	        创建SessionFactory的代码省略

	        SqlSession session = sessionFactory.openSession();
	        EmpMapper mapper = session.getMapper(EmpMapper.class);
	        Emp emp = mapper.getEmpByNo
	        return Emp;
	    }
	    .......
```
