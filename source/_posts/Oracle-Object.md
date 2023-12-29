---
title: Oracle Object
date: 2023-04-09 09:12:56
tags: 
- Database
- Oracle
---

# Oracle Object

## 表

```sql
create table t_<name>;
select * from t_<name>;
drop table t_<name>;
```

### 常用数据类型

**varchar2(n)** 可变长的字符串  

    具体定义时指明最大长度n byte，最大字节数都是4000（UTF8 1333个汉字）

**number(precision,scale)** 可变长的数值列，允许浮点数、正值及负值  

    precision [1,38] 是所有有效数字的位数（从左边第一个不为0的数算起，小数点和负号不计入有效位数）
   scale [-84,127] 是小数点以后的位数，多出来的位数四舍五入

**date 时间日期** 

    包括：世纪、年、月、日、时、分、秒
    sysdate
    TO_CHAR(SYSDATE, 'yyyy-mm-dd hh24:mi:ss')
    TO_DATE('2002-10-09 13:23:44', 'yyyy-mm-dd hh24:mi:ss')

**blob** 二进制数据 用来保存较大的图形文件或带格式的文本文件

**clob** 字符型数据

## 视图

视图以经过定制的方式显示来自一个或多个表的数据
  提供了另外一种级别的表安全性
  隐藏的数据的复杂性
  简化的用户的SQL命令
  隔离基表结构的改变
  通过重命名列，从另一个角度提供数据

```sql
create view v_<name> as
select ...

CREATE [OR REPLACE] [FORCE] VIEW
  view_name [(alias[, alias]...)] 
AS select_statement
[WITH CHECK OPTION]
[WITH READ ONLY];
```
视图上的DML语句有如下限制：
  只能修改一个底层的基表
  如果修改违反了基表的约束条件，则无法更新视图
  如果视图包含连接操作符、DISTINCT 关键字、集合操作符、聚合函数或 GROUP BY 子句，则将无法更新视图
  如果视图包含伪列或表达式，则将无法更新视图



## 同义词

同义词是现有对象的一个别名
  简化SQL语句
  隐藏对象的名称和所有者
  提供对对象的公共访问
  公有同义词 可被所有的数据库用户访问。
  私有同义词 只能在其模式内访问，且不能与当前模式的对象同名

```sql
CREATE SYNONYM emp FOR SCOTT.emp;

CREATE PUBLIC SYNONYM s_emp FOR SCOTT.emp;

CREATE OR REPLACE SYNONYM s_emp FOR SCOTT.emp;

DROP SYNONYM emp; 

DROP PUBLIC SYNONYM s_emp; 
```

## 序列

序列是用于生成唯一、连续序号的对象
序列可以是升序的，也可以是降序的

通过序列的伪列来访问序列的值
  NEXTVAL 返回序列的下一个值
  CURRVAL 返回序列的当前值


```sql
-- 创建
CREATE SEQUENCE seq_emp
 START WITH 10
 INCREMENT BY 10
 MAXVALUE 2000
 MINVALUE 10
 NOCYCLE
 CACHE 10;

-- 使用
INSERT INTO emp (empno, ename, job) 
     VALUES ( seq_emp.NEXTVAL, 'quanwei', 'sale');

SELECT seq_emp.CURRVAL FROM dual;

-- 不能更改序列的START WITH参数
ALTER SEQUENCE seq_emp MAXVALUE 5000 CYCLE;

-- 删除
DROP SEQUENCE seq_emp;
```

相关参数:

**increment by**：序列每次增加的值，负数表示递减，缺省值为1；

**start with**：序列的初始值，缺省值为1；

**maxvalue**：序列可生成的最大值，缺省值为nomaxvalue，即不设置最大值；系统能产生的最大值为$10^{27}$；

**minvalue**：序列可生成的最小值，缺省值为nominvalue；

**cycle**：定义当序列达到最大/小值后是否循环，缺省值为不循环；**nocycle**：不循环；cycle：循环；如果不使用循环达到限制值后继续产生新值就会出错；使用循环达到最大值后的下一个值为1，和start with设置的值无关，递增还是increment by设置的值；

**cache**：定义缓存序列的个数，缺省值为20，**nocache**表示不设置缓存；使用缓存可以提高序列的性能，但数据库出错时会造成数据丢失使序列不连续；



## 索引

索引是Oracle自动维护的一个可选结构
  用以提高 SQL 语句执行的性能
  减少磁盘I/O
  在逻辑上和物理上都独立于表的数据

索引应该在SQL语句的"where"部分涉及的表列(也称谓词)建立

如果表中列的值占该表中行的20%以内，这个表列就可以作为候选索引表列

如果在SQL语句谓词中多个表列被一起连续引用，则应该考虑将这些表列一起放在一个索引内



类型：
  B树索引(默认类型)

  位图索引

  HASH索引

  索引组织表索引

  反转键(reverse key)索引

  基于函数的索引

  分区索引(本地和全局索引)

  位图连接索引

不应该建索引:
  对于那些在查询中很少使用或者参考的列不应该创建索引
  对于那些只有很少数据值的列也不应该增加索引
  对于那些定义为blob数据类型的列不应该增加索引
  当修改性能远远大于检索性能时，不应该创建索引



```sql

-- 创建索引
create index [unique | bitmap] [schema.]<idx_name> 
  on [schema.]<tab_name>(<col1>[,<clo2>,...])
  tablespace <tbs_name>;

-- 查询索引
select t.*
  from dba_indexes t
where t.owner = 'SCOTT'
  and t.table_name = 'STUDENT_INFO';

-- 修改索引名称 idx_si_sname -> idx_si_sname_new
alter index [schema.]<idx_name>  rename to <new_idx_name>;

-- 修改索引为无效
alter index [schema.]<idx_name> unusable;

-- 重建索引
alter index [schema.]<idx_name> rebuild online;

-- 删除索引
drop index [schema.]<idx_name>;
```


## 数据库对象命名规范

| 类型     | 前缀 |
| -------- | ---- |
| 表       | t_   |
| 视图     | v_   |
| 同义词   | s_   |
| 簇表     | c_   |
| 序列     | seq_ |
| 存储过程 | p_   |
| 函数     | f_   |
| 包       | pkg_ |
| 类       | tp_  |
| 主键     | pk_  |
| 外键     | fk_  |
| 普通索引 | x_   |
| 唯一索引 | ux_  |
| 位图索引 | bx_  |
| 函数索引 | fx_  |