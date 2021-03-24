# Mac Mysql 管理

&nbsp;

## 安装

1、执行安装命令或官网下载 https://dev.mysql.com/downloads/mysql/

```bash
$ brew install mysql
```

&nbsp;

2、安装完后启动mysql

```shell
$ mysql.server start
```

&nbsp;

3、执行安全设置

```shell
$ mysql_secure_installation
```

显示如下

```bash
There are three levels of password validation policy:

LOW    Length >= 8
MEDIUM Length >= 8, numeric, mixed case, and special characters
STRONG Length >= 8, numeric, mixed case, special characters and dictionary file
```

按照提示选择密码等级，并设置root密码

&nbsp;

## 创建新的数据库、用户并授权

&nbsp;

### 登录mysql

```undefined
mysql -u root -p
```

按提示输入root密码

&nbsp;

### 修改 root 密码

```mysql
mysql> set password for root@localhost = password('952005111');
Query OK, 0 rows affected, 1 warning (0.00 sec)
```

```mysql
root@poksi-test-2019:~# mysql -u root -p
Enter password: 
```

&nbsp;

### 创建数据库

```mysql
create database retail_db character set utf8mb4;
```

&nbsp;

### 创建用户

```mysql
create user 'retail_u'@'%' identified by 'retail_PWD_123';
```

&nbsp;

### 授权用户

```mysql
grant all privileges on retail_db.* to 'retail_u'@'%';
```

```mysql
flush privileges;
```

&nbsp;

### 查看当前的数据库

```mysql
show databases;
```

&nbsp;

### 显示当前数据库的表单

```mysql
show tables;
```

&nbsp;

## 建表

```mysql
CREATE TABLE t_user(
  key_id VARCHAR(255) NOT NULL PRIMARY KEY,  -- id 统一命名为key_id
  user_name VARCHAR(255) NOT NULL ,
  password VARCHAR(255) NOT NULL ,
  phone VARCHAR(255) NOT NULL,
  deleted INT NOT NULL DEFAULT 0,  -- 逻辑删除标志默认值
  create_time   timestamp NULL default CURRENT_TIMESTAMP, -- 创建时间默认值
  update_time   timestamp NULL default CURRENT_TIMESTAMP -- 修改时间默认值
) ENGINE=INNODB AUTO_INCREMENT=1000 DEFAULT CHARSET=utf8mb4;
```

&nbsp;

## 检查mysql服务状态

- 先退出mysql命令行，输入命令

```bash
$ systemctl status mysql.service
```

&nbsp;

- 显示如下结果说明mysql服务是正常的

```jsx
● mysql.service - MySQL Community Server
  Loaded: loaded (/lib/systemd/system/mysql.service; enabled; vendor preset: enabled)
  Active: active (running) since Wed 2019-05-22 10:53:13 CST; 13min ago
Main PID: 16686 (mysqld)
   Tasks: 29 (limit: 4667)
  CGroup: /system.slice/mysql.service
          └─16686 /usr/sbin/mysqld --daemonize --pid-file=/run/mysqld/mysqld.pid

May 22 10:53:12 poksi-test-2019 systemd[1]: Starting MySQL Community Server...
May 22 10:53:13 poksi-test-2019 systemd[1]: Started MySQL Community Server.
```

&nbsp;

#### 启动mysql

```bash
sudo /usr/local/mysql/support-files/mysql.server start
```

&nbsp;

#### 停止mysql

```bash
sudo /usr/local/mysql/support-files/mysql.server stop
```

&nbsp;

#### 重启mysql

```bash
sudo /usr/local/mysql/support-files/mysql.server restart
```

&nbsp;

#### 修改 my.cnf

首先，先找到 `my.cnf` 的路径，如果安装的时候没有做什么修改，那么它的默认路径是在 `/etc/my.cnf` 这个地方。如果找不到也没有关系，我们可以用两步找到它；

- 第一步：首先找到mysqld的路径：

```bash
$ which mysqld
```

&nbsp;

出来的路径就是mysqld 的路径；

[![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=512762346,4012464387&fm=173&app=25&f=JPEG?w=471&h=139&s=55523BC013D938605C591C0B0200A0CB)](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=512762346,4012464387&fm=173&app=25&f=JPEG?w=471&h=139&s=55523BC013D938605C591C0B0200A0CB)

- 第二步：敲命令：

```bash
$ /usr/local/mysql/bin/mysqld --verbose --help |grep -A 1 'Default options'
```

```conf
Default options are read from the following files in the given order:
/etc/my.cnf /etc/mysql/my.cnf /usr/local/mysql/etc/my.cnf ~/.my.cnf
```

&nbsp;

下面我们打开my.cnf看看里面都有些什么配置：

[![img](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=874431039,4121996540&fm=173&app=25&f=JPEG?w=620&h=270&s=4542BB40B3B9BA69067D9D0F0000E0C0)](https://ss0.baidu.com/6ONWsjip0QIZ8tyhnq/it/u=874431039,4121996540&fm=173&app=25&f=JPEG?w=620&h=270&s=4542BB40B3B9BA69067D9D0F0000E0C0)

&nbsp;

- 如没有，新建，并将文件放入到上面所查位置。 my.cnf
  - `/etc/my.cnf /etc/mysql/my.cnf /usr/local/mysql/etc/my.cnf ~/.my.cnf`

```bash
[mysqld]
wait_timeout=31536000
interactive_timeout=31536000
bind-address = 0.0.0.0
connect_timeout=10000
 
 ## 其它用默认即可
```

&nbsp;

# [在mac上使用tar.gz安装mysql](https://www.cnblogs.com/ephemerid/p/10294918.html)

官方：

- download: https://dev.mysql.com/downloads/mysql/
- mysql参考文档：https://dev.mysql.com/doc/

环境：

- macOS Mojave 10.14.2

用户：

- 管理员用户，期间没有新建mysql用户或组

安装包：

- mysql-8.0.13-macos10.14-x86_64.tar.gz

&nbsp;

## 安装配置

1.解压
下载的*tar.gz*解压后，并移动到/usr/local/mysql，目录结构如下：

```
$ pwd
/usr/local
$ tree mysql -L 1
mysql
├── LICENSE
├── LICENSE.router
├── README
├── README.router
├── bin
├── data
├── docs
├── include
├── lib
├── man
├── run
├── share
└── support-files
```

> 实际上是按照Unix目录结构的规范，建议这么做。
> 本人实际操作过程中，只是通过`sudo ln -s ${MYSQL_HOME} /usr/local/mysql`，创建了一个软链接。
> 解压命令：`tar -xzvf ${filename}`

&nbsp;

2.修改权限：`sudo chown -R usr:group /usr/local/mysql`

&nbsp;

3.初始化

```bash
$ cd /usr/local/mysql
$ bin/mysqld --initialize --user=ephemerid --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data
...
initializing of server has completed
```

&nbsp;

4.启动

```
$ support-files/mysql.server start
Starting MySQL
. SUCCESS! 
```

&nbsp;

5.连接及修改初始密码

```bash
$ bin/mysql -uroot -pA_mBvrbnD0*r
mysql: [Warning] Using a password on the command line interface can be insecure.
Welcome to the MySQL monitor.  Commands end with ; or \g.
Your MySQL connection id is 8
Server version: 8.0.13

Copyright (c) 2000, 2018, Oracle and/or its affiliates. All rights reserved.

Oracle is a registered trademark of Oracle Corporation and/or its
affiliates. Other names may be trademarks of their respective
owners.

Type 'help;' or '\h' for help. Type '\c' to clear the current input statement.

mysql> show databases;
ERROR 1820 (HY000): You must reset your password using ALTER USER statement before executing this statement.

mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'pass123456';
Query OK, 0 rows affected (0.02 sec)

mysql> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
| sys                |
+--------------------+
4 rows in set (0.01 sec)

mysql> exit
Bye
```

**!!!平常输入密码的时候不要直接添加在`-p`选项后面了，会被看到的，一定要注意安全!!!**

> 第一次登陆时，会提示需要重置密码：
>
> > ALTER USER 'root'@'localhost' IDENTIFIED BY 'pass123456';

&nbsp;

6.停止服务

```
$ support-files/mysql.server stop
Shutting down MySQL
.. SUCCESS! 
```

&nsbp;

## EORROR

### The server quit without updating PID file

在未初始化，就启动时出现了：

```
$ sudo ./support-files/mysql.server start
Password:
Starting MySQL
. ERROR! The server quit without updating PID file (/usr/local/mysql/data/ephemerid.local.pid).
```

#### 解决

初始化一下，即上文提到的 `mysqld --initialize`

&nbsp;

### MY-011011

在初始化时，没有找到有效的mysql目录以及mysql data目录。

```
$ sudo ./bin/mysqld --initaialize --user=ephemerid
2019-01-20T05:13:45.476844Z 0 [System] [MY-010116] [Server] /Workspaces/programfiles/mysql-8.0.13-macos10.14-x86_64/bin/mysqld (mysqld 8.0.13) starting as process 993
2019-01-20T05:13:45.480320Z 0 [Warning] [MY-010159] [Server] Setting lower_case_table_names=2 because file system for /Workspaces/programfiles/mysql-8.0.13-macos10.14-x86_64/data/ is case insensitive
2019-01-20T05:13:45.487935Z 1 [ERROR] [MY-011011] [Server] Failed to find valid data directory.
2019-01-20T05:13:45.488090Z 0 [ERROR] [MY-010020] [Server] Data Dictionary initialization failed.
2019-01-20T05:13:45.488119Z 0 [ERROR] [MY-010119] [Server] Aborting
2019-01-20T05:13:45.488808Z 0 [System] [MY-010910] [Server] /Workspaces/programfiles/mysql-8.0.13-macos10.14-x86_64/bin/mysqld: Shutdown complete (mysqld 8.0.13)  MySQL Community Server - GPL.
```

&nbsp;

#### 解决

就是添加的两个选项，即上文提到的 `--basedir` 和 `--datadir` 
了解更多，参考：

- https://dev.mysql.com/doc/refman/8.0/en/data-directory-initialization-mysqld.html