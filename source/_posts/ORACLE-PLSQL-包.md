---
title: PL/SQL 包
date: 2023-05-1 13:04:12
tags: 
- Database
- Oracle
- PL/SQL
---

## 包的创建及使用

包类似于面向对象中的类，是数据库中的一个实体，其中包含一系列公共常量、变量、数据类型、游标、过程以及函数的定义。

## 创建包

包由包的描述部分和包体两部分组成，包描述部分相当于一个包头，他对包的所有部件进行一个简单的声明，这些部件可以被外界应用程序访问，包描述部分格式如下：
```sql
CREATE PACKAGE <包名> IS
  变量、常量及数据类型的定义； 游标定义头部； 函数、过程的说明；
END<包名>； 
```

### 包头部分：

```sql
CREATE PACKAGE MY_PACKAGE IS DEPT_NUM NUMBER;
 --Cursor manager_cur;
 FUNCTION F_DEPT_NUM(
  IN_DEPTNO IN SCOTT.EMP.DEPTNO%TYPE
) RETURN NUMBER;
PROCEDURE P_DEPT_NUM(
  IN_DEPTNO IN SCOTT.EMP.DEPTNO%TYPE,
  OUT_NUM OUT NUMBER
);
END MY_PACKAGE;
```

### 包体部分：

包体部分是包的描述部分中游标、函数及过程的定义，格式如下：

```sql
Create package body<包名>
As
游标、函数、过程的具体意义；
End<包名>；
```

包体部分：

```sql
CREATE OR REPLACE PACKAGE BODY MY_PACKAGE AS
  FUNCTION F_DEPT_NUM(
    IN_DEPTNO IN SCOTT.EMP.DEPTNO%TYPE
  ) RETURN NUMBER AS
    OUT_NUM NUMBER;
  BEGIN
    SELECT
      COUNT(*) INTO OUT_NUM
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=IN_DEPTNO;
    RETURN OUT_NUM;
  END F_DEPT_NUM;
  PROCEDURE P_DEPT_NUM(
    IN_DEPTNO IN SCOTT.EMP.DEPTNO%TYPE,
    OUT_NUM OUT NUMBER
  ) AS
  BEGIN
    SELECT
      COUNT(*) INTO OUT_NUM
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=IN_DEPTNO;
  END P_DEPT_NUM;
END MY_PACKAGE;
```

## 调用包

包调用的方式为：
包名.变量名
包名.游标名
包名.函数名（过程名）
一旦包创建之后，便可以随时调用其中的内容。

```sql
SET SERVEROUTPUT ON FORMAT WRAPPED

DECLARE
  NUM NUMBER;
BEGIN
  NUM:=MY_PACKAGE.F_DEPT_NUM(20);
  DBMS_OUTPUT.PUT_LINE('THE NUM OF DEPT 20 IS '
    || TO_CHAR(NUM));
  MY_PACKAGE.P_DEPT_NUM(10, NUM);
  DBMS_OUTPUT.PUT_LINE('THE NUM OF DEPT 10 IS '
    ||TO_CHAR(NUM));
END;
```

## 包中游标的使用

### 首先定义一个包头，存放游标变量类型

```sql
CREATE OR REPLACE PACKAGE SELECT_TABLE IS
  TYPE MANAGER_RCD IS
    RECORD (ENAME SCOTT.EMP.ENAME%TYPE, SAL SCOTT.EMP.SAL%TYPE, EDEPT SCOTT.EMP.DEPTNO%TYPE ); -- 定义一个记录类型
  TYPE T_MANAGERREF IS
    REF CURSOR RETURN MANAGER_RCD; --定义一个游标变量类型
END SELECT_TABLE; --定义一个包用来存放自定义的类型
```

例2：创建一个完整的包 :包头和包体 

### 包头 

```sql
CREATE OR REPLACE PACKAGE 
  TEST_PACKAGE IS
    FUNCTION AVGSAL(
      IN_DEPTNO IN NUMBER
    ) RETURN NUMBER; --声明函数
  PROCEDURE MANAGER_INF(
    MANAGER_CUR OUT SELECT_TABLE.T_MANAGERREF
  ); --声明过程
END TEST_PACKAGE;
```

### 包体

```sql
CREATE OR REPLACE PACKAGE BODY TEST_PACKAGE IS
  FUNCTION AVGSAL(
    IN_DEPTNO IN NUMBER
  ) RETURN NUMBER AS
    AVG_SAL NUMBER;
  BEGIN
    SELECT
      AVG(SAL) INTO AVG_SAL
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=IN_DEPTNO;
    RETURN AVG_SAL;
  END AVGSAL; --定义函数
  PROCEDURE MANAGER_INF(
    MANAGER_CUR OUT SELECT_TABLE.T_MANAGERREF
  ) AS --过程manager_inf带有一个out类型的参数manager_cur，该参数是一个类型为select_table.t_managerref的游标变量
  BEGIN
    OPEN MANAGER_CUR FOR
      SELECT
        ENAME,
        SAL,
        DEPTNO
      FROM
        SCOTT.EMP
      WHERE
        LOWER(JOB)='MANAGER'; --打开游标变量
  END MANAGER_INF;
END TEST_PACKAGE;
```
### 调用包

```sql
DECLARE
  AVGNUM     NUMBER;
  MAN_CUR    SELECT_TABLE.T_MANAGERREF;
  EMP_NAME   SCOTT.EMP.ENAME%TYPE;
  EMP_SAL    SCOTT.EMP.SAL%TYPE;
  EMP_DEPTNO SCOTT.EMP.DEPTNO%TYPE;
BEGIN
  AVGNUM:= TEST_PACKAGE.AVGSAL(30); --调用包test_package的avgsal函数
  DBMS_OUTPUT.PUT_LINE('AVGSAL IS '
    ||TO_CHAR(AVGNUM));
  TEST_PACKAGE.MANAGER_INF(MAN_CUR);
 --调用包test_package的manager_inf过程，游标变量Man_cur接受来自过程manager_inf的out参数manager_cur的值
  FETCH MAN_CUR INTO EMP_NAME, EMP_SAL, EMP_DEPTNO;
  DBMS_OUTPUT.PUT_LINE(' '
    || EMP_NAME
    ||' '
    || TO_CHAR(EMP_SAL)
    ||' '
    || TO_CHAR(EMP_DEPTNO));
  LOOP
    EXIT WHEN NOT MAN_CUR%FOUND;
    FETCH MAN_CUR INTO EMP_NAME, EMP_SAL, EMP_DEPTNO;
    DBMS_OUTPUT.PUT_LINE(' '
      || EMP_NAME
      ||' '
      || TO_CHAR(EMP_SAL)
      ||' '
      || TO_CHAR(EMP_DEPTNO));
  END LOOP;
END;
```