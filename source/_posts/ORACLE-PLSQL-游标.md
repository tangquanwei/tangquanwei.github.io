---
title: Oracle PL/SQL 游标
date: 2023-04-25 18:35:46
tags: 
- Database
- Oracle
- PL/SQL
---



## PL/SQL中的游标CURSOR

注意例子中：%TYPE和%ROWTYPE两种特殊变量；

游标是用来处理使用select语句从数据库中检索到的多行记录的工具，借助于游标的功能，数据库应用程序可以对一组记录逐个进行处理，每次处理一行。可以将游标理解为指向select查询结果的指针。

游标分为显式游标和隐式游标，隐式游标无需用户过问，我们主要学习显式游标。

显式游标处理的4个步骤：
  a)声明游标
  b)为查询打开游标
  c)将结果提取到PL/SQL变量中
  d)关闭游标

```sql
DECLARE
 -- [1] 返回值
  EMP SCOTT.EMP%ROWTYPE;
 -- [2] 游标
  CURSOR EMP_CUR IS
    SELECT
      EMPNO,
      SAL
    FROM
      SCOTT.EMP;
BEGIN
 -- [3] open
  OPEN EMP_CUR;
 -- [4] loop
  LOOP
 -- [5] exit condition
    EXIT WHEN NOT EMP_CUR%FOUND;
 -- [6] fetch
    FETCH EMP_CUR INTO EMP;
 -- [7] do sth
  END LOOP;
 -- [8]关闭游标
  CLOSE EMP_CUR;
END;
```

(1)
```sql
DECLARE
  EMP_ID   SCOTT.EMP.EMPNO%TYPE; --变量Emp_id的类型取scott.emp.empno的类型
  EMP_NAME SCOTT.EMP.ENAME%TYPE;
  EMP_JOB  SCOTT.EMP.JOB%TYPE;
  EMP_SAL  SCOTT.EMP.SAL%TYPE;
  CURSOR EMP_CUR IS --声明游标emp_cur
    SELECT
      EMPNO,
      ENAME,
      JOB,
      SAL
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=20
    ORDER BY
      SAL;
BEGIN
  OPEN EMP_CUR; -- 打开游标，实质就是执行游标emp_cur定义中的sql语句
  FETCH EMP_CUR INTO EMP_ID, EMP_NAME, EMP_JOB, EMP_SAL;
 --提取游标，实质就是获取emp_cur所指向的结果集中的当前行
  LOOP
    EXIT WHEN NOT EMP_CUR%FOUND; --如果游标走到结果集的尾部，则结束LOOP
    IF LOWER(EMP_JOB)='MANAGER' THEN
      DBMS_OUTPUT.PUT_LINE('THE MANAGER IS'
        || EMP_NAME);
    ELSE
      DBMS_OUTPUT.PUT_LINE(EMP_ID
        || 'IS'
        ||EMP_NAME
        ||'AND SALARY IS'
        || EMP_SAL);
    END IF;
    FETCH EMP_CUR INTO EMP_ID, EMP_NAME, EMP_JOB, EMP_SAL;
  END LOOP;
  CLOSE EMP_CUR; --关闭游标
END;
/
```

(2) 上面的例子可以修改如下：

```sql
DECLARE
  EMPL SCOTT.EMP%ROWTYPE; --变量empl的类型为复合类型，类似于c语言中的结构体
  CURSOR EMP_CUR IS --声明游标emp_cur
    SELECT
      *
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=20
    ORDER BY
      SAL;
BEGIN
  OPEN EMP_CUR; -- 打开游标，实质就是执行游标emp_cur定义中的sql语句
  FETCH EMP_CUR INTO EMPL;
 --提取游标，实质就是获取emp_cur所指向的结果集中的当前行
  LOOP
    EXIT WHEN NOT EMP_CUR%FOUND; --如果游标走到结果集的尾部，则结束LOOP
    IF LOWER(EMPL.JOB)='MANAGER' THEN
      DBMS_OUTPUT.PUT_LINE('THE MANAGER IS'
        || EMPL.ENAME);
    ELSE
      DBMS_OUTPUT.PUT_LINE(EMPL.EMPNO
        || ' IS '
        ||EMPL.ENAME
        || ' AND SALARY IS '
        || EMPL.SAL);
    END IF;
    FETCH EMP_CUR INTO EMPL;
  END LOOP;
  CLOSE EMP_CUR; --关闭游标
END;
/         --执行上面的语句
```


## 隐式游标的基本操作

隐式游标 ---由Oracle数据库自动创建，名称是(SQL) ,主要用途是可以返回一个操作是否成功或失败.
  (1) 由Oracle在内部声明，由系统管理
  (2) 用于处理
     -DML语句   --注意只能用于DML语句哦。
     -返回单行的查询--如果使用select .. into ，则要求该select查询的结果只包含一条记录
  (3) 用于判断一个操作是否成功.
     SQL%notfound  --返回Boolean值  存在结果集返回 False
     SQL%found    --返回Boolean值   存在结果集返回 True
     SQL%rowcount  --修改涉及到的记录的行数
     SQL%isopen   --在隐式游标里一般这个属性是自动打开和关闭的，且任何时候查询都返回False。

例1：
```sql
SET SERVEROUTPUT ON

DECLARE
  ICOUNT INT:=0;
BEGIN
  INSERT INTO EMP(
    EMPNO,
    ENMAE
  ) VALUES(
    2,
    'jerry'
  );
  DBMS_OUTPUT.PUT_LINE('游标所影响的行数：'
    ||SQL%ROWCOUNT);
  IF SQL%NOTFOUNT THEN
    DBMS_OUTPUT.PUT_LINE('NotFount为真');
  ELSE
    DBMS_OUTPUT.PUT_LINE('NofFount为假');
  END IF;
END;
```
## 显式游标的属性操作

### （1）是否找到游标－%FOUND
  此属性表示当前游标是否指向有效的一行，取值：TRUE FALSE

例2：
```sql
BEGIN
  DELETE FROM EMP
  WHERE
    EMPNO=7934;
  IF SQL%FOUND THEN
    DBMS_OUTPUT.PUT_LINE('DELETE SUCCESS');
  ELSE
    DBMS_OUTPUT.PUT_LINE('DELETE FAIL');
  END IF;
END;
```
说明：该例使用了隐式游标,当然，显式游标也可以使用属性%FOUND

### （2）是否未找到游标－%NOTFOUND

上例等价于：

例3：

```sql
BEGIN
  DELETE FROM EMP
  WHERE
    EMPNO=7934;
  IF SQL%NOTFOUND THEN
    DBMS_OUTPUT.PUT_LINE('delete fail');
  ELSE
    DBMS_OUTPUT.PUT_LINE('delete success');
  END IF;
END;
/
```

### (3) 游标行数－%ROWCOUNT
    此属性记录了用户成功提取数据的行数，也可以了解为游标所在的行数。

例4：

```sql
DECLARE
  EMPL EMP%ROWTYPE;
  CURSOR EMP_CUR IS
    SELECT
      *
    FROM
      EMP;
BEGIN
  OPEN EMP_CUR;
  LOOP
    FETCH EMP_CUR INTO EMPL;
    DBMS_OUTPUT.PUT_LINE(TO_CHAR(EMPL.EMPNO));
    EXIT WHEN EMP_CUR%ROWCOUNT=5 OR EMP_CUR%NOTFOUND;
  END LOOP;
  CLOSE EMP_CUR;
END;
```
(4) 是否打开游标－%ISOPEN

例5：

```sql
DECLARE
  EMPL EMP%ROWTYPE;
  CURSOR EMP_CUR IS
    SELECT
      *
    FROM
      EMP;
BEGIN
  IF EMP_CUR%ISOPEN THEN
    FETCH EMP_CUR INTO EMPL;
    DBMS_OUTPUT.PUT_LINE(TO_CHAR(EMPL.EMPNO));
  ELSE
    DBMS_OUTPUT.PUT_LINE('EMP_CUR IS NOT OPEN');
    OPEN EMP_CUR;
    LOOP
      FETCH EMP_CUR INTO EMPL;
      DBMS_OUTPUT.PUT_LINE(TO_CHAR(EMPL.EMPNO));
      EXIT WHEN EMP_CUR%ROWCOUNT=5;
    END LOOP;
    CLOSE EMP_CUR;
  END IF;
END;
/
```

## 参数化游标

在定义游标时可以带上参数，使得在使用游标时，根据参数不同所选中的结果集也不同，达到动态使用游标的目的。

例6：

首先在SQL/PLUS上输入下面的语句：

```s
Accept dept_id prompt 'please input the deptno:'
```

说明：accept是SQL*PLUS的命令，不是pl/sql语句，类似于c++的cin，该命令可以接受你的键盘输入到dept_id。

例6：
```sql
DECLARE
  DID  EMP.DEPTNO%TYPE:=&DEPT_ID;
  EMPL EMP%ROWTYPE;
  CURSOR EMP_CUR(DEPTID NUMBER)IS
    SELECT
      *
    FROM
      EMP
    WHERE
      DEPTNO=DEPTID;
BEGIN
  OPEN EMP_CUR(DID);
  LOOP
    FETCH EMP_CUR INTO EMPL;
    DBMS_OUTPUT.PUT_LINE(TO_CHAR(EMPL.EMPNO));
    EXIT WHEN (EMP_CUR%ROWCOUNT=3) OR (EMP_CUR%NOTFOUND);
  END LOOP;
  CLOSE EMP_CUR;
END;
```

## 游标变量的使用

### （1）声明游标变量类型

游标变量是一种引用类型，当程序运行时，他们可以指向不同的存储单元（类似于c语言中的指针类型）。如果要使用引用类型，首先要声明该变量，然后相应的存储单元必须要分配。Pl/sql中引用类型通过下述语法进行声明：

  Ref type

其中，type是已经被定义的类型，ref关键字指明：新的类型必须是一个指向type类型的指针。因此，游标变量就是 REF CURSOR（指向游标的指针）。定义一个游标变量的完整类型语法如下：

  Type <新类型名> is ref cursor return<游标的返回类型>

其中，<新类型名>是新定义的游标变量的类型，<游标的返回类型>是指该游标返回的记录类型，这样定义的游标变量类型是受限的，它的返回类型只能是特定的记录类型；
如果没有return<游标的返回类型>部分：

  Type <新类型名> is ref cursor

这样的游标变量类型是非受限的，一个非受限的游标变量可以为任何查询打开。

例1：声明游标变量

```sql
Declare
Type t_deptref is ref cursor
  Return scott.dept%rowtype;   --使用%rowtype定义返回类型
Type manager_rcd is record(
Ename scott.emp.sname%type,
Sal scott.emp.sal%type,
Edept scott.emp.deptno%type); --定义记录类型，类似于c语言中的结构体

Type t_managerref is ref cursor
   Return manager_rcd;        -- 定义返回类型为manager_rcd的游标变量类型
  V_dept t_deptref;           -- 定义t_deptref类型的游标变量
  V_manager t_managerref;     -- 定义t_managerref类型的游标变量
```

### （2）打开游标变量

如果要将一个游标变量于一个特定的select语句相关联，需要使用OPEN FOR语句，其语法是：

  OPEN <游标变量> FOR <select语句>；

例2：打开游标变量

```sql
DECLARE
  TYPE T_DEPTREF IS
    REF CURSOR RETURN SCOTT.DEPT%ROWTYPE;
  V_DEPT   T_DEPTREF;
  ROW_DEPT SCOTT.DEPT%ROWTYPE;
BEGIN
  OPEN V_DEPT FOR
    SELECT
      *
    FROM
      SCOTT.DEPT;
  FETCH V_DEPT INTO ROW_DEPT;
  DBMS_OUTPUT.PUT_LINE(' ' 
    ||TO_CHAR(ROW_DEPT.DEPTNO)
    ||' '
    ||ROW_DEPT.DNAME
    ||' '
    ||ROW_DEPT.LOC);
  LOOP
    EXIT WHEN NOT V_DEPT%FOUND;
    FETCH V_DEPT INTO ROW_DEPT;
    DBMS_OUTPUT.PUT_LINE(' '
      ||TO_CHAR(ROW_DEPT.DEPTNO)
      ||' '
      ||ROW_DEPT.DNAME
      ||' '
      ||ROW_DEPT.LOC);
  END LOOP;
END;
```