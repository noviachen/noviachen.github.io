---
title: Padavan 安装 PushBot 全能推送钉钉消息
tags:
  - Padavan
  - PushBot
  - 钉钉
cover: 'https://s2.loli.net/2024/02/28/RuvtMasAjOE6B3W.png'
abbrlink: 4efd700d
date: 2024-02-28 13:17:40
---

Padavan 自带的 ServerChan 微信推送有每天条数的限制，而且公众号上不显示完整消息，所以想转移到钉钉上去，免费而且使用方便，各地设备的状态变更可以发到同一个群里面，便于查看和管理。

但是 ServerChan 不支持钉钉，自己用的另外一个 OpenWrt 设备上有大神修改的 PushBot 全能推送，但是 K2P 内存空间小，不好安装。幸好恩山论坛上有大神做了修改（参见 [【padavan】serverchan脚本，支持钉钉](https://www.right.com.cn/forum/thread-4065321-1-1.html)），不需要通过 ipk 的方式安装，节约存储空间。

但是大神的教程比较简单，这里做一下自己的记录，同时稍微调整一下，方便其他有需要的朋友。

# 添加钉钉机器人，获取 Token

在钉钉`群设置`-`机器人管理`中添加机器人，选择`自定义`（通过 Webhook 接入自定义服务），输入机器人名字，安全设置选择`自定义关键词`，输入需要设定的关键词，后面会用到。其他不需要更改，点击完成。

之后会得到一个链接，链接中`token=`后面的内容就是 Token。

# 下载文件并上传到路由器

打开这个链接 https://github.com/Twinzo1/padavan/tree/master/serverchan，下载
`serverchan.sh`到本地。

使用 WinSCP 等工具将上述文件上传到`/etc/storage`，并修改权限为 755

# Padavan 设置

进入`自定义设置`-`脚本`，在`路由器启动后执行`下增加如下内容：

```bash
################ PushBot 全能推送 \################
logger -t "【消息推送】" "serverchan脚本"
[ ! -f /etc/storage/serverchan.sh ] && curl -k -s -o /etc/storage/serverchan.sh --connect-timeout 10 --retry 3 https://raw.githubusercontent.com/Twinzo1/padavan/master/serverchan/serverchan.sh
\# 主要变量设置
nvram set serverchan_enable="1" #总开关
nvram set sc_device_name="Padavan" #本设备名称
nvram set sc_sleeptime="60" #检测时间间隔
nvram set sc_send_dd="1" #开启钉钉推送
nvram set sc_dd_bot_keyword="<这里填写自定义关键词>" # 自定义关键词
nvram set sc_dd_bot_token="<这里填写 Token>" # Token
nvram set sc_sc_ipv4="1" #ipv4 变动通知
nvram set sc_cpuload_enable="1" #CPU 负载报警
nvram set sc_cpuload="2" #负载报警阈值
nvram set sc_sc_cl_ls="1" #是否推送当前设备列表
nvram commit
chmod 755 /etc/storage/serverchan.sh && /etc/storage/serverchan.sh start &
```

然后，SSH 连接至路由器，或者在路由器 Web 控制台输入以下命令，将文件保存至闪存。

```bash
/sbin/mtd_storage.sh save
```

也可以到`系统管理`-`恢复/导出/上传设置`，点击`保存 /etc/storage/ 内容到闪存`旁边的提交按钮。

重启路由器试试，如果没问题的话就可以收到钉钉消息了。如果需要更改推送的内容，请参考大神的 [config.md](https://github.com/Twinzo1/padavan/blob/master/serverchan/config.md)，里面有详细说明。