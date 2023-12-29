---
title: PL/SQL 存储过程
date: 2023-04-26 13:04:12
tags: 
- Database
- Oracle
- PL/SQL
---

### PL/SQL 存储过程

## 存储过程的创建及使用

首先：set serveroutput on format wrapped

说明：

In 参数类型：表示这个参数值输入给过程，供过程使用；

Out参数类型：表示该参数是输出类型的参数，表示这个参数在过程中被赋值，可以传给过程体外的环境；

In out参数类型：这种类型的参数实际上就是综合了上述两种参数类型，及向过程体传值，也向过程体外的环境传值；

例1：in /out 参数的使用

```sql
CREATE OR REPLACE PROCEDURE DEPT_MEMBER_NUM (
  IN_DEPTNO IN SCOTT.EMP.DEPTNO%TYPE,
  OUT_TOTAL_NUM OUT NUMBER
) AS
BEGIN
  SELECT
    COUNT(*) INTO OUT_TOTAL_NUM
  FROM
    SCOTT.EMP
  WHERE
    DEPTNO=IN_DEPTNO;
END DEPT_MEMBER_NUM;
```

调用存储过程:

```sql
DECLARE
  RECIVE_OUTPARA NUMBER;
BEGIN
  DEPT_MEMBER_NUM(10, RECIVE_OUTPARA);
  DBMS_OUTPUT.PUT_LINE('THE NUMBER OF DEPT 10 IS '
    ||TO_CHAR(RECIVE_OUTPARA));
END;
```

例2：in out参数的使用

```sql
CREATE OR REPLACE PROCEDURE DEPT_MEMBER_NUM (
  INOUT_PARA IN OUT SCOTT.EMP.DEPTNO%TYPE
) AS
BEGIN
  SELECT
    COUNT(*) INTO INOUT_PARA
  FROM
    SCOTT.EMP
  WHERE
    DEPTNO=INOUT_PARA;
END DEPT_MEMBER_NUM;
```

调用存储过程:

```sql
DECLARE
  RECIVE_INOUTPARA NUMBER :=10;
BEGIN
  DEPT_MEMBER_NUM(RECIVE_INOUTPARA);
  DBMS_OUTPUT.PUT_LINE('THE NUMBER OF DEPT 10 IS '
    ||TO_CHAR(RECIVE_INOUTPARA));
END;
```

## 存储过程的修改及删除：

上面的存储过程使用的Create or replace procedure关键字可以用create procedure代替，事实上create procedure表示创建存储过程，而Create or replace procedure表示创建或替代已经存在的存储过程，通常使用Create or replace procedure替代原来的存储过程来表示修改存储过程；

删除存储过程：

```sql
DROP PROCEDURE DEPT_MEMBER_NUM；
```





