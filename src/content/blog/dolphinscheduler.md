---
title: Dolphinscheduler 工作流调度
date: 2023-12-02T05:00:00Z
categories:
  - Software
tags:
  - dolphinscheduler
author: harry
---


Dolphinscheduler 是一种开源的、分布式的、易于使用的大数据工作流调度系统。

## 下载安装环境

环境配置：

- 服务器系统版本：CentOS Linux release 7.9.2009 (Core)
- dolphinscheduler软件版本：3.1.4

### 1.安装 jdk-20_linux-x64_bin

```sh
rpm -ivh jdk-20_linux-x64_bin.rpm
```

测试是否ok：

`java --version`

下载地址：

### 2.下载 mysql-connector-j-8.0.31.jar

下载mysql-connector-j-8.0.31.jar，并拷贝到服务目录，并安装mysql jar文件到指定目录:

```sh
 cp mysql-connector-j-8.0.31.jar apache-dolphinscheduler-3.1.4-bin/standalone-server/libs/standalone-server/ 
 ```

下载地址：

### 3.下载 apache-dolphinscheduler-3.1.4-bin


下载地址：



## 配置环境变量

```sh
JAVA_HOME=/usr/local/java 

export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar 

export PATH=/usr/local/nginx/sbin:$PATH:${JAVA_HOME}/bin

```



## 修改配置文件

cd apache-dolphinscheduler-3.1.4-bin

vim bin/env/dolphinscheduler_env.sh

```sh
# JAVA_HOME, will use it to start DolphinScheduler server
export JAVA_HOME=/usr/lib/jvm/jdk-20-oracle-x64

# Database related configuration, set database type, username and password
export DATABASE=mysql
export SPRING_PROFILES_ACTIVE=${DATABASE}
export SPRING_DATASOURCE_URL="jdbc:mysql://host:3306/database?useUnicode=true&characterEncoding=UTF-8&useSSL=false"
export SPRING_DATASOURCE_USERNAME="username"
export SPRING_DATASOURCE_PASSWORD="password"

# DolphinScheduler server related configuration
export SPRING_CACHE_TYPE=${SPRING_CACHE_TYPE:-none}
export SPRING_JACKSON_TIME_ZONE="Asia/Shanghai"
export MASTER_FETCH_COMMAND_NUM=${MASTER_FETCH_COMMAND_NUM:-10}

```

## 导入sql初始化

1. 先创建数据库为 **dolphinscheduler**

2. 进入软件目录 **~/apache-dolphinscheduler-3.1.4-bin/tools/sql/sql**

3. 执行一下 **dolphinscheduler_mysql.sql** ，生成数据表


## 开始关闭程序

**启动程序**

```sh
sh ./bin/dolphinscheduler-daemon.sh start standalone-server
```

**停止程序**

```sh
sh ./bin/dolphinscheduler-daemon.sh stop standalone-server
```

**查看日志**

cd ~/apache-dolphinscheduler-3.1.4-bin/standalone-server/logs

```sh
tail -f dolphinscheduler-standalone.log
```



## 软件使用说明

- 软件端口开放：**12345**

- 默认用户名和密码：**admin/dolphinscheduler123**   （记得安装完成之后修改默认密码）

- 系统需要配置
  - 建立项目：项目管理- 创建项目
  - 新建租户：安全中心-租户管理- 创建租户 操作系统租户：root，队列：default（默认）
  - 配置租户：安全中心-用户管理- 编辑用户（设置租户和其它信息）
  - 告警设置：安全中心-告警实例管理-设置企业微信告警通知


> 官方地址：[https://dolphinscheduler.apache.org/zh-cn/docs/3.1.4/guide/start/quick-start](https://dolphinscheduler.apache.org/zh-cn/docs/3.1.4/guide/start/quick-start)