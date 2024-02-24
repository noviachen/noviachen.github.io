---
title: 更新 Pandavan 上的 Transmission Web Control
tags:
  - Pandavan
  - Transmission
cover: https://i.loli.net/2020/10/07/fR1bZNcvYVWzmon.jpg
abbrlink: df7bdf3c
date: 2019-05-01 01:05:59
---


作为一个更新狂魔，看到软件、APP 需要更新总是忍不住，Pandavan 自带的 Transmission Web Control 也不例外，竟然还是 2014 年的版本。

<!--more-->

下载最新版本的 Transmission Web Control，详见下方地址。

> 最新版本：https://github.com/ronggang/transmission-web-control/archive/master.zip
>
> 最新 Release 版本：<https://github.com/ronggang/transmission-web-control/releases>

下载之后解压，我们只要里面的 src 文件夹。将 src 文件夹上传到相应目录。我使用的是移动硬盘，放到 `/media/AiDisk_a1/transmission` 里面，当然可以放到其他地方。之后，在路由器的设置页面依次打开 `高级设置` >> `自定义设置` >> `脚本` >> `在路由器启动后执行`，在 `挂载存储设备` 后面添加如下代码。

```
### 挂载最新版 transmission-web-control
mount --bind /media/AiDisk_a1/transmission/src /usr/share/transmission/web
sleep3
```

![](https://i.loli.net/2020/10/07/QbLdwBumij1rqxh.jpg)

应用设置后重启路由器，就可以看到最新版本的界面了。好像看起来也差不多，哈哈哈哈哈~

![](https://i.loli.net/2020/10/07/jXcApWh1qfbY4vH.jpg)


> 参考文章：[更新 Padavan 上的 transmission-web-control](<https://chua.pro/upgrade-transmission-web-control-on-padavan/>)

