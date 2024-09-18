---
title: 在 1Panel 中使用 Docker-Compose 部署 Flask 应用
tags:
  - 1Panel
  - Docker-Compose
  - Flask
  - Python
cover: 'https://s2.loli.net/2024/09/18/qHDKfW2e7vRkwJF.jpg'
abbrlink: 90c8a838
date: 2024-09-18 16:00:23
---

一直想把面板从宝塔换成 1Panel，担心宝塔有后台，而且老是跳出来要绑定服务器，实在是太麻烦了。因为每隔几天就换了 IP，都需要重新绑定。但是 1Panel 一直未支持 Python，所以搁置了很久。最近注意到可以使用 Docker 部署 Python 项目，虽然稍微麻烦点，但是基本上部署之后不会再动了，还能接受。

下面是一个简单的 Flask 应用，以及项目的目录结构，文件位于 /opt/1panel/docker/compose/flask-server。以下内容请根据实际的项目进行更改。

```Python
# app.py

from flask import Flask

app = Flask(__name__)

@app.route("/")
def hello_world():
    return "<p>Hello, World!</p>"
```

```
│
├─flask-server
│  ├─app
│  │  ├─app.py
│  │  ├─requirements.txt
```

需要注意的是，请在 requirements.txt 中增加两个包：`gunicorn` 和 `gevent`。

然后，在 flask_server 文件夹中新建 `Dockerfile` 文件，示例内容如下：

```bash
# 使用 Python 官方的 Docker 镜像作为基础镜像
FROM python:3.9-alpine

# 设置工作目录
WORKDIR /app

# 复制应用代码到镜像中的 /app 目录
COPY ./app /app

# 修改时区
RUN cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime && echo "Asia/Shanghai" > /etc/timezone

# 安装应用依赖
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --no-cache-dir -r requirements.txt

# 设置启动命令
CMD gunicorn app:app -b :5000 -k gevent --log-level info
```

接着，在 `容器 -> 编排模板` 中创建新的编排模板，名字可自定。

```
version: '3'
services:
  # 容器服务名称
  flask-server:
    # 容器名称
    container_name: flask-server
    build:
      #在当前目录下寻找Dockerfile文件并构建镜像
      context: .
      dockerfile: Dockerfile
      # 重启策略
    restart: always
    # 使用1Panel的网络方便容器间通信
    networks:
      - 1panel-network
    # 挂载目录 本地化容器数据
    # 这里挂载了本地当前目录的app目录到容器的/app目录
    volumes:
      - ./app:/app
 
    # 端口映射 容器端口映射到主机端口
    ports:
      - "5000:5000"
 
networks:
  1panel-network:
    external: true
```

最后，到 `编排` 中创建编排，选择使用编排模板。请注意，文件夹名称一定要与项目文件夹一致，比如这里需要设置成 flask-server

![new_compose.png](https://s2.loli.net/2024/09/18/SOFU2J7RAiYT3KB.png)

点击确认按钮，等待完成。如果最后提示`docker-compose up successful!`，访问 IP:端口号 即可看到 Hello, World! 的内容。



