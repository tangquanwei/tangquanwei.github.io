---
title: Docker Oracle 11g
date: 2023-03-24 19:54:19
tags: 
- Database
- Oracle
---

<!-- more -->
```
docker run -d -p 1521:1521\
 --name oracle-11g\
 -e ORACLE_ALLOW_REMOTE=true\
 -e ORACLE_DISABLE_ASYNCH_IO=true\
 oracleinanutshell/oracle-xe-11g
```

关闭实例

shutdown immediate

启动实例到nomount状态
startup nomount

建立控制文件
alter database backup controlfile to trace;

show parameter user_dump_dest;

 





```sql
--
--     Set #1. NORESETLOGS case
--
-- The following commands will create a new control file and use it
-- to open the database.
-- Data used by Recovery Manager will be lost.
-- Additional logs may be required for media recovery of offline
-- Use this only if the current versions of all online logs are
-- available.
-- After mounting the created controlfile, the following SQL
-- statement will place the database in the appropriate
-- protection mode:
--  ALTER DATABASE SET STANDBY DATABASE TO MAXIMIZE PERFORMANCE
STARTUP NOMOUNT
CREATE CONTROLFILE REUSE DATABASE "QUANWEI" NORESETLOGS  NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
LOGFILE
  GROUP 1 '/u01/app/oracle/fast_recovery_area/QUANWEI/onlinelog/o1_mf_1_g6c5nhsl_.log'  SIZE 50M BLOCKSIZE 512,
  GROUP 2 '/u01/app/oracle/fast_recovery_area/QUANWEI/onlinelog/o1_mf_2_g6c5nj25_.log'  SIZE 50M BLOCKSIZE 512
-- STANDBY LOGFILE
DATAFILE
  '/u01/app/oracle/oradata/QUANWEI/system.dbf',
  '/u01/app/oracle/oradata/QUANWEI/sysaux.dbf',
  '/u01/app/oracle/oradata/QUANWEI/undotbs1.dbf',
  '/u01/app/oracle/oradata/QUANWEI/users.dbf'
CHARACTER SET AL32UTF8
;
RECOVER DATABASE
-- Database can now be opened normally.
ALTER DATABASE OPEN;
-- Commands to add tempfiles to temporary tablespaces.
-- Online tempfiles have complete space information.
-- Other tempfiles may require adjustment.
ALTER TABLESPACE TEMP ADD TEMPFILE '/u01/app/oracle/oradata/QUANWEI/temp.dbf'
     SIZE 20971520  REUSE AUTOEXTEND ON NEXT 655360  MAXSIZE 32767M;
```

```sql
-- End of tempfile additions.
--
--     Set #2. RESETLOGS case
--
-- The following commands will create a new control file and use it
-- to open the database.
-- Data used by Recovery Manager will be lost.
-- The contents of online logs will be lost and all backups will
-- be invalidated. Use this only if online logs are damaged.
-- After mounting the created controlfile, the following SQL
-- statement will place the database in the appropriate
-- protection mode:
--  ALTER DATABASE SET STANDBY DATABASE TO MAXIMIZE PERFORMANCE
STARTUP NOMOUNT
CREATE CONTROLFILE REUSE SET DATABASE QUANWEI RESETLOGS  NOARCHIVELOG
    MAXLOGFILES 16
    MAXLOGMEMBERS 3
    MAXDATAFILES 100
    MAXINSTANCES 8
    MAXLOGHISTORY 292
LOGFILE
  GROUP 1 '/u01/app/oracle/fast_recovery_area/XE/onlinelog/o1_mf_1_g6c5nhsl_.log'  SIZE 50M BLOCKSIZE 512,
  GROUP 2 '/u01/app/oracle/fast_recovery_area/XE/onlinelog/o1_mf_2_g6c5nj25_.log'  SIZE 50M BLOCKSIZE 512
-- STANDBY LOGFILE
DATAFILE
  '/u01/app/oracle/oradata/XE/system.dbf',
  '/u01/app/oracle/oradata/XE/sysaux.dbf',
  '/u01/app/oracle/oradata/XE/undotbs1.dbf',
  '/u01/app/oracle/oradata/XE/users.dbf'
CHARACTER SET AL32UTF8
;
-- Commands to re-create incarnation table
-- Below log names MUST be changed to existing filenames on
-- disk. Any one log file from each branch can be used to
-- re-create incarnation records.
-- ALTER DATABASE REGISTER LOGFILE '/u01/app/oracle/fast_recovery_area/XE/archivelog/2023_03_29/o1_mf_1_1_%u_.arc';
-- ALTER DATABASE REGISTER LOGFILE '/u01/app/oracle/fast_recovery_area/XE/archivelog/2023_03_29/o1_mf_1_1_%u_.arc';
-- Recovery is required if any of the datafiles are restored backups,
-- or if the last shutdown was not normal or immediate.
RECOVER DATABASE USING BACKUP CONTROLFILE
-- Database can now be opened zeroing the online logs.
ALTER DATABASE OPEN RESETLOGS;
-- Commands to add tempfiles to temporary tablespaces.
-- Online tempfiles have complete space information.
-- Other tempfiles may require adjustment.
ALTER TABLESPACE TEMP ADD TEMPFILE '/u01/app/oracle/oradata/XE/temp.dbf'
     SIZE 20971520  REUSE AUTOEXTEND ON NEXT 655360  MAXSIZE 32767M;
-- End of tempfile additions.
--
```

pfile 
init.ora
```
QUAWNEI.__db_cache_size=415236096
QUAWNEI.__java_pool_size=4194304
QUAWNEI.__large_pool_size=4194304
QUAWNEI.__oracle_base='/u01/app/oracle'#ORACLE_BASE set from environment
QUAWNEI.__pga_aggregate_target=201326592
QUAWNEI.__sga_target=603979776
QUAWNEI.__shared_io_pool_size=0
QUAWNEI.__shared_pool_size=171966464
QUAWNEI.__streams_pool_size=0
*.audit_file_dest='/u01/app/oracle/admin/XE/adump'
*.compatible='11.2.0.0.0'
*.control_files='/u01/app/oracle/oradata/xe/control01.ctl'
*.db_name='QUANWEI'
*.DB_RECOVERY_FILE_DEST='/u01/app/oracle/fast_recovery_area'
*.DB_RECOVERY_FILE_DEST_SIZE=10G
*.diagnostic_dest='/u01/app/oracle'
*.disk_asynch_io=FALSE
*.dispatchers='(PROTOCOL=TCP) (SERVICE=XEXDB)'
*.job_queue_processes=4
*.open_cursors=300
*.pga_aggregate_target=200540160
*.remote_login_passwordfile='EXCLUSIVE'
*.sessions=20
*.sga_target=601620480
*.shared_servers=4
*.undo_management='AUTO'
*.undo_tablespace='UNDOTBS1'
```

create spfile='/u01/app/oracle/product/11.2.0/xe/dbs/spfileQW.ora'
 from pfile='/u01/app/oracle/product/11.2.0/xe/dbs/init.ora'

 select value from v$parameter where name='db_name';









select group#,status from v$log;

select group#,status from v$logfile;

alter database add logfile group 2
  ('/home/oracle/REDO0401.LOG','/home/oracle/REDO0402.LOG') 
  SIZE 10M;


alter database add logfile member	
 '/home/oracle/REDO0501.LOG' TO GROUP 1, 
 '/home/oracle/REDO0502.LOG' TO GROUP 2,
 '/home/oracle/REDO0503.LOG' TO GROUP 3;


ALTER DATABASE DROP LOGFILE GROUP 3;

ALTER DATABASE DROP LOGFILE MEMBER '$HOME/ORADATA/u04/log3c.rdo';



