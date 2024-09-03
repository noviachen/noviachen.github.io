---
title: 使用 Docker 自建订阅转换 Subconverter + Subweb + MyUrls + Lukcy
tags:
  - Subconverter
  - Subweb
  - MyUrls
  - Lucky
  - Docker
cover: 'https://s2.loli.net/2024/09/03/AUVwQY1CEpP5Is6.png'
abbrlink: 950baac3
date: 2024-09-03 08:38:44
---


因为对机场自带的规则不太满意，同时每次更新订阅都要使用代理，所以想自己搭建一个订阅转换服务。本人也是小白，看了 [自建Clash订阅转换 - Subconverter+Subweb+MyUrls搭建教程 - 全docker完成 - 避坑指南](https://imgki.com/archives/718.html?replyTo=443) 这篇文件，摸索了好几次才搭建完成。这里给网友们提供一下简略版的搭建过程，一步步来就行，省了很多步骤。


本人使用 Lucky 提供反代服务，比较简单。如过使用宝塔或者其他，请自行搜索相关教程。

首先，添加三个域名，`sub-converter-api.your_name.com` 用于 Subconverter，`sub-converter.your_name.com` 用于 Subweb，`short.your_name.com` 用于 MyUrls，对应本地的端口号请看最下面的 docker compose 文件。域名请自定。

需要注意的是，MyUrls 域名请使用定制模式，开启跨域支持。

新建 convert 文件夹作为项目目录，相关文件都存储在这里。

```
mkdir convert && cd convert

```

拉取 sub-web 项目文件，并编辑

```
git clone https://github.com/CareyWang/sub-web.git 
cd sub-web

# 编辑 .env 配置文件
vi .env

# 手动修改下面三个配置
VUE_APP_SUBCONVERTER_DEFAULT_BACKEND = "https://sub-converter-api.your_name.com"
VUE_APP_MYURLS_API = "https://short.your_name.com/short"
VUE_APP_CONFIG_UPLOAD_API = "https://sub-converter-api.your_name.com"


# 编辑 Subconverter.vue
cd src/views
vi Subconverter.vue

# 修改第 235 行（当前是这一行），注意后面的 /sub? 需要保留
backendOptions: [{ value: "https://sub-converter-api.your_name.com/sub?" }],
```

补充一下，上面的 Subconverter.vue 可以自行添加规则，比如 ACL4SSR，在第 236 行（当前）`remoteConfig: [` 处回车，粘贴下面的代码即可。

```
{
            label: "ACL4SSR",
            options: [
              {
                label: "ACL4SSR_Online 默认版 分组比较全 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online.ini"
              },
              {
                label: "ACL4SSR_Online_AdblockPlus 更多去广告 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_AdblockPlus.ini"
              },
              {
                label: "ACL4SSR_Online_NoAuto 无自动测速 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_NoAuto.ini"
              },
              {
                label: "ACL4SSR_Online_NoReject 无广告拦截规则 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_NoReject.ini"
              },
              {
                label: "ACL4SSR_Online_Mini 精简版 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini.ini"
              },
              {
                label: "ACL4SSR_Online_Mini_AdblockPlus.ini 精简版 更多去广告 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_AdblockPlus.ini"
              },
              {
                label: "ACL4SSR_Online_Mini_NoAuto.ini 精简版 不带自动测速 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_NoAuto.ini"
              },
              {
                label: "ACL4SSR_Online_Mini_Fallback.ini 精简版 带故障转移 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_Fallback.ini"
              },
              {
                label: "ACL4SSR_Online_Mini_MultiMode.ini 精简版 自动测速、故障转移、负载均衡 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Mini_MultiMode.ini"
              },
              {
                label: "ACL4SSR_Online_Full 全分组 重度用户使用 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full.ini"
              },
              {
                label: "ACL4SSR_Online_Full_NoAuto.ini 全分组 无自动测速 重度用户使用 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full_NoAuto.ini"
              },
              {
                label: "ACL4SSR_Online_Full_AdblockPlus 全分组 重度用户使用 更多去广告 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full_AdblockPlus.ini"
              },
              {
                label: "ACL4SSR_Online_Full_Netflix 全分组 重度用户使用 奈飞全量 (与Github同步)",
                value:
                  "https://raw.githubusercontent.com/ACL4SSR/ACL4SSR/master/Clash/config/ACL4SSR_Online_Full_Netflix.ini"
              },
              {
                label: "ACL4SSR 本地 默认版 分组比较全",
                value: "config/ACL4SSR.ini"
              },
              {
                label: "ACL4SSR_Mini 本地 精简版",
                value: "config/ACL4SSR_Mini.ini"
              },
              {
                label: "ACL4SSR_Mini_NoAuto.ini 本地 精简版+无自动测速",
                value: "config/ACL4SSR_Mini_NoAuto.ini"
              },
              {
                label: "ACL4SSR_Mini_Fallback.ini 本地 精简版+fallback",
                value: "config/ACL4SSR_Mini_Fallback.ini"
              },
              {
                label: "ACL4SSR_BackCN 本地 回国",
                value: "config/ACL4SSR_BackCN.ini"
              },
              {
                label: "ACL4SSR_NoApple 本地 无苹果分流",
                value: "config/ACL4SSR_NoApple.ini"
              },
              {
                label: "ACL4SSR_NoAuto 本地 无自动测速 ",
                value: "config/ACL4SSR_NoAuto.ini"
              },
              {
                label: "ACL4SSR_NoAuto_NoApple 本地 无自动测速&无苹果分流",
                value: "config/ACL4SSR_NoAuto_NoApple.ini"
              },
              {
                label: "ACL4SSR_NoMicrosoft 本地 无微软分流",
                value: "config/ACL4SSR_NoMicrosoft.ini"
              },
              {
                label: "ACL4SSR_WithGFW 本地 GFW列表",
                value: "config/ACL4SSR_WithGFW.ini"
              }
            ]
          },
```

拉取 MyUrls 项目文件，无需编辑任何文件

```
cd ..
git clone https://github.com/CareyWang/MyUrls.git MyUrls
```

返回到 convert 文件夹，新建 docker-compose 文件

```
cd ..
vi docker-compose.yml
```

添加下面的内容并保存

```
version: '3.1'

services:
 
  subconverter:
    image: tindy2013/subconverter:latest # 拉取镜像，而不是自行 build
    container_name: subconverter
    restart: always
    ports:
      - 25500:25500

  subweb:
    build: ./sub-web # 自行 build 镜像
    container_name: sub-web-build
    restart: always
    ports:
      - 58080:80

  myurls:
    build: ./MyUrls
    container_name: myurls
    restart: always
    ports:
      - "58081:8080"
    volumes:
      - ./myurls/logs:/app/logs  
    depends_on:
      - myurls-redis
    entrypoint: ["/app/myurls", "-domain", "short.your_name.com", "-conn", myurls-redis:6379]

  myurls-redis:
    image: "redis:7"
    container_name: myurls-redis
    restart: always
    volumes:
      - ./data/redis:/data
    expose:
      - "6379"
```

之后使用 `docker-compose up -d`，等待完成。访问 https://sub-converter.your_name.com 测试是否成功。