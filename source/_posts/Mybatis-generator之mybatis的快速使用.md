---
title: Mybatis-generator之mybatis的快速使用
date: 2018-02-04 21:17:05
category: Java
tags: mybatis
---
<h1>mybatis-generator插件自动生成单表的类和映射文件</h1>

1、 下载mybatis-generrator插件包

	mybatis-generator-core-1.3.2

目录结构：

    docs    //说明文档文件夹
    lib        //jar包文件夹
    LICENSE    
    NOTICE
    README.txt
<!--more-->
2、 配置mybatis-generator插件

在lib文件夹下有一个generatorConfig.xml文件用NotePad++打开：

	<generatorConfiguration>  
	<!-- 数据库驱动-->  
	<classPathEntry  location="mysql-connector-java-5.1.25-bin.jar"/>  
	<context id="DB2Tables"  targetRuntime="MyBatis3">  
	    <commentGenerator>  
	        <property name="suppressDate" value="true"/>  
	        <!-- 是否去除自动生成的注释 true：是 ： false:否 -->  
	        <property name="suppressAllComments" value="true"/>  
	    </commentGenerator>  
	    <!--数据库链接URL，用户名、密码 -->  
	    <jdbcConnection driverClass="com.mysql.jdbc.Driver" connectionURL="jdbc:mysql://127.0.0.1:3306/db_novel" userId="root" password="123456">  
	    </jdbcConnection>  
	    <javaTypeResolver>  
	        <property name="forceBigDecimals" value="false"/>  
	    </javaTypeResolver>  
	    <!-- 生成模型的包名和位置-->  
	    <javaModelGenerator targetPackage="novel.spider.entitys" targetProject="src">  
	        <property name="enableSubPackages" value="true"/>  
	        <property name="trimStrings" value="true"/>  
	    </javaModelGenerator>  
	    <!-- 生成映射文件的包名和位置-->  
	    <sqlMapGenerator targetPackage="mapper" targetProject="src">  
	        <property name="enableSubPackages" value="true"/>  
	    </sqlMapGenerator>  
	    <!-- 生成DAO的包名和位置-->  
	    <javaClientGenerator type="XMLMAPPER" targetPackage="novel.spider.mapper" targetProject="src">  
	        <property name="enableSubPackages" value="true"/>  
	    </javaClientGenerator>  
	    <!-- 要生成的表 tableName是数据库中的表名或视图名 domainObjectName是实体类名-->  
	    <table tableName="t_novel" domainObjectName="Novel" enableCountByExample="false" enableUpdateByExample="false" enableDeleteByExample="false" enableSelectByExample="false" selectByExampleQueryId="false"></table>
	</context>  
	</generatorConfiguration> 

按照注释修改自己的信息即可

3、 使用mybatis-generator

在lib文件夹下面按住shift键点击鼠标右键，点击在此处打开命令窗口
输入一下命令：

    java -jar mybatis-generator-core-1.3.2.jar -configFile generatorConfig.xml  -overwrite

生成的文件在lib文件下面的src文件夹中
