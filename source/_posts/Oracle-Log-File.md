---
title: Oracle Log File
date: 2023-04-03 20:30:57
tags: 
- Database
- Oracle
cover: Picture1.png
---

# Oracle Log File

![log image](Picture1.png)

<!-- more -->

## 重做日志文件(Redo Log Files)

重做日志文件记录对数据所做的所有更改，并提供从系统或介质故障中恢复的机制。

重做日志文件按组(group)进行组织。

Oracle 数据库至少需要两个组(group)，因为日志组要循环使用。

组中的每个重做日志称为一个成员(member)。

重做日志文件的用途

重做日志文件提供了在发生数据库故障时重做事务的方法。每个事务都同步写入重做日志文件，以便在介质发生故障时提供恢复机制。例外情况包括：使用 NOLOGGING 选项完成的直接加载和直接读取。 这包括尚未提交的事务、撤消段信息以及架构和对象管理语句。重做日志文件用于实例无法恢复尚未写入数据文件的已提交数据等情况。**重做日志文件仅用于恢复**

## 重做日志文件的结构

![structure](Picture2.png)

DBA 可以设置 Oracle 数据库来维护联机重做日志文件的副本，以避免由于单点故障而丢失数据库信息。

### 在线重做日志组(Online Redo Log Groups)

联机重做日志文件的一组相同副本称为联机重做日志组。

LGWR 后台进程同时将相同的信息写入组中的所有联机重做日志文件。
Oracle 服务器至少需要两个联机重做日志文件组才能使数据库正常运行。

### 在线重做日志成员(Online Redo Log Members)

组中的每个联机重做日志文件称为一个成员。

组中的每个成员具有相同的日志序列号和相同的大小。每次 Oracle 服务器开始写入日志组时，都会分配日志序列号，以唯一标识每个重做日志文件。当前日志序列号存储在控制文件和所有数据文件的标头中。


### 创建初始重做日志文件

初始联机重做日志组和成员集是在数据库创建期间创建的。

以下参数限制联机重做日志文件的数量：

创建数据库命令中的 **MAXLOGFILES** 参数指定联机重做日志组的绝对最大值。

**MAXLOGFILE** 的最大值和默认值取决于您的操作系统。

创建数据库命令中使用的 **MAXLOGMEMBERS** 参数确定每个组的最大成员数。

**MAXLOG**成员的最大值和默认值取决于您的操作系统。

## 重做日志的工作原理

* 重做日志以循环(Recycle)方式使用。
* 当重做日志文件已满时，LGWR 将移动到下一个日志组。
* 这称为log switch
* 还会发生检查点操作 
* 信息写入控制文件

Oracle 服务器按顺序将对数据库所做的所有更改记录在重做日志缓冲区中。重做条目从重做日志缓冲区写入 LGWR 进程称为当前联机重做日志组的联机重做日志组之一。

LGWR在以下情况下写入：
* 当事务提交时
* 当重做日志缓冲区已满三分之一时
* 当重做日志缓冲区中更改的记录超过一兆字节(1MB)时
* 在 DBWn 将数据库缓冲区缓存中修改的块写入数据文件之前
  
重做日志以循环方式使用。每个重做日志文件组都由每次重用日志时覆盖的日志序列号标识。

LGWR 按顺序写入联机重做日志文件。 当当前联机重做日志组被填满时，LGWR 开始写入下一个组。这称为日志SWITCH。

当最后一个可用的联机重做日志文件被填满时，LGWR 将返回到第一个联机重做日志组并再次开始写入。

### CHECKPOINT

在检查点期间：
DBWn 将被检查点的日志覆盖的许多脏数据库缓冲区写入数据文件。DBWn 写入的缓冲区数由 **FAST_START_MTTR_TARGET** 参数（如果指定）确定。

检查点后台进程 CKPT 更新所有数据文件和控制文件的标头，以反映它已成功完成。
可以对数据库中的所有数据文件或仅对特定数据文件执行检查点。
例如，在以下情况下会发生检查点：
* 在每个日志切换时
* 当实例已使用正常、事务或即时选项关闭时
* 通过设置初始化参数强制时 FAST_START_MTTR_TARGET。
* DBA 手动请求时
* 当 ALTER 表空间 [脱机正常|只读|开始备份]导致对特定数据文件的检查点。
如果LOG_CHECKPOINTS_TO_ALERT初始化参数设置为 TRUE，则有关每个检查点的信息将记录在alert_SID.log文件中。此参数的默认值 FALSE 不记录检查点。

## 获取日志组和成员信息

查询两个动态视图

```sql
V$LOG

V$LOGFILE
```


## 手动切换日志&设置检查点

```sql
ALTER SYSTEM SWITCH LOGFILE;

ALTER SYSTEM CHECKPOINT;
```


如前所述，日志切换和检查点在数据库操作中的某些点自动完成，但 DBA 可以强制进行日志切换或检查点。

可以使用 **FAST_START_MTTR_TARGET** 参数强制检查点。 

FAST_START_MTTR_TARGET = 600 表示实例恢复时间不应超过 600 秒，数据库将根据此目标调整其他参数。

如果使用FAST_START_MTTR_TARGET，则不得使用FAST_START_IO_TARGET和LOG_CHECKPOINT_TIMEOUT。


## 添加重做日志组(Online Redo Log Groups)

```sql
ALTER DATABASE ADD LOGFILE GROUP 3 
(
 '$HOME/ORADATA/u01/log3a.log',
 '$HOME/ORADATA/u02/log3b.log'
)
SIZE 10M;
```
![](P3.png)

若要创建新的联机重做日志文件组，请使用以下 SQL 命令：

```sql
ALTER DATABASE [database]
		ADD LOGFILE [GROUP integer] filespec
		[,          [GROUP integer] filespec]...]
```

您可以使用文件规范指定成员的名称和位置。可以为每个重做日志文件组选择 GROUP 参数的值。如果省略此参数，Oracle 服务器将自动生成其值。


## 添加重做日志成员(Online Redo Log Members)

```sql
ALTER DATABASE ADD LOGFILE MEMBER
'$HOME/ORADATA/u04/log1c.rdo' TO GROUP 1,
'$HOME/ORADATA/u04/log2c.rdo' TO GROUP 2,
'$HOME/ORADATA/u04/log3c.rdo' TO GROUP 3;

```

![](P4.png)

可以使用以下命令将新成员添加到现有重做日志文件组：

```sql
ALTER DATABASE [database]
  ADD LOGFILE MEMBER
  [     'filename' [REUSE]
      [, 'filename' [REUSE]]...
  TO {GROUP integer
      |('filename'[, 'filename']...)
      }
  ]...
```

使用日志文件成员的完全指定名称;否则，将在数据库服务器的缺省目录中创建文件。
如果文件已存在，则它必须具有相同的大小，并且必须指定“REUSE”选项。您可以通过指定组的一个或多个成员或指定组编号来标识目标组。

## 删除重做日志组(Online Redo Log Groups)

```sql
ALTER DATABASE DROP LOGFILE GROUP 3;
```
![](P%.png)

要增加或减少联机重做日志组的大小，请添加新的联机重做日志组（具有新大小），然后删除旧日志组。

可以使用以下命令删除整个联机重做日志组：
```sql
ALTER DATABASE [database]
DROP LOGFILE {GROUP integer|('filename'[, 'filename']...)}
      [,{GROUP integer|('filename'[, 'filename']...)}]...
```
### 限制
* 一个实例至少需要两组在线重做日志文件。
* 无法删除活动组或当前组。
* 删除联机重做日志组时，不会删除操作系统文件。

## 删除重做日志成员(Online Redo Log Members)

```sql
ALTER DATABASE DROP LOGFILE MEMBER '$HOME/ORADATA/u04/log3c.rdo';
```

![](P6.png)


您可能希望删除联机重做日志成员，因为它无效。如果要删除一个或多个特定的联机重做日志成员，请使用以下“更改数据库删除日志文件成员”命令：
```sql
ALTER DATABASE [database]
	DROP LOGFILE MEMBER 'filename'[, 'filename']...
```
### 限制
*  如果要删除的成员是组中的最后一个有效成员，则无法删除该成员。
*  如果组是最新的，则必须先强制切换日志文件，然后才能删除成员。
*  如果数据库在 ARCHIVELOG 模式下运行，并且未归档成员所属的日志文件组，则无法删除该成员。
*  删除联机重做日志成员时，不会删除操作系统文件。

## 清除联机重做日志文件 (online redo log files)

```sql
ALTER DATABASE CLEAR LOGFILE '$HOME/ORADATA/u01/log2a.rdo';
```

如果重做日志文件在所有成员中都已损坏，则DBA 可以通过使用 ALTER DATABASE CLEAR 日志文件重新初始化这些日志文件来解决此问题：

```sql
	ALTER DATABASE [database]
	CLEAR [UNARCHIVED] LOGFILE
		 {GROUP integer|('filename'[, 'filename']...)}
		[,{GROUP integer|('filename'[, 'filename']...)}]...
```

使用此命令等效于添加和删除联机重做日志文件。但是，即使只有两个日志组，每个日志组一个文件，即使清除的组可用但未存档，也可以发出此命令。

### 限制
* 您可以清除联机重做日志文件，无论它是否已存档。但是，如果未存档，则必须包含关键字“**UNARCHIVED**”。如果恢复需要联机重做日志文件，则会导致备份不可用。
* 重新定位和重命名重做日志文件
* 可以通过添加新日志文件并删除旧日志文件来更改联机重做日志文件的位置。 另一种方法“更改数据库重命名文件”可用，但这需要将数据库置于 MOUNT 模式。 因此，添加新的和删除旧的要容易得多。


### 联机重做日志文件数

若要确定数据库实例的适当数量的联机重做日志文件，必须测试不同的配置。
在某些情况下，一个数据库实例可能只需要两个组。在其他情况下，数据库实例可能需要其他组来保证这些组始终可用于 LGWR。例如，如果 LGWR 跟踪文件或警报文件中的消息指示 LGWR 经常因为检查点尚未完成或组尚未存档而必须等待组，则需要添加组。

尽管使用 Oracle 服务器多路复用组可以包含不同数量的成员，但请尝试构建对称配置。非对称配置应只是异常情况（如磁盘故障）的临时结果。

### 联机重做日志文件的位置

多路复用联机重做日志文件时，请将组的成员放在不同的磁盘上。通过执行此操作，即使一个成员不可用但其他成员可用，实例也不会关闭。
将归档日志文件和联机重做日志文件分开放在不同的磁盘上，以减少 ARCn 和 LGWR 后台进程之间的争用。


## 控制归档

非归档模式 -> 归档模式
```sql
查询归档信息
archive log list

shutdown immediate（注意不能shutdown abort）

startup mount

alter database archivelog;

alter database open;
```

```sql
归档模式 -> 非归档模式

archive log list （查询归档信息）

shutdown immediate

startup mount

alter database noarchivelog;

alter database open;
```

```sql
在实例启动后，启用自动归档功能
alter system archive log start;

设置最大归档进程数目
alter system set log_archive_max_processes=3;

在实例启动后，禁用自动归档功能
alter system archive log stop;

执行手工归档
alter system archive log all;
```

