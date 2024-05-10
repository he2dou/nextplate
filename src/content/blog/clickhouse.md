---
title: clickhouse 列式数据库实战教程
date: 2023-04-04T05:00:00Z
tags:
  - clickhouse
author: harry tao
categories:
  - Technology
---


> ClickHouse 是用于实时应用程序和分析的速度最快、资源效率最高的开源数据库。


## **第一节 环境安装**

CentOS系统环境

```shell
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://packages.clickhouse.com/rpm/clickhouse.repo
sudo yum install -y clickhouse-server clickhouse-client

sudo /etc/init.d/clickhouse-server start

clickhouse-client # or "clickhouse-client --password" if you set up a password.
```


## 第二节 软件配置


## 第三节 使用方法

使用客户端工具连接server

```shell
clickhouse-client --host 116.63.xx.xx --port 9000 --password little.123
```


创建数据库

```sql
CREATE DATABASE test;
```

压缩分区数据表

```shell
OPTIMIZE TABLE test.tb1 PARTITION 202304 final;
```

压缩全表

```sql
OPTIMIZE TABLE test.tb1 final;
```


使用mysql结构创建ck数据表

```sql

CREATE TABLE test.tb1 ENGINE = ReplacingMergeTree ORDER BY id AS SELECT * FROM mysql('192.168.0.xxx:3306', 'db', 'tablename', 'user', 'password') limit 0

```


## **第四节 数据同步**

**保证原子性重命名表名**

```sh
EXCHANGE TABLES old_table AND new_table;
```

## 第五节 定位问题


```sql
select query from system.query_log where event_time>'2023-04-28 10:00:00' and event_time<'2023-04-28 11:00:00' and query like '%mb_material%' limit 10;\G
```


```sql
SELECT partition, name, rows, active FROM system.parts WHERE table = 'mb_material' AND database = 'mapbridge_ck';
```
### 复制表数据

先创建table1表结构，然后执行下面语句

```sql
insert into database.table1  SELECT * FROM  remote('xxx.com:9000', 'database', table2, 'user', 'password')
```

### 批量执行clickhouse数据同步

脚本文件：[test.sh](http://test.sh/)



```sh
#!/bin/bash

# 检查参数
if [ $# -ne 1 ]; then
  echo "用法：$0 filename"
  exit 1
fi

# 判断文件是否存在
if [ ! -f $1 ]; then
  echo "文件不存在！"
  exit 1
fi

# 读取文件并打印每行内容
while read table; do
  echo "$table"
  clickhouse-client --host ip --port 9000 --query="insert into database.$table  SELECT * FROM  remote('xxx.com:9000', 'database', $table, 'user', 'password')"
done < $1
```

**赋予权限**



```sh
chmod +x test.sh
```

测试文本: a.txt



```sh
table1
table2
```

执行脚本



```sh
./test.sh a.txt
```