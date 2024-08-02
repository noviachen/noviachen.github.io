---
title: 解决 Mac 上使用 Requests-HTML 时出现证书错误
tags:
  - Requests-HTML
  - Cerificate
cover: https://i.loli.net/2020/10/07/j23spZRJvg15zXM.jpg
abbrlink: 6db264f8
date: 2018-07-21 10:50:38
---

Requests-HTML 支持 Javascript，不过是通过 Chromium 执行。第一次运行的时候会自动进行下载，不过需要科学上网。

在 Windows 上可以下载成功，但 Mac 上却总是失败，提示 `SSL: CERTIFICATE_VERIFY_FAILED`
<!--more-->

```bash
[W:pyppeteer.chromium_downloader] start chromium download.
Download may take a few minutes.
Traceback (most recent call last):

  ···

ssl.SSLError: [SSL: CERTIFICATE_VERIFY_FAILED] certificate verify failed (_ssl.c:777)
```

找了好久，终于在下方文章中找到了解决办法。

> 作者：Tim Kamanin
> 标题：Fixing CERTIFICATE_VERIFY_FAILED error when trying requests-html out on Mac
> 地址：[https://timonweb.com/tutorials/fixing-certificate_verify_failed-error...](https://timonweb.com/tutorials/fixing-certificate_verify_failed-error-when-trying-requests_html-out-on-mac/)

根据博主的说明，出现问题是由于 Python 3.6 使用了自己的 SSL 证书，而不使用系统自带的证书。只要安装 `certifi` 就能解决，主要有两种方法：

+ 通过 pip 安装
```bash
pip install --upgrade certifi
```

不过只出现下方的提示，实际上并没有成功

```bash
Requirement already up-to-date: certifi in /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages
```

+ 运行 `Cerificates.command` 命令

```bash
open /Applications/Python\ 3.6/Install\ Certificates.command
```

这次就成功了

```bash
/Applications/Python\ 3.6/Install\ Certificates.command ; exit;
 -- pip install --upgrade certifi
Requirement already up-to-date: certifi in /Library/Frameworks/Python.framework/Versions/3.6/lib/python3.6/site-packages
You are using pip version 9.0.1, however version 10.0.1 is available.
You should consider upgrading via the 'pip install --upgrade pip' command.
 -- removing any existing file or link
 -- creating symlink to certifi certificate bundle
 -- setting permissions
 -- update complete
logout
Saving session...
...copying shared history...
...saving history...truncating history files...
...completed.
Deleting expired sessions...10 completed.

[进程已完成]
```

至此，完美解决，继续愉快的使用吧。