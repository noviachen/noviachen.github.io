---
title: macOS 下恢复 Hexo 博客
tags:
  - hexo
cover: https://i.loli.net/2021/05/18/JyVe4iDuXAvOjrt.jpg
abbrlink: 9644ce66
date: 2021-03-18 22:48:30
---


上次说因为 Wi-Fi 的问题重装了系统，导致 Hexo 也要重新安装了，网上找了一些资料，记录了一下如何重装。

<!--more-->

1. 安装 Homebrew

   ```bash
   # 使用官方安装脚本
   /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
   
   # 使用国内安装脚本
   /bin/zsh -c "$(curl -fsSL https://gitee.com/cunkai/HomebrewCN/raw/master/Homebrew.sh)"
   ```

2. 安装 Node.js 

   ```bash
   brew install node
   ```

3. 安装 Hexo

   ```bash
   npm install -g hexo-cli
   ```

4. 新建新的博客文件夹 myblog

    ```bash
    # 在当前用户的文件夹下新建博客文件夹，当然你可以选择其他位置
    cd ~
    mkdir myblog
    ```

5.  初始化 Hexo

   ```bash
   hexo init
   ```

6. 将原博客文件的将原始博客的 source、themes 和 scaffolds 文件夹，_config.yml、_config.butterfly.yml（如有）和 package.json 文件拷贝至新的博客文件夹内并进行替换。

   > source 文件夹内包含了之前撰写的博文
   > themes 文件夹内是原始博文的主题文件
   > scaffolds 文件夹内是一些博文模板，如果之前有修改过模板，例如加入了博文密码、评论开关、置顶控制等相关字符，就需要保留这个文件夹
   > _config.yml 是博客配置文件
   > _config.butterfly.yml 为 Buttrefly 主题配置文件
   > package.json 文件是原博客中安装的插件列表

7. 安装原博客的插件

   ```bash
   npm install
   ```

8. 上传到 Github。这里会提示输入 Github 的用户名和密码，上传成功后博客完成迁移。

   ```bash
   hexo g #编译
   hexo d #上传
   ```



>参考文章：[macOS下Hexo博客恢复指南](<https://cxjiang.top/2018/04/03/mac-hexo-reinstall/>)