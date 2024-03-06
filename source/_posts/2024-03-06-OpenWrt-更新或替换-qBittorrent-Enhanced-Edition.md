---
title: OpenWrt 更新或替换 qBittorrent Enhanced Edition
tags:
  - OpenWrt
  - qBittorrent
cover: 'https://s2.loli.net/2024/03/04/VoRSjDWY1uLP4QJ.png'
abbrlink: a412d2f
date: 2024-03-06 15:33:05
---

iStoreOS 安装的 qBittorrent 为普通版本，没有反吸血功能。所以想升级到 Enhanced 版本，而且新增种子的时候可以自动添加 trackers，可能会增加下载的速度。说干就干。

打开 qBittorrent Enhanced Edition 的 Github 仓库地址 [https://github.com/c0re100/qBittorrent-Enhanced-Edition](https://github.com/c0re100/qBittorrent-Enhanced-Edition), 到 Release 页面下载最新版本。请根据自己路由器的版本选择对应的包，比如我的是 x86 软路由，下载`qbittorrent-enhanced-nox_x86_64-linux-musl_static.zip`这个文件。

将下载的文件解压，得到 qbittorrent-nox。

首先网页端关闭 qBittorrent，然后使用 WinSCP 等工具, 打开 /usr/bin 文件夹，删除原本的 qbittorrent-nox 文件。 

上传之前解压的 qbittorrent-nox 文件，并修改权限为755。返回到网页端重新启用 qBittorrent。

![](https://s2.loli.net/2024/03/04/aJwGNObgzXETR3F.png)

查看版本号，是否已经更新了。完工！