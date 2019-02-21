---
title: Mybatis之多表查询
date: 2018-02-03 21:11:27
tags: mybatis
---
<h1>Mybatis详解第三弹——联表查询</h1>

** 一对一 **

1. 创建表和数据：

```sql
	CREATE TABLE teacher(
		t_id INT PRIMARY KEY AUTO_INCREMENT, 
		t_name VARCHAR(20)
	);
	CREATE TABLE class(
		c_id INT PRIMARY KEY AUTO_INCREMENT, 
		c_name VARCHAR(20), 
		teacher_id INT
	);

	ALTER TABLE class ADD CONSTRAINT fk_teacher_id FOREIGN KEY (teacher_id) REFERENCES teacher(t_id);    
	INSERT INTO teacher(t_name) VALUES('LS1');
	INSERT INTO teacher(t_name) VALUES('LS2');
	INSERT INTO class(c_name, teacher_id) VALUES('bj_a', 1);
	INSERT INTO class(c_name, teacher_id) VALUES('bj_b', 2);
```
<!--more-->

2. 定义实体类：

```java
	public class Teacher {
	    private int id;
	    private String name;
	}
	public class Classes {
	    private int id;
	    private String name;
	    private Teacher teacher;
	}
```
c. 定义sql映射文件ClassMapper.xml：

方式一：嵌套查询方式, 通过执行另外一个SQL映射语句来返回预期的复杂类型
```xml
	<select id="getClasses" parameterType="int" resultMap="ClassesResultMap2">
	    select * from class where c_id=#{id}
	</select>

	<resultMap type="CLasses" id="ClassesResultMap2">
	    <id column="c_id" property="id"/>
	    <result column="c_name" property="name"/>
	    <association property="teacher" javaType="Teacher" column="teacher_id" select="getTeacher"></association>
	</resultMap>

	<select id="getTeacher" parameterType="int" resultType="Teacher">
	    select t_id id, t_name name from teacher where t_id=#{id}
	</select>
```
方式二：嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集
```xml
	 <select id="getClasses2" parameterType="int" resultMap="ClassesResultMap">
	        select * from class c,teacher t where c.c_id=#{id} and c.teacher_id=t.t_id; 
	</select>

	<resultMap type="Classes" id="ClassesResultMap">
	    <id column="c_id" property="id"/>
	    <result column="c_name" property="name"/>
	    <association column="teacher_id" property="teacher" javaType="Teacher">
	        <id column="t_id" property="id"/>
	        <result column="t_name" property="name"/>
	    </association>
	</resultMap>
```
d. 测试：
```java
	@Test
	public void testQueryOO() {
	    SqlSession session = factory.openSession();
	    Classes c = sqlSession.selectOne("com.test.CTMapper.getClasses", 1);
	    System.out.println(c);
	}

	@Test
	public void testQueryOO2() {
	    SqlSession session = factory.openSession();
	    Classes c = sqlSession.selectOne("com.test.CTMapper.getClasses2", 1);
	    System.out.println(c);
	}
```
** 一对多 **

1. 创建表：

```SQL	
	CREATE TABLE student(
		s_id INT PRIMARY KEY AUTO_INCREMENT, 
		s_name VARCHAR(20), 
		class_id INT
	);

	INSERT INTO student(s_name, class_id) VALUES('xs_B', 1);
	INSERT INTO student(s_name, class_id) VALUES('xs_D', 1);
	INSERT INTO student(s_name, class_id) VALUES('xs_E', 1);
	INSERT INTO student(s_name, class_id) VALUES('xs_A', 2);
	INSERT INTO student(s_name, class_id) VALUES('xs_H', 2);
	INSERT INTO student(s_name, class_id) VALUES('xs_J', 2);
```
2. 定义实体类：

```java	
	public class Student {
	    private int id;
	    private String name;
	}

	public class Classes {
	    private int id;
	    private String name;
	    private Teacher teacher;
	    private List<Student> students;
	}
```
3. 定义sql映射文件ClassMapper.xml：(根据classId查询对应的班级信息,包括学生)

集合的嵌套结果：使用嵌套结果映射来处理重复的联合结果的子集
```xml
	 <select id="getClasses3" parameterType="int" resultMap="ClassesResultMap3">
	        select * from class c,teacher t, student s where c.c_id=#{id} and c.teacher_id=t.t_id and s.class_id=c.c_id 
	</select>

	<resultMap type="Classes" id="ClassesResultMap3">
	    <id column="c_id" property="id"/>
	    <result column="c_name" property="name"/>
	    <association column="teacher_id" property="teacher" javaType="Teacher">
	        <id column="t_id" property="id"/>
	        <result column="t_name" property="name"/>
	    </association>
	    <collection property="students" ofType="Student" javaType="ArrayList">
	        <id property="id" column="s_id" />
	        <result property="name" column="s_name"/>
	    </collection>
	</resultMap>
```
集合的嵌套查询方式, 通过执行另外一个SQL映射语句来返回预期的复杂类型
```xml
	<select id="getClasses4" parameterType="int" resultMap="ClassesResultMap4">
	        select * from class c where c.c_id=#{id}
	</select>
	<resultMap type="CLasses" id="ClassesResultMap4">
	    <id column="c_id" property="id"/>
	    <result column="c_name" property="name"/>
	    <association property="teacher" javaType="Teacher" column="teacher_id" select="getTeacher"></association>
	    <collection property="students" ofType="Teacher" column="c_id" select="getStudentsSelect" ></collection>
	</resultMap>

	<select id="getTeacher" parameterType="int" resultType="Teacher">
	    select t_id id, t_name name from teacher where t_id=#{id}
	</select>
	<select id="getStudentsSelect" parameterType="int" resultType="Student" >
	    select s_id id, s_name name from student where class_id=#{id}
	</select>
```
4. 测试：
```java
	@Test
	public void testQueryOT() {
	    SqlSession session = factory.openSession();
	    Classes classes = openSession.selectOne("com.test.OMMapper.getClasses3", 1);
	    System.out.println(c);
	}

	@Test
	public void testQueryOT2() {
	    SqlSession session = factory.openSession();
	    Classes classes = openSession.selectOne("com.test.OMMapper.getClasses4", 1);
	    System.out.println(c);
	}
```