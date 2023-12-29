---
title: PL/SQL 函数
date: 2023-04-26 13:04:12
tags: 
- Database
- Oracle
- PL/SQL
---
# 掌握函数的创建、修改及删除

## 函数创建的语法格式：

Create [or replace] function <function_name>
(<参数1> [方式1]<数据类型1>,<参数2> [方式2]<数据类型2>…)
Return<数据类型> is|as
Pl/sql程序体 

――其中必须有一个return语句

注：通常函数只有in类型的参数

例1：
```sql
SET SERVEROUTPUT ON FORMAT WRAPPED

CREATE OR REPLACE FUNCTION DEPT_NUM (
  IN_DEPT IN SCOTT.EMP.DEPTNO%TYPE
) RETURN NUMBER AS
  OUT_NUM NUMBER;
BEGIN
  SELECT
    COUNT(*) INTO OUT_NUM
  FROM
    SCOTT.EMP
  WHERE
    SCOTT.EMP.DEPTNO=IN_DEPT;
  RETURN(OUT_NUM);
END DEPT_NUM;
```

## 函数的调用

```sql
DECLARE
  DEPTNUM NUMBER;
BEGIN
  DEPTNUM:=DEPT_NUM(20);
  DBMS_OUTPUT.PUT_LINE('THE NUMBER OF DEPT 20 IS '
    ||TO_CHAR(DEPTNUM));
END;
```

## 函数的删除:

同存储过程

```sql
DROP FUNCTION DEPT_NUM;
```