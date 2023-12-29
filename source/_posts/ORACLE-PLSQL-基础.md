---
title: PL/SQL 基础
date: 2023-04-12 13:04:12
tags: 
- Database
- Oracle
- PL/SQL
---

# PL/SQL 基础

## PL/SQL的基本结构：

PL/SQL是一种块结构语言，其块结构如下:

```sql
  DECLARE
       ――声明部分:声明变量、常量、用户定义的数据类型以及游标
       ――这一部分可选，不需要可不写
  BEGIN
       ――主程序体，在这可以加入各种合法的语句
  EXCEPTION
       ――异常处理程序，当程序中出现错误时执行这一部分
       ――这一部分可选
  END; ――主程序体结束，分号必不可少；

   从上面的结构可以看出，PL/SQL块由三部分组成：声明部分、执行部分和异常处理部分，其中只有执行部分是必须的：
  BEGIN
    /*执行部分*/   
  END;
```

```sql
DECLARE
    /*声明部分*/
 BEGIN
    /*执行部分*/  
 END; 
```
都是合法的。


## PL/SQL中的数据类型

### (1) 标量

NUMBER--用于存储和操纵数字数据 ,格式是NUMBER（p,s）,p是数据宽度，s是小数点后的位数，默认为0；

CHARACTER――

  CHAR :字符型，最长2000b

  NCHAR：依赖所使用的语种的字符集，最长2000b，与CHAR类似；

  VARCHAR2：用来存放变长字符串，但要指定最大长度，宽度范围1-4000；

  NVARCHAR2：类似于VARCHAR2，依赖于所使用语种的字符集

  VARCHAR：在当前的版本中，与VARCHAR2同义，但以后的版本中将实现真正的变长，不受宽度限制。

DATE

   世纪、年、月、日、时、分、秒，总长14个字节

| Format                        | Result                         |
| ----------------------------- | ------------------------------ |
| YYYY-MM-DD                    | 2015-06-15                     |
| YYYY-MON-DD                   | 2015-JUN-15                    |
| YYYY-MM-DD HH24:MI:SS FF3     | 2015-06-15 13:18:10 700        |
| YYYY-MM-DD HH24:MI:SS FF3 TZR | 2015-06-15 13:18:10 700 +08:00 |
| DS                            | 6/15/2015                      |
| DL                            | Monday, June 15, 2015          |
| TS                            | 1:18:10 PM                     |

TO_DATE('12/1/1985', 'DS')


BOOLEAN―― TRUE、FALSE、NULL

### （2）复合
RECORD

VARRAY

NESTED TABLE

### （3）引用

REF CURSOR 

REF操作符 

### （4）LOB
BLOB

CLOB

NCLOB

BFILE 

## 3、PL/SQL的控制结构举例

### （1）IF语句
```sql
SET SERVEROUTPUT ON

DECLARE
  V_NUM NUMBER(3);
BEGIN
  SELECT
    COUNT(*) INTO V_NUM
  FROM
    SCOTT.EMP
  WHERE
    SAL>2500;
  IF V_NUM<5 THEN
    DBMS_OUTPUT.PUT_LINE('TOTAL MANAGER NUM '
      ||TO_CHAR(V_NUM));
  ELSIF V_NUM<5 DBMS_OUTPUT.PUT_LINE('TOTAL MANAGER NUM 2'
    ||TO_CHAR(V_NUM));
ELSE
  DBMS_OUTPUT.PUT_LINE('TOTAL MANAGER NUM 3'
    ||TO_CHAR(V_NUM));
END IF;
END;
```
### （2）loop语句
```sql
DECLARE
  N      NUMBER:=1;
  COUNT1 NUMBER:=2;
BEGIN
  LOOP
    N:=N*COUNT1;
    COUNT1:=COUNT1+1;
    IF COUNT1>10 THEN
      EXIT;
    END IF;
  END LOOP;
  DBMS_OUTPUT.PUT_LINE(TO_CHAR(N));
END;
/  
```

### （3）FOR 语句
```sql
DECLARE
  V_LOWER NUMBER :=1;
  V_UPPER NUMBER :=10;
BEGIN
  FOR I IN V_LOWER .. V_UPPER LOOP
    DBMS_OUTPUT.PUT_LINE('i is: '
      || I);
  END LOOP;
END;
```

### （4）WHILE 语句

```sql
DECLARE
  N NUMBER:=0;
BEGIN
  WHILE N<100 LOOP
    N:=N+1;
    DBMS_OUTPUT.PUT_LINE('n='
      ||N);
  END LOOP;
END;
```