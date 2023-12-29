---
title: ORACLE PLSQL 触发器
date: 2023-05-08 17:31:43
tags: 
- Database
- Oracle
- PL/SQL
---

# 触发器

基础知识：

触发器类似于函数和过程，它们都是具有声明部分、执行部分和异常处理部分的命名PL/SQL块。

像包一样，触发器必须在数据库中以独立对象的身份存储。过程是显式地通过过程调用从其他块中执行的.
同时，过程调用可以传递参数。与之相反,触发器是在事件发生时隐式地运行的，并且触发器不能接收参数。
运行触发器的方式叫做激发（firing）触发器，触发事件可以是对数据库表的DML（INSERT、UPDATE或DELETE）操作或某种视图的操作(View)。
或系统事件，如数据库的启动和关闭，以及某种DDL操作。

触发器可以用于下列情况：  
• 维护在表创建阶段通过声明限制无法实现的复杂完整性限制。  
• 通过记录修改内容和修改者来审计表中的信息。  
• 在表内容发生变更时，自动通知其他程序采取相应的处理。  
有三种主要的触发器类型： D M L、替代触发器和系统触发器。


## 创建触发器的通用语法

```sql
CREATE [OR REPLACE ] TRIGGER trigger_name  
{BEFORE | AFTER | INSTEAD OF }  
{INSERT [OR] | UPDATE [OR] | DELETE}  
[OF col_name]  
ON table_name  
[REFERENCING OLD AS o NEW AS n]  
[FOR EACH ROW]  
WHEN (condition)   
DECLARE 
   Declaration-statements 
BEGIN  
   Executable-statements 
EXCEPTION 
   Exception-handling-statements 
END;
```

其中

    CREATE [OR REPLACE] TRIGGER trigger_name - 使用trigger_name创建或替换现有的触发器。
    {BEFORE | AFTER | INSTEAD OF} - 指定何时执行触发器。INSTEAD OF子句用于在视图上创建触发器。
    {INSERT [OR] | UPDATE [OR] | DELETE} - 这指定了DML操作。
    [OF col_name] − 这指定了将要更新的列名称。
    [ON table_name] - 这指定了与触发器关联的表的名称。
    [REFERENCING OLD AS o NEW AS n] - 这允许各种DML语句(如INSERT，UPDATE和DELETE)引用新值和旧值。
    [FOR EACH ROW] - 这指定了一个行级别的触发器，即触发器将被执行的每一行受到影响。否则触发器将在执行SQL语句时执行一次，这称为表级触发器。
    WHEN(condition) - 这为触发器触发的行提供了一个条件。先对该条件求值。触发器主体只有在该条件为真值时才运行。

注意：触发器主体不能超过32K。如果触发器长度超过了该限制，就要把该体内的某些代码放到单独编译的包或存储子程序中，并从触发器主体中调用这些代码。

## DML触发器

DML触发器是由对数据库表进行INSERT、UPDATE、DELETE操作而激发的触发器。该类
触发器可以在上述操作之前或之后激发运行，也可以按每个变更行激发一次（行级触发器），或每个语句激发一次进行（语句级触发器）。这些条件的组合形成了触发器的类型。总共有1 2种可能的触发类型： 3种语句×2种定时×2级。DML触发器只能定义在表上，也就是说只有对表的DML语句可以触发DML触发器。

| 类别    | 值                     | 说明                                                       |
| ------- | ---------------------- | ---------------------------------------------------------- |
| DML语句 | INSERT、DELETE、UPDATE | 定义何种DML语句激发触发器                                  |
| 定时    | AFTER BEFOR            | 定义触发器是在语句运行前或运行后激发                       |
| 级      | 行                     | 对由触发语句变更的每一行激发一次，定义中的FOR EACH ROW子句 |
|         | 语句                   | 该触发器就在语句之前或之后激发一次                         |

例1：行级DML触发器(每执行一行触发一次)：
```sql
set serveroutput on 
```
定义   
(1)
```sql
CREATE OR REPLACE PACKAGE TRIGER_PACKAGE AS
 -- Global counter for use in the triggers
  V_COUNTER NUMBER:=1;
END TRIGER_PACKAGE;
```
(2)
```sql
CREATE OR REPLACE TRIGGER ROW_DML BEFORE
  UPDATE ON SCOTT.EMP FOR EACH ROW
BEGIN
  DBMS_OUTPUT.PUT_LINE('THE '
    ||TO_CHAR(TRIGPACKAGE.V_COUNTER)
    ||' TH ROW UPDATED');
  TRIGPACKAGE.V_COUNTER:= TRIGPACKAGE.V_COUNTER +1;
END ROW_DML;
```
（2）触发
```sql
BEGIN
  UPDATE SCOTT.EMP
  SET
    SAL=SAL＋100
  WHERE
    DEPTNO=10;
END;
```
例2：语句级DML触发器(每执行一条DNL语句触发一次)：  
(1) 定义:
```sql
CREATE OR REPLACE TRIGGER STATEMENT_DML BEFORE
  UPDATE ON SCOTT.EMP
BEGIN
  DBMS_OUTPUT.PUT_LINE('ONLY TRIGGERS 1 TIMES');
END STATEMENT_DML;
```
（2）触发:
```sql
BEGIN
  UPDATE SCOTT.EMP
  SET
    SAL=SAL+1
  WHERE
    DEPTNO=10;
DBMS_OUTPUT.PUT_LINE('20203206222 唐权威');
END;
```
例3：比较before 和after触发器  
(1) 定义before触发器
```sql 
-- 20203206222 唐权威
CREATE OR REPLACE TRIGGER BEFORE_DML AFTER
  INSERT OR UPDATE OR DELETE ON SCOTT.EMP
DECLARE
  CURSOR EMP_CUR IS
    SELECT
      EMPNO,
      SAL
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=10;
  EMP_SAL   SCOTT.EMP.SAL%TYPE;
  EMP_EMPNO SCOTT.EMP.EMPNO%TYPE;
  DML_TYPE  VARCHAR2(10);
BEGIN
  IF INSERTING THEN
    DML_TYPE:='INSERT';
  ELSIF UPDATING THEN
    DML_TYPE:='UPDATE';
  ELSE
    DML_TYPE:='DELETE';
  END IF;
  OPEN EMP_CUR;
  FETCH EMP_CUR INTO EMP_EMPNO, EMP_SAL;
  WHILE NOT EMP_CUR%NOTFOUND LOOP
    DBMS_OUTPUT.PUT_LINE('BEFORE '
      ||DML_TYPE
      ||', THE SAL OF '
      ||TO_CHAR(EMP_EMPNO)
      || ' IS'
      ||TO_CHAR(EMP_SAL));
    FETCH EMP_CUR INTO EMP_EMPNO, EMP_SAL;
  END LOOP;
  CLOSE EMP_CUR;
  DBMS_OUTPUT.PUT_LINE('THEN BEGIN TO INSERT');
END BEFORE_DML;
/
```
函数INSRETING,UPDATING,DELETING.

（2）触发BEFORE触发器
```sql
-- 20203206222 唐权威
DECLARE
  CURSOR SAL_CUR IS
    SELECT
      EMPNO,
      SAL
    FROM
      SCOTT.EMP
    WHERE
      DEPTNO=10;
  EMP_SAL   SCOTT.EMP.SAL%TYPE;
  EMP_EMPNO SCOTT.EMP.EMPNO%TYPE;
BEGIN
  INSERT INTO SCOTT.EMP VALUES(
    8001,
    'JACK',
    'CLERK',
    7788,
    SYSDATE,
    8000,
    1000,
    10
  );
  OPEN SAL_CUR;
  FETCH SAL_CUR INTO EMP_EMPNO, EMP_SAL;
  WHILE SAL_CUR%FOUND LOOP
    DBMS_OUTPUT.PUT_LINE('AFTER INSERT, THE SAL OF '
      ||TO_CHAR(EMP_EMPNO)
      || ' IS'
      ||TO_CHAR(EMP_SAL));
    FETCH SAL_CUR INTO EMP_EMPNO, EMP_SAL;
  END LOOP;
  CLOSE SAL_CUR;
END;
```
或者简单的执行
```sql
INSERT INTO SCOTT.EMP(
  EMPNO,
  ENAME,
  SAL
) VALUES(
  10,
  'JACK',
  1000
);
INSERT INTO SCOTT.EMP(
  EMPNO,
  ENAME,
  SAL,
  DEPTNO
) VALUES(
  8012,
  'JACK',
  1000,
  10
) DELETE FROM EMP WHERE EMPNO IN(
  8000,
  8001
);
-- 20203206222 唐权威
UPDATE SCOTT.EMP
SET
  DEPTNO=50
WHERE
  DEPTNO=10;
```

## instead of触发器（替代触发器）

替代触发器不同于DML触发器的一点是，替代触发器**只能定义在视图**上，而DML触发器即可以定义在视图上，也可定义在表上。  
替代触发器顾名思义，就是不执行满足触发条件的语句，而用替代触发器体种的语句替代。

例1：替代触发器  
（1）创建一个测试视图emp_dept  
```sql
-- 20203206222 唐权威
CREATE OR REPLACE VIEW EMP_DEPT(
  EMPNO,
  DEPTNO,
  DEPTLOC
) AS
  SELECT
    EMPNO,
    D.DEPTNO,
    D.LOC
  FROM
    SCOTT.EMP,
    SCOTT.DEPT D
  WHERE
    EMP.DEPTNO=D.DEPTNO;
```
(2)由于视图emp_dept是由两个表连接而成，因此不能对视图emp_dept进行DML操作，因我们创建一个替代触发器，通过这种手段向emp_dept对应的基本表EMP和DEPT中插入数据。

说明：  
行级触发器是按触发语句所处理的行激发的。在触发器内部，我们可以访问正在处理中的行的数据，这种访问是通过两个相关的标识符： :old和:new实现的。相关标识符是一种特殊的PL/SQL变量，PL/SQL编译器将把这种变量按记录类型处理：

>triggering_table%ROWTYPE   

其中，triggering_table表示触发器所在的表或视图。因此可以通过  
：new.field 或：old.field   
来访问正在处理的行中的字段。

| 触发语句 | :old                       | :new               |
| -------- | -------------------------- | ------------------ |
| INSERT   | 无意义，null               | INSERT语句中的新值 |
| UPDATE   | UPDATE前，表、视图上的原值 | UPDATE语句中的新值 |
| DELETE   | DELETE前，表、视图上的原值 | 无意义，null       |

例如：  
在表emp上定义了一个update触发器，并且在emp中deptno＝50对应的记录为：  
>8003 JACK       CLERK           7788 17-10月-07       8000       1000    50

下面的语句触发了该触发器
```sql

select * from scott.emp;

select * from scott.dept;

insert into scott.dept values (80,'QUANW','AnYue');
-- 20203206222 唐权威
update scott.emp set deptno=80 where deptno=10;
```
那么 
```
:old.deptno=50,:old.enmae=JACK,:old.job=CLERK …；
     :new.empno=null,:new.enmae=null,…… :new.deptno=80,:new.sal=null,…；
```
(2.1)定义instead of触发器
```sql
-- 20203206222 唐权威
CREATE OR REPLACE TRIGGER INSTEAD_DML INSTEAD OF
  INSERT OR UPDATE OR DELETE ON SCOTT.EMP_DEPT FOR EACH ROW
BEGIN
  IF INSERTING THEN
    DBMS_OUTPUT.PUT_LINE(TO_CHAR(:NEW.DEPTNO)
      ||TO_CHAR(:NEW.DEPTLOC));
    INSERT INTO SCOTT.DEPT(
      DEPTNO,
      LOC
    ) VALUES(
      :NEW.DEPTNO,
      :NEW.DEPTLOC
    );
 --Insert into SCOTT.emp(empno,deptno) values(:new.empno,:new.deptno);
  ELSIF UPDATING THEN
    UPDATE SCOTT.DEPT
    SET
      DEPTNO=:NEW.DEPTNO
    WHERE
      DEPTNO=:OLD.DEPTNO;
    UPDATE SCOTT.DEPT
    SET
      LOC=:NEW.DEPTLOC
    WHERE
      DEPTNO=:OLD.DEPTNO;
    UPDATE SCOTT.EMP
    SET
      EMPNO=:NEW.EMPNO
    WHERE
      EMPNO=:OLD.EMPNO
      AND DEPTNO=:OLD.DEPTNO;
    UPDATE SCOTT.EMP
    SET
      DEPTNO=:NEW.DEPTNO
    WHERE
      EMPNO=:OLD.EMPNO
      AND DEPTNO=:OLD.DEPTNO;
  ELSE
    DBMS_OUTPUT.PUT_LINE('DELETE IS IGNORED');
  END IF;
END INSTEAD_DML;
/
```

\DELETE
```sql
select * from all_triggers where TRIGGER_NAME='INSTEAD_DML';

select * from all_views where VIEW_NAME='EMP_DEPT';
```

（2．2）触发instead of触发器

分别执行下面的语句就触发了（2.1）定义的instead of触发器；
```sql
INSERT INTO SCOTT.EMP_DEPT VALUES(8020,90,'BEIJING');

-- 20203206222 唐权威
UPDATE SCOTT.EMP_DEPT
SET
  DEPTLOC='SHANGHAI'
WHERE
  DEPTNO=80;
-- 20203206222 唐权威
DELETE FROM SCOTT.EMP_DEPT
WHERE
  DEPTLOC='SHANGHAI';
```
执行下面的语句，看看变化：
```sql
SELECT * FROM  SCOTT.EMP;
SELECT * FROM  SCOTT.DEPT;
SELECT * FROM  SCOTT.EMP_DEPT;
```
例2：引用子句：

我们可以根据需要使用子句REFERENCING来为:new和:old指定一个不同的名称，
使程序阅读更明了，例如例1（2.1）中创建的instead of触发器，可以改为：
```sql
-- 20203206222 唐权威
CREATE OR REPLACE TRIGGER INSTEAD_DML INSTEAD OF
  INSERT OR UPDATE OR DELETE ON SCOTT.EMP_DEPT
   REFERENCING NEW AS NEW_EMP_DEPT FOR EACH ROW
BEGIN
  IF INSERTING THEN
    DBMS_OUTPUT.PUT_LINE(TO_CHAR(:NEW_EMP_DEPT.DEPTNO)
      ||TO_CHAR(:NEW_EMP_DEPT.DEPTLOC));
    INSERT INTO SCOTT.DEPT(
      DEPTNO,
      LOC
    ) VALUES(
      :NEW_EMP_DEPT.DEPTNO,
      :NEW_EMP_DEPT.DEPTLOC
    );
 --Insert into SCOTT.emp(empno,deptno) values(:NEW_EMP_DEPT.empno,:NEW_EMP_DEPT.deptno);
  ELSIF UPDATING THEN
    UPDATE SCOTT.DEPT
    SET
      DEPTNO=:NEW_EMP_DEPT.DEPTNO
    WHERE
      DEPTNO=:OLD.DEPTNO;
    UPDATE SCOTT.DEPT
    SET
      LOC=:NEW_EMP_DEPT.DEPTLOC
    WHERE
      DEPTNO=:OLD.DEPTNO;
    UPDATE SCOTT.EMP
    SET
      EMPNO=:NEW_EMP_DEPT.EMPNO
    WHERE
      EMPNO=:OLD.EMPNO
      AND DEPTNO=:OLD.DEPTNO;
    UPDATE SCOTT.EMP
    SET
      DEPTNO=:NEW_EMP_DEPT.DEPTNO
    WHERE
      EMPNO=:OLD.EMPNO
      AND DEPTNO=:OLD.DEPTNO;
  ELSE
    DBMS_OUTPUT.PUT_LINE('DELETE IS IGNORED');
  END IF;
END INSTEAD_DML;
```




## 系统触发器

1、DML和替代触发器都在（或代替）DML事件，即INSERT、UPDATE、DELETE语句上激活。而系统触发器可以在两种不同的事件即DDL或数据库事件上激活。DDL事件包括CREATE、ALTER或DROP语句，而数据库事件包括服务器的启动或关闭，用户的登录或退出，以及服务器错误。创建系统触发器的语法如下：
```sql
CREATE [OR REPLACE] TRIGGER [ schema.] trigger_name
{BEFORE | AFTER}
{ ddl_event_list| database_event_list}
ON {DATABASE | SCHEMA}
[ when_clause]
trigger_body;
```
说明:  
关键字 **DATABASE** 和 **SCHEMA** 的区别:  
若用户在创建logon 触发器tt_tirg时,选择了SCHEMA关键字,那么只有用户tt的登陆回触发tt_tirg,其他用户的登陆不会触发tt_tirg;  
若用户tt在创建logon 触发器tt_tirg时,选择了database关键字,那么所有用户的登陆都会触发tt_tirg;

| 事件       | event       | 允许时机      | 说明                             |
| ---------- | ----------- | ------------- | -------------------------------- |
| 启动       | startup     | after         | 实例启动时激活                   |
| 关闭       | shutdown    | before        | 实例正常关闭时激活               |
| 服务器错误 | servererror | after         | 只要有该类错误就激活             |
| 登录       | logon       | after         | 在用户成功连接数据库后激活       |
| 注销       | logoff      | before        | 在用户注销开始时激活             |
| 创建       | create      | before，after | 在创建模式对象之前或之后激活     |
| 撤消       | drop        | before，after | 在创建模式对象撤消之前或之后激活 |
| 变更       | alter       | before，after | 在创建模式对象变更之前或之后激活 |

3.数据库事件触发器

例1：在自己的模式下面创建一个测试用表
```sql
-- 20203206222 唐权威
CREATE TABLE LOGON_RECORD(
  USER_ID VARCHAR2(10),
  LOG_TIME DATE,
  ACTION VARCHAR(20)
);
```

例2：在自己的模式下面创建一个schema级别的logon触发器   

(1)创建  

```sql
-- 20203206222 唐权威
CREATE OR REPLACE TRIGGER LOGON_TRIG 
AFTER LOGON ON SCHEMA
BEGIN
  INSERT INTO LOGON_RECORD VALUES(
    USER,
    SYSDATE,
    'SCHEMA_LOGON'
  );
END;
```

```sql
select * from ALL_TRIGGERS where trigger_name='LOGON_TRIG';
```
(2) 触发

```
conn username/poasswd;
```
并让附近同学执行相应的连接语句  

(3) 查看表logon_record
```sql
select * from logon_record;
```
其他用户的登陆不会记录在表logon_record中;  

例3: 在自己的模式下面创建一个database级别的logon触发器  

(1)创建

```sql
CREATE OR REPLACE TRIGGER LOGON_TRIG_DB 
AFTER LOGON ON DATABASE
BEGIN
  INSERT INTO LOGON_RECORD 
  VALUES(
    USER,
    SYSDATE,
    'DBLOGON'
  );
END;
```


(2)触发
```
conn username/poasswd;
```
并让附近同学执行相应的连接语句
(3)查看表logon_record  
```sql
select * from logon_record;
```
其他用户的登陆会记录在表logon_record中;  
4、DDL触发器  
(1)创建一个测试用表ddl_tri_db  
```sql
CREATE TABLE DDL_TRI_DB(
  USER_ID VARCHAR2(10),
  CTIME DATE,
  ACTION VARCHAR2(20)
);
```
(2)创建DDL触发器
```sql
CREATE OR REPLACE TRIGGER DDL_TRI 
AFTER CREATE ON DATABASE
BEGIN
  INSERT INTO DDL_TRI_DB(
    USER_ID,
    CTIME,
    ACTION
  ) VALUES(
    USER,
    SYSDATE,
    'CREATE'
  );
END DDL_TRI;
```
（3）触发
```sql
CREATE TABLE TT(
  TID NUMBER
);
```
(4) 查看
```sql
select * from quanwei.ddl_tri_db;
```
请将上例中的关键字database 换为schema然后比较变化； 
```sql
CREATE OR REPLACE TRIGGER DDL_TRI 
AFTER CREATE ON SCHEMA
BEGIN
  INSERT INTO DDL_TRI_DB(
    USER_ID,
    CTIME,
    ACTION
  ) VALUES(
    USER,
    SYSDATE,
    'CREATE_S'
  );
END DDL_TRI;
```
（3）触发
```sql
CREATE TABLE TT(
  TID NUMBER
);
```
(4) 查看
```sql
select * from ddl_tri_db;
```
5、when 子句的使用

如果触发器存在when子句，并且在WHEN子句中指定了trigger_condition的话，则首先对该条件求值。触发器主体只有在该条件为真值时才运行。
```sql
set serveroutput on 
```
(1)创建
```sql
CREATE OR REPLACE TRIGGER WHEN_TEST BEFORE
  UPDATE ON SCOTT.EMP FOR EACH ROW WHEN (OLD.DEPTNO=10)
BEGIN
  DBMS_OUTPUT.PUT_LINE('THE UPDATE EXECUTED ONLY WHEN THE DEPTNO=10');
END WHEN_TEST;
```
（2） 触发
```sql
update SCOTT.emp set sal=sal+10000;
```

6、 系统触发器和WHEN子句

就象DML触发器一样，系统触发器可以使用WHEN子句来指定触发器激活条件。
然而，对每一种系统触发器所指定的条件类型有如下限制：  

    STARTUP和SHUTDOWN触发器不能带有任何条件。   
    SERVERERROR触发器可以使用ERRNO测试来检查特定的错误。  
    LOGON 和LOGOFF触发器可以使用USERID或USERNAME测试来检查用户标识或用户名。  
    DDL触发器可以检查正在修改对象的名称和类型。  