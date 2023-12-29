---
title: OracleDB 最佳实践
date: 2022-09-24 16:14:33
tags: 
- Database
- Oracle
- PL/SQL
---

# OracleDB 最佳实践

### 1. 创建表空间、用户

```sql
-- 创建表空间  
CREATE TABLESPACE tang 
	DATAFILE '/u01/app/oracle/oradata/xe/tang.dbf' 
	SIZE 32 M 
	AUTOEXTEND ON 
	NEXT 32 M;
```

<!-- more -->

```sql
-- 新建用户并选择刚创建的表空间 
CREATE USER quanwei  
	IDENTIFIED BY 123456          
		ACCOUNT UNLOCK          
		DEFAULT TABLESPACE tang;
```

```sql
-- connect,resource,dba权限赋予 quanwei用户
GRANT CONNECT,RESOURCE,DBA TO quanwei;

-- 多权限授权
GRANT 
CREATE USER,
DROP USER,
ALTER USER ,
CREATE ANY VIEW ,
DROP ANY VIEW,
EXP_FULL_DATABASE,
IMP_FULL_DATABASE,
DBA,
CONNECT,
RESOURCE,
CREATE SESSION 
TO quanwei;
```

## 2. 常用命令操作

```sql
--首先查询一下用户的profile的类型
SELECT username, profile FROM dba_users;

--查看制定概要文件（默认为DEFAULT）的密码有效期:
SELECT * FROM dba_profiles WHERE profile='DEFAULT' AND resource_name='PASSWORD_LIFE_TIME';

--然后将密码的有效期有180天设置为“无限制”;
ALTER PROFILE DEFAULT LIMIT PASSWORD_LIFE_TIME UNLIMITED;

-- 修改密码
ALTER USER scott IDENTIFIED BY 123456;

-- 查询所有用户
SELECT * FROM ALL_USERS;

-- 创建账户
CREATE USER quanwei IDENTIFIED BY 123456;

--  给用户授予权限
GRANT CONNECT, RESOURCE, DBA TO quawnei;
GRANT CREATE  SESSION  TO  quanwei;

-- 解除锁定
ALTER USER quanwei ACCOUNT UNLOCK;
```

## 3. 主键自增

SQL不区分大小写

```sql
---自增序列
CREATE SEQUENCE jcd_user_seq
    MINVALUE 1
    NOMAXVALUE
    INCREMENT BY 1
    START WITH
        1 NOCACHE ;

--创建触发器
create or replace trigger JCD_USER_ID_TRIGGER
    before insert on JCD_USER
    for each row
begin
    select jcd_user_seq.nextval into: JCD_USER.U_ID from dual;
end jcd_USER_ID_TRIGGER;
```

## 4. 简单的表操作

```sql
-- 创建表
CREATE TABLE person (
	p_id NUMBER(6) PRIMARY KEY,
	p_name varchar(20) NOT NULL,
	p_other NUMBER(6),
	CONSTRAINT p_fk FOREIGN KEY (p_other) REFERENCES person(p_id)
);
 

-- 查询用户的所有表
SELECT * FROM USER_TABLES;

-- 查询表中内容
SELECT * FROM PERSON;

-- 修改表

-- 添加新属性（列）
ALTER TABLE PERSON  
ADD p_birthday date NOT NULL;

-- 修改以有属性的数据类型
ALTER TABLE PERSON 
MODIFY p_birthday timestamp;

-- 将属性设置成unused
ALTER TABLE PERSON 
SET unused COLUMN  p_birthday;

-- 查看
SELECT * FROM PERSON; -- p_birthda这一列不在了

-- 删除指定列
ALTER TABLE PERSON 
DROP COLUMN p_name;

-- 删除unused状态的列
ALTER TABLE PERSON 
DROP unused COLUMN;

-- 重命名属性
ALTER TABLE PERSON 
RENAME COLUMN p_id TO persion_id;

-- 重命名表
ALTER TABLE PERSON 
RENAME TO test;

-- 查看发现原来的表没有了
SELECT * FROM user_tables;

-- 删除表
DROP TABLE test;
```

## 数据库对象命名规范

| 类型 | 前缀 |
|         -|   -|
|表前缀     | t_ |
|视图前缀   | v_ |
|同义词前缀  | s_ |
|簇表前缀    | c_ |
|序列前缀    | seq_ |
|存储过程前缀| p_ |
|函数前缀    | f_ |
|包前缀     | pkg_ |
|类前缀     | tp_ |
|主键前缀    | pk_ |
|外键前缀    | fk_ |
|唯一索引前缀| ux_ |
|普通索引前缀| idx_ |
|位图索引前缀| bx_ |
|函数索引前缀| fx_ |