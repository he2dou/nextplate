---
title: DBeaver 开源数据库管理工具
date: 2023-10-31T05:00:00Z
author: harry
categories:
  - Software
tags:
  - dbeaver
draft: false
---


DBeaver  是一款免费的跨平台数据库工具，适用于开发人员、数据库管理员、以及所有从事数据工作的人员。

# DBeaver的时区问题

**查询资料找到解决方法：**

选中数据库连接->右键编辑连接(F4)->连接设置->驱动属性，找到如下三个值进行修改后，

点击确定保存设置后，重新执行sql，时间显示恢复正确。

> 官方网站：https://dbeaver.io/