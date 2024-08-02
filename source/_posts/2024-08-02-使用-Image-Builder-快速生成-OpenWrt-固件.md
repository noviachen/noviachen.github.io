---
title: 使用 Image Builder 快速生成 OpenWrt 固件
tags:
  - Github Action
  - Openwrt
  - ImmortalWrt
  - 云编译
cover: 'https://s2.loli.net/2024/08/02/dOnmMUX2WRo8qQw.png'
abbrlink: fcde2f81
date: 2024-08-02 16:06:31
---

之前编译固件一直使用的是 P3TERX 的 [Actions-OpenWrt](https://github.com/P3TERX/Actions-OpenWrt)，大概需要花费一个小时到一个半小时。后面使用了缓存 [cachewrtbuild](https://github.com/stupidloud/cachewrtbuild)，时间能控制在半个小时以内。但是还是觉得慢了一点，有时候生成的固件有问题，重新生成一个又需要等半个小时。

再后面就是发现了 Image Builder，比官方的 firm selector 定制程度高一点，可以加入一些第三方的插件。搜了网上的教程，基本上都是在机器上自己编译的，用虚拟机或者实体机，看着输入一堆命令就头疼。所以还是想到了用 Github Action 来编译吧。

网上搜了一些，好像没有找到比较简单的教程，也可能是自己的搜索方式有问题，所以还是自己重新做一个吧。仓库地址：[Image-Builder](https://github.com/noviachen/Image-Builder)，自认为使用方法比较简单。

1. 使用右上角 Use this template 按钮复制到自己的仓库中。
2. 默认使用的是 ImmortalWrt 23.05.3 (r27917-81a1f98d5b)，需要更换版本的话，修改 `.github/workflows/image-builder.yml` 文件中的 `DOWNLOAD_URL` 参数就可以了。Openwrt 官方版本没测试过，理论上是可以的。
3. 根据自己的设备，修改上述文件中的 `PROFILE`。
4. 修改 `uci-custom` 和 `packages.list` 两个文件。一个是首次启动脚本，比如设置 LAN IP、Wifi、PPPOE 等，需要的话，取消注释即可。另外一个文件是需要添加/或删除的软件包。
5. 其他官方没有的软件包，可以把 ipk 文件上传到 `packages` 文件夹。
6. 点击上方的 Action 页面标签，选择 ImmortalWrt Image Builder，点击右边的 Run workflow 按钮。等待固件生成。
7. 固件生成后，在 Artifacts 标签下面可以找到生成的固件，点击下载即可。
