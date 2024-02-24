---
title: Debian 安装 jiacrontab  管理定时任务
tags:
  - Debian
  - jiacrontab
cover: https://i.loli.net/2020/10/07/HxnsXLNTG6oiBSF.png
abbrlink: c31b0dab
date: 2019-02-25 07:45:25
---

自己有几个需要定时运行的爬虫，用不到诸如 pyspider 之类的爬虫框架，但是又觉得 crontab 不太直观。所以找到了 jiacrontab，自带 web 界面，使用 Go 语言开发。

<!--more-->

# jiacrontab 介绍

提供可视化界面的定时任务&常驻任务管理工具。

## 功能

1. 允许设置每个脚本的超时时间，超时操作可选择邮件通知管理者，或强杀脚本进程
2. 允许设置脚本的最大并发数。
3. 一台 server 管理多个 client。
4. 每个脚本都可在 server 端灵活配置，如测试脚本运行，查看日志，强杀进程，停止定时...。
5. 允许添加脚本依赖（支持跨服务器），依赖脚本提供同步和异步的执行模式。
6. 友好的 web 界面，方便用户操作。
7. 脚本出错时可选择邮箱通知多人。
8. 支持常驻任务，任务失败后可配置自动重启。
9. 支持管道操作。

## 说明
jiacrontab 由 server，client 两部分构成，两者完全独立通过 rpc 通信。
server：向用户提供可视化界面，调度多个 client。
client：实现定时逻辑，隔离用户脚本，将 client 布置于多台服务器上可由 server 统一管理。 每个脚本的定时格式完全兼容 linux 本身的 crontab 脚本配置格式。

## 界面截图

 演示地址：<http://jiacrontab.iwannay.cn/>，用户名 `admin` 密码 `123456`。

![](https://i.loli.net/2020/10/07/mSClQixZXvTMJNW.jpg)

![](https://i.loli.net/2020/10/07/Kmwqry25gcHxJFV.jpg)

![](https://i.loli.net/2020/10/07/BXexf3h7aQ8qcds.jpg)


# 下载安装并运行

Github 地址：<https://github.com/iwannay/jiacrontab>

最新版下载地址：<https://jiacrontab.iwannay.cn/download/>

以目前的最新版本 v1.4.5 为例：

```bash
$ wget https://jiacrontab.iwannay.cn/download/jiacrontab-v1.4.5-linux-amd64.zip

$ unzip jiacrontab-v1.4.5-linux-amd64.zip

$ cd jiacrontab/server
$ nohup ./jiaserver &> jiaserver.log &

$ cd .. && cd client
$ nohup ./jiaclient &> jiaclient.log &
```

之后，可以在浏览器中输入 http://YourServerIP:20000 进行访问。默认用户名为 `admin`，密码是 `123456`。

# 设置开机自启动

在 `/etc/init.d` 新建开机启动脚本，命名为 `jiacrontab`

```bash
$ vi /etc/init.d/jiacrontab

# 输入以下内容，:wq 保存

#!/bin/bash
### BEGIN INIT INFO
# Provides:          uznEnehC
# Required-Start:    $network $remote_fs $local_fs
# Required-Stop:     $network $remote_fs $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: jiacontab
# Description:       jiacontab startup
### END INIT INFO
cd root/jiacrontab/server # 假设之前下载到 root 文件夹，根据自身情况修改
nohup ./jiaserver &> jiaserver.log &
cd .. && cd client
nohup ./jiaclient &> jiaclient.log &
exit 0
```

给予脚本相应的权限

```bash
$ chmod 777 /etc/init.d/jiacrontab
```

 设置开机启动

```bash
$ update-rc.d jiacrontab defaults
# update-rc.d -f jiacrontab remove # 删除开机启动，添加 -f 参数强制删除
```