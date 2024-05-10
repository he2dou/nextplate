---
title: Screen 全屏窗口管理器
date: 2023-11-27T05:00:00Z
categories:
  - Software
tags:
  - screen
author: harry
---


Linux Screen是一个全屏窗口管理器，它可以创建多个窗口，并在其间进行切换。


### 1. 安装screen

```sh
sudo yum install -y screen
```


### 2. 常用命令

```sh
# 进入工程目录
cd /alidata/redis-shake-v2.0.3

# 新建窗口
screen -S redis-shake

# 执行命令
screen ./redis-shake.linux -conf=redis-shake.conf -type=sync

# 分离会话
ctrl+a，d

# 列出screen
screen -ls

# 进入screen
screen -r redis-shake

# 关闭 screen
screen -X -S redis-shake quit

```

**批量删除screen任务的脚本**

```shell

#!/bin/bash  

# 列出所有Screen会话  
screen -ls

# 询问用户要删除的会话列表  
read -p "请输入要删除的会话列表，用逗号分隔（例如：session1,session2,session3）:" sessions_to_delete

# 将用户输入的列表拆分成数组  
IFS=',' read -ra sessions <<< "$sessions_to_delete"

# 循环遍历要删除的会话列表  
for session in "${sessions[@]}"; do
  # 检查会话是否存在  
  #if screen -ls | grep -oP "^$session "; then  
    # 删除会话  
    screen -X -S "$session" quit
    echo "已删除会话：$session"  
  #else  
  #  echo "会话 $session 不存在。"  
  #fi  
done



```


使用方法：

1. 将上述脚本保存为一个文件，例如 delete_screen_sessions.sh。
2. 打开终端，导航到脚本所在的目录。
3. 运行以下命令来赋予脚本执行权限：
   
```shell
chmod +x delete_screen_sessions.sh
```

```shell
./delete_screen_sessions.sh
```

4. 脚本将列出所有Screen会话，并提示您输入要删除的会话列表。按照提示输入要删除的会话名称，用逗号分隔。例如，输入 session1,session2,session3。
5. 脚本将循环遍历输入的会话列表，检查会话是否存在并删除它们。
6. 完成删除后，脚本将显示已删除的会话列表。


请注意，这个脚本只是删除指定的Screen会话，而不会终止或删除与会话关联的进程。如果您希望同时终止进程，您需要另外处理这些进程。此外，请谨慎使用此脚本，确保您知道要删除的会话名称，以免意外删除重要的会话。

> 参考地址： [https://blog.csdn.net/hejunqing14/article/details/50338161](https://blog.csdn.net/hejunqing14/article/details/50338161)
