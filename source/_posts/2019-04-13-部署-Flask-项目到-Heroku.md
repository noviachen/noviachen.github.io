---
title: 部署 Flask 项目到 Heroku
tags:
  - Flask
  - Heroku
cover: https://i.loli.net/2020/10/07/7IUoOx2B8ayNRXs.jpg
abbrlink: b4cb2e1c
date: 2019-04-13 21:35:45
---


以前都是直接在 VPS 上部署 Flask 项目的，麻烦不说，还容易弄错，查来查去还是运行不起来。自从用了 Heroku，顿时觉得发现了新世界，好好用啊！

<!--more-->

1、首先需要注册 Heroku 账号，然后新建一个 app，这个自不必多说。

2、上传到 Heroku 之前，咱们需要在项目根目录增加两个文件：

- Requirements.txt

  这个文件大家都很清楚，是本项目依赖的库。可以这么写：

  ```
  Flask==1.0.2
  gunicorn==19.9.0
  ```

  **注意，依赖库需要增加 gunicorn 来帮助项目运行，当然可以选择其他的，只是我觉得 gunicorn 比较好使。**

  还可以这么写，像我这样想偷懒的：

  ```
  Flask
  gunicorn
  ```

  项目上传到 Heroku 之后会自动安装相关的依赖库。

- Procfile

  这个就是咱们 Flask 项目的启动命令

  ```
  web: gunicorn run:app
  ```

  这里需要注意一下，如果你的主程序是 `run.py` 那么上面这么写是可以。咱们换一个名字，如果是 `main.py` 那就是下面这样，请大家注意。
  
	```
  web: gunicorn main:app
	```

3、Heroku 支持三种上传模式，推荐使用关联 Github 的方式进行上传。

首先搜索项目，点击右边的 Connect 进行关联。

![](https://i.loli.net/2020/10/07/htbzD2PTonNWyHG.jpg)

之后可以选择 Enable Automatic Deploys 进行自动上传，在项目产生新的提交之后。咱们现在需要手动上传，点击下面的 Deploy Branch 即可，会自动安装并运行 app。之后打开 https://your-app-name.herokuapp.com 就可以看到你的项目啦。

![](https://i.loli.net/2020/10/07/ECbTk37lRJeWyAH.jpg)


