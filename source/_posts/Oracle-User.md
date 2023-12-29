---
title: Oracle User
date: 2023-04-12 11:39:53
tags: 
- Database
- Oracle
- PL/SQL
---

## 创建用户
```sql
CREATE USER user
	IDENTIFIED {BY password | EXTERNALLY}
	[ DEFAULT TABLESPACE tablespace ]
	[ TEMPORARY TABLESPACE tablespace ]
	[ QUOTA {integer [K | M ] | UNLIMITED } ON tablespace
	[ QUOTA {integer [K | M ] | UNLIMITED } ON tablespace 	]...]
	[ PASSWORD EXPIRE ]
	[ ACCOUNT { LOCK | UNLOCK }]
	[ PROFILE { profile | DEFAULT }]
```

修该密码

alter  user   scott  identified by "tiger";


## 删除用户

```sql
DROP USER user [CASCADE]
```
CASCADE 同时删除表

## 查看配额

DBA_USERS
DBA_TS_QUOTAS


## 调整配额

```sql
ALTER USER user
	[ DEFAULT TABLESPACE tablespace]
	[ TEMPORARY TABLESPACE tablespace]
	[ QUOTA {integer [K | M] | UNLIMITED } ON tablespace
	[ QUOTA {integer [K | M] | UNLIMITED } ON tablespace ] 	...] 
```

```sql
SELECT
  USERNAME,
  DEFAULT_TABLESPACE
FROM
  DBA_USERS;
```



## 操作系统身份认证(Operating System Authentication)

```sql
CREATE USER OPS$QUANWEI IDENTIFIED EXTERNALLY;

GRANT SESSION TO OPS$QUANWEI;
```

```bash
usermod -a -G dba quanwei

su quanwei

sqlplus /

sqlplus / sysdba
```

## 用户权限

### System Privileges

| Category   | Examples                                                   |
| ---------- | ---------------------------------------------------------- |
| INDEX      | CREATE ALTER DROP ANY INDEX                                |
| TABLE      | CREATE ALTER ANY TABLE DROP SELECT UPDATE DELETE ANY TABLE |
| SESSION    | CREATE ALTER RESTRICTED SESSION                            |
| TABLESPACE | CREATE ALTER DROP UNLIMITED TABLESPACE                     |

### SYSDBA and SYSOPER Privileges

| Category | Examples                             |
| -------- | ------------------------------------ |
| SYSOPER  | STARTUP                              |
|          | SHUTDOWN                             |
|          | ALTER DATABASE OPEN  MOUNT           |
|          | ALTER DATABASE BACKUP CONTROLFILE TO |
|          | RECOVER DATABASE                     |
|          | ALTER DATABASE ARCHIVELOG            |
| SYSDBA   | SYSOPER PRIVILEGES WITH ADMIN OPTION |
|          | CREATE DATABASE                      |
|          | ALTER DATABASE BEGIN/END BACKUP      |
|          | RESTRICTED SESSEION                  |
|          | RECOVER DATABASE UNTIL               |


### 授权
```sql
GRANT {system_privilege|role}
	   [, {system_privilege|role} ]...
	TO    {user|role|PUBLIC}	
	   [, {user|role|PUBLIC} ]...
	[WITH ADMIN OPTION]

```
PUBLIC		        :grants system privilege to all users
WITH ADMIN OPTION	:enables the grantee to further grant the privilege or role	to other users or roles

### 取消授权

```sql
REVOKE {system_privilege|role}
    [, {system_privilege|role} ]...
FROM   {user|role|PUBLIC}
    [, {user|role|PUBLIC} ]...
```

### 权限信息

| table         | role                                                          |
| ------------- | ------------------------------------------------------------- |
| DBA_SYS_PRIVS | lists system privileges granted to users and roles            |
| SESSION_PRIVS | lists the privileges that are currently available to the user |
| DBA_TAB_PRIVS | lists all grants on all objects in the database               |
| DBA_COL_PRIVS | describes all object column grants in the database.           |
