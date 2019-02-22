---
title: Oracle笔记 （十四）
date: 2017-12-12 19:38:50
tags: oracle
---
# 游标、存储过程
![](https://github.com/No-Sky/storage/raw/master/images/Logo/OracleLogo1.jpg)
 <!-- more -->
##一、游标参数的传递
例：
```SQL
SET SERVEROUTPUT ON
DECLARE
    V_empno NUMBER(5);
	V_ename VARCHAR2(10);
	CURSOR 	emp_cursor(p_deptno NUMBER,p_job VARCHAR2) IS SELECT empno,ename FROM emp WHERE	deptno = p_deptno AND job = p_job;
BEGIN
 	OPEN emp_cursor(10, 'CLERK');
  	LOOP
  	   FETCH emp_cursor INTO v_empno,v_ename;
  	EXIT WHEN emp_cursor%NOTFOUND;
  	   DBMS_OUTPUT.PUT_LINE(v_empno||','||v_ename);
	END LOOP;
END; 
```
## 二、异常处理

错误处理的语法如下：
```SQL
EXCEPTION
WHEN 错误1[OR 错误2] THEN 语句序列1;
WHEN 错误3[OR 错误4] THEN 语句序列2;
...
WHEN OTHERS 语句序列n;
  END;
```
例：
```SQL
SET SERVEROUTPUT ON
DECLARE
    v_name VARCHAR2(10);
BEGIN
    SELECT	ename  INTO v_name  FROM emp  WHERE	empno = 1234;
DBMS_OUTPUT.PUT_LINE('该雇员名字为：'|| v_name);
EXCEPTION
		WHEN NO_DATA_FOUND THEN	DBMS_OUTPUT.PUT_LINE('编号错误，没有找到相应雇员！');
		WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('发生其他错误！');
END;
```
## 三、存储过程

### 创建和删除存储过程

格式： 
```SQL
 CREATE [OR REPLACE] PROCEDURE 存储过程名[(参数[IN|OUT|IN OUT] 数据类型...)]
{AS|IS}
	[说明部分]   --定义需要使用的临时变量
BEGIN
	语句集;
	[EXCEPTION]
	    [错误处理部分]
END [过程名];
```
删除：  
```SQL
drop procedure 存储过程名;
```
### 调用存储过程
方法1：   
```SQL
EXECUTE 模式名.存储过程名[(参数...)];   (适用于命今行窗口及sql窗口)
```
方法2： (适用于sql窗口)
```SQL
BEGIN
  模式名.存储过程名[(参数...)];
END;
```
例：编写显示雇员信息的存储过程EMP_LIST，并引用EMP_COUNT存储过程(无参存储过程 )。
```SQL
CREATE OR REPLACE PROCEDURE EMP_LIST
AS
   CURSOR emp_cursor IS SELECT empno,ename FROM emp;
BEGIN
	FOR Emp_record IN emp_cursor LOOP   
			DBMS_OUTPUT.PUT_LINE(Emp_record.empno||Emp_record.ename);
	END LOOP;
	EMP_COUNT;
END;
```
调用：
```SQL
begin
	EMP_LIST;
end;
```		
### 参数传递
a.输入参数: 参数名  IN 数据类型 DEFAULT 值；

例：编写给雇员增加工资的存储过程CHANGE_SALARY，通过IN类型的参数传递要增加工资的雇员编号和增加的工资额。
```SQL
CREATE OR REPLACE PROCEDURE CHANGE_SALARY(P_EMPNO IN NUMBER DEFAULT 7788,P_RAISE NUMBER DEFAULT 10)  --形参P_EMPNO及P_RAISE
AS
   V_ENAME VARCHAR2(10);
   V_SAL NUMBER(5);
BEGIN
	   SELECT ENAME,SAL INTO V_ENAME,V_SAL FROM EMP WHERE EMPNO=P_EMPNO;
   UPDATE EMP SET SAL=SAL+P_RAISE WHERE EMPNO=P_EMPNO;
   DBMS_OUTPUT.PUT_LINE('雇员'||V_ENAME||'的工资被改为'||TO_CHAR(V_SAL+P_RAISE));
   COMMIT;
EXCEPTION
  WHEN OTHERS THEN DBMS_OUTPUT.PUT_LINE('发生错误，修改失败！');
  ROLLBACK; --如果出了异常则撤消
END;
```
调用：
```SQL
begin
	CHANGE_SALARY(7788,80)
end;
```
 b.输出参数: 参数名 OUT 数据类型 DEFAULT  值；

--例：统计雇员的人数
```SQL
CREATE OR REPLACE PROCEDURE EMP_COUNT(P_TOTAL OUT NUMBER)   --P_TOTAL为输出参数
AS
BEGIN
	SELECT COUNT(*) INTO P_TOTAL FROM EMP;
END;
```
调用：
```SQL
DECLARE
V_EMPCOUNT NUMBER;   --定义变量接收过程求出的结果
BEGIN
	EMP_COUNT(V_EMPCOUNT);
	DBMS_OUTPUT.PUT_LINE('雇员总人数为：'||V_EMPCOUNT);
END;
 ``` 
 c.输入输出参数： 参数名  IN OUT   数据类型   DEFAULT   值；

--例：使用IN OUT类型的参数，给电话号码增加区码。
```SQL
CREATE OR REPLACE PROCEDURE ADD_REGION(P_HPONE_NUM IN OUT VARCHAR2)
AS
BEGIN
   P_HPONE_NUM:='024-'||P_HPONE_NUM;
END;
```
调用： 
```SQL
DECLARE
	V_PHONE_NUM VARCHAR2(15);
BEGIN
	V_PHONE_NUM:='26731092';
	ADD_REGION(V_PHONE_NUM);
	DBMS_OUTPUT.PUT_LINE('新的电话号码：'||V_PHONE_NUM);
END;
```