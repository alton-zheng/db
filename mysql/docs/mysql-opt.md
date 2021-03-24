# Mysql	调优

&nbsp;

## 监控

- `type`
  - `ALL` displays all information
  - `BLOCK IO` displays counts for block input and output operations
  - `CONTEXT SWITCHES` displays counts for voluntary and involuntary context switches
  - `CPU` displays user and system CPU usage times
  - `IPC` displays counts for messages sent and received
  - `MEMORY` is not currently implemented
  - `PAGE FAULTS` displays counts for major and minor page faults
  - `SOURCE` displays the names of functions from the source code, together with the name and line number of the file in which the function occurs
  - `SWAPS` displays swap counts

&nbsp;

- e.g.

```mysql
-- v5.x
-- 开启查看执行计划
set profiling = 1;

select * from dual;


SHOW PROFILE [type [, type] ... ]
    [FOR QUERY n]
    [LIMIT row_count [OFFSET offset]]

type: {
    ALL
  | BLOCK IO
  | CONTEXT SWITCHES
  | CPU
  | IPC
  | MEMORY
  | PAGE FAULTS
  | SOURCE
  | SWAPS
}

-- 查看当前窗口执行过的语句执行时间
show profiles;

-- 查看最近一条语句所有执行过程 cost
show profile;

show profile all;
show profile cpu;

-- 查看执行计划 2 为 query id
show profile for query 2;
```

&nbsp;

&nbsp;

## Performance Schema

```mysql
-- 以上 profile profiles 在未来版本会被删除，用以下方式查看执行计划
show databases;
use performance_schema;
show tables;

-- 表数据不持久化，会在内存里进行缓存， 服务，器启动后，会重新开始记录
-- instruments: 生产者，用于采集事件信息
select * from set_instruments;
```

&nbsp;

```mysql
[mysqld]
performance_schema=ON

mysql> SHOW VARIABLES LIKE 'performance_schema';
+--------------------+-------+
| Variable_name      | Value |
+--------------------+-------+
| performance_schema | ON    |
+--------------------+-------+

*************************** 1. row ***************************
      ENGINE: PERFORMANCE_SCHEMA
     SUPPORT: YES
     COMMENT: Performance Schema
TRANSACTIONS: NO
          XA: NO
  SAVEPOINTS: NO

mysql> SHOW ENGINES\G
...
      Engine: PERFORMANCE_SCHEMA
     Support: YES
     Comment: Performance Schema
Transactions: NO
          XA: NO
  Savepoints: NO
```

&nbsp;

- https://dev.mysql.com/doc/refman/5.7/en/performance-schema-quick-start.html
- https://dev.mysql.com/doc/refman/5.7/en/performance-schema.html

&nbsp;

## show processlist

- https://dev.mysql.com/doc/refman/5.7/en/show-processlist.html

&nbsp;

## Schema与数据类型优化

### 数据类型的优化



