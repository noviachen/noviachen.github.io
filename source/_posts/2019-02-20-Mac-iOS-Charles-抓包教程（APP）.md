---
title: Mac + iOS + Charles 抓包教程（APP）
tags:
  - Mac
  - iOS
  - Charles
cover: https://i.loli.net/2020/10/07/TV3i7FzaJ9MkOYc.png
abbrlink: 44fa3337
date: 2019-02-20 20:52:39
---


Charles 是有名的抓包软件，我觉得比 Fiddler 好使，至少界面更加直观，使用也更方便。这段时间一直用它来抓包 APP，所以就来记录一下使用的方法。

本文使用的是 Mac 和 iOS，其实 Windows 或 Android 的组合也是基本通用的。

目前最新版是 4.2.7，Windows / Mac OS / Linux 全平台可用。未注册的版本，每隔 30 分钟会自动关闭，不太影响使用便是。注册很简单，大家自行 Google。

<!--more-->

![](https://i.loli.net/2020/10/07/5cCsNMfKWe9hE4n.jpg)

# 关闭拦截 Mac 网络请求

我们的目的是截取 iOS APP的网络请求，所以不要有电脑的网络请求来影响我们的分析，需要取消 Proxy -> MacOS Proxy 的勾选。

![](https://i.loli.net/2020/10/07/QyYz6xc1lX7wCkd.jpg)

# 配置端口号

Proxy -> Proxy Settings 中设置端口号，并勾选 `Enable transparent HTTP proxying`。这里保持默认的 8888 端口。

![](https://i.loli.net/2020/10/07/KnAqpByQ8hwj6Xk.jpg)

# 配置 SSL 代理

Proxy -> SSL Proxy Settings 中点击 Add 添加 443 端口，否则无法查看 HTTPS 的请求内容，会显示向上的蓝色箭头。

![](https://i.loli.net/2020/10/07/hKm2aQcxDeEVwJB.jpg)

![](https://i.loli.net/2020/10/07/Z921tTkSbRmLcDg.jpg)

# 在 Wi-Fi 中设置代理

![](https://i.loli.net/2020/10/07/KTVGOxa6Z8przNt.jpg)

# 手机上安装 SSL 证书

设置完代理后，Charles 会出现一个对话框，点击“允许”（Allow）以允许设备接入 Charles。

![](https://i.loli.net/2020/10/07/z3L2botr8usTVhI.jpg)

之后在浏览器中打开 chls.pro/ssl 安装证书。如果没有弹出以下界面，请确认 Wi-Fi 代理是否设置正确。

![](https://i.loli.net/2020/10/07/oOXZQm5YwMh4LN6.jpg)

![](https://i.loli.net/2020/10/07/HgPNE9ebwjCdqTr.jpg)

最后这一步也很重要，不要忽略。在 iOS 的 设置 -> 通用 -> 关于本机 -> 证书信任设置 中信任新安装的证书。

![](https://i.loli.net/2020/10/07/UTnSlCMYGkLNvzf.jpg)

至此，愉快的抓包吧，啦啦啦 ＼(☆o☆)／