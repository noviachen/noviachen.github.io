---
title: 杭州电信 OpenWrt 实现任意设备 IPTV 播放.md
tags:
  - OpenWrt
  - IPTV
  - 光猫
cover: 'https://s2.loli.net/2024/02/26/146AJYMR7oOwzlq.webp'
abbrlink: 5778b497
date: 2024-01-26 08:36:49
---


前段时间由于常用的两个直播 APP 被禁，想看春晚的话就不太容易了，找了其他 APP，清晰度也不是很满意，而且可能也存在被封禁的风险，所以就想到了电信 IPTV。因为之前在 Padavan 搞过单线复用，这次还是挺熟练的。

> 运营商：中国电信
> 城市：浙江杭州
> 光猫型号：中兴 ZXHN F610GV9（GPON）
> 路由器系统：OpenWrt（iStoreOS）

# 1. 修改光猫为桥接模式

这一步需要获取光猫超级管理员密码，有相同型号的同学可以翻翻站内的文章。这次发现超级密码被重置了，幸好之前打开过永久 Telnet 端口，重新获取一次就行。密码成了随机字符，不再是 telecomadmin 加几个数字了。

用超级密码登陆进去后，依次点击`网络`-`网络设置`-`网络链接`，按照下图修改保存。

![](https://s2.loli.net/2024/02/26/G8Y9F7xEBrkgIwZ.png)

# 2. 修改光猫 IPTV 绑定的网口
我家的路由器连接的是光猫的`网口1`，按照下图勾选。

![](https://s2.loli.net/2024/02/26/itw1WphjH2osFZ8.png)

# 3. 测试组播地址能否播放

家里任何一个设备，使用 PotPlayer 或 VLC 等播放器，看看能否播放`rtp://233.50.201.118:5140`这个地址。如果可以的话就看下一步。

# 4. 设置 udpxy

这里很简单，端口自己可以随意设置，不要占用其他应用的端口就行，这里以 9999 为例。我的 wan 口是 eth1，可安装实际使用的修改。

![](https://s2.loli.net/2024/02/26/CDq2jy769ibaJzY.png)

假设路由器的 IP 地址是 192.168.1.1，保存设置后打开 http://192.168.1.1:9999/status，查看 udpxy 是否成功开启。

# 5. 添加 M3U 播放源

播放源可以使用 [https://github.com/c1pher-cn/iptv-hangzhou-dianxin](iptv-hangzhou-dianxin) 这里的，需要根据自己路由器的 IP 地址进行修改。

播放器的话，安卓电视可以使用 Tivimate、Televizo 等，iOS 推荐使用 APTV。

# 6. 其他提示
如果只是在电脑上进行播放，或者使用支持播放 RTP 链接的播放器，其实就不需要路由器上面的设置了，直接把 m3u 播放列表中的链接修改成 rtp:// 格式，使用相应的播放器即可。