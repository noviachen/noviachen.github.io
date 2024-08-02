---
title: 咕咕机打印下厨房菜谱 —— RecipeBird 菜谱机
tags:
  - Python
  - Flask
  - 咕咕机
  - 下厨房
cover: https://i.loli.net/2020/10/07/Z9aKu1NtcvyGEhr.png
abbrlink: a3c10d66
date: 2019-04-06 08:50:29
---


有时自己在家做饭的时候需要看菜谱，但是直接用手机有点不方便，又没有高大上的油烟机自带屏幕，所以就想到了用咕咕机打印出来。一开始是复制网页版上的文字再打印，但是实在是麻烦，而且作为一个有点皮毛的技术男实在不能允许只能纯手工打印。所以 RecipeBird 菜谱机就诞生了！

<!--more-->

其实 2018 年就已经在开发了。一开始是自己对照着官方 API 文档自己写 Python API，但是总是有问题，老是重复打印内容，换行也不行，最糟糕的是不知道哪里出了问题。后来也比较忙，就搁置了。

最近又想到了它，决定还是用别人造好的轮子吧，看了好几个，觉得还是 pymobird 最好用，也是最新更新的。还是不用自己造轮子简单啊，一会就搞定了，哈哈哈。

我已经部署到 Heroku 上，访问  <https://recipebird.herokuapp.com/> 即可直接使用。

![](https://i.loli.net/2020/10/07/g9YMx5XRP4vkpoH.jpg)

如果需要部署到自己的服务器或者 Heroku 上， 请前往本项目 Github 自行操作，项目地址 <https://github.com/noviachen/RecipeBird>。

iOS 用户也可以使用 Workflow (快捷指令)，前往 [https://www.icloud.com/shortcuts/...](https://www.icloud.com/shortcuts/37700b397af84ea19593ccb384b7cb26) 进行下载，里面有详细的使用说明。

![](https://i.loli.net/2020/10/07/OMxYAF9d17vflJb.jpg)
