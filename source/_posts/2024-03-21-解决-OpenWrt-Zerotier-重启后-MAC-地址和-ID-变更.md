---
title: 解决 OpenWrt Zerotier 重启后 MAC 地址和 ID 变更
tags:
  - OpenWrt
  - Zerotier
cover: 'https://s2.loli.net/2024/03/13/qmulHhQ3KNLpdYP.png'
abbrlink: 9e6b4afd
date: 2024-03-21 19:16:37
---

最近发现主路由重启之后，无法使用 Zerotier 远程连接了，然后看到 Zerotier Central 中多了好几个新的设备。再次重启了才发现，原来是重启之后 MAC 地址和 ID 变化导致的。

查了一些资料，发现是 zerotier 的配置文件有点问题，下面来手动修改一下。

使用 WinSCP 等工具，进入 /etc/config，修改`zerotier`这个文件。在最下面增加一行配置，如下图所示。

```bash
option secret 'generate'
```

![](https://s2.loli.net/2024/03/13/eSGiso6xRrWfvdL.png)

重启路由器。再次打开上面修改过的文件，看到内容已经变了，增加了一长串字符，说明成功了。再次重新路由器试试，可以看到 MAC 地址和 ID 不会变化了。

![](https://s2.loli.net/2024/03/13/vDnrsVkHRoeXZOz.png)
