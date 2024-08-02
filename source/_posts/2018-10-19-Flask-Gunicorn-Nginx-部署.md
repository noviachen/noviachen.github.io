---
title: Flask + Gunicorn + Nginx 部署
tags:
  - Python
  - Flask
  - Gunicorn
  - Nginx
cover: https://i.loli.net/2020/10/07/iJvMmQ2cfOLXkHY.png
abbrlink: 5f0d3a0
date: 2018-10-19 20:45:07
---

以前刚开始折腾用的是 uWSGI + Supervisor，没成功。又发现了 Gunicorn，虽然瞎折腾成功了，但是搞不懂为什么，因为老是没法开机启动，后来又突然可以了。现在才知道，原来是 LBSInitScript 中的 `Required-Start` 没写好造成的。

<!--more-->

我用的是 Debian 7.0 x86，自带 Python 2.7，手动安装了 Python 3.6。下面是我的系统环境：

- Debian 7.0 x86
- Python 3.6
- [LNMP 一键安装包](https://lnmp.org/)

下面以添加网站 `www.example.com` 为例：

1、使用 root 账户登陆，通过 `lnmp vhost add` 新建虚拟主机，具体操作请看[LNMP添加、删除虚拟主机及伪静态使用教程](https://lnmp.org/faq/lnmp-vhost-add-howto.html)。

2、添加虚拟主机之后会自动创建网站目录 `/home/wwwroot/www.example.com/`，然后将项目文件上传。

3、修改 Nginx 的配置 `/usr/local/nginx/conf/vhost/www.example.com.conf`

```nginx
server
    {
        listen 80;
        #listen [::]:80;
        server_name www.example.com example.com;
		
        location / {
                proxy_pass http://127.0.0.1:1234; # 与 Gunicorn 的通信端口
                proxy_redirect     off;
                proxy_set_header   Host                 $http_host;
                proxy_set_header   X-Real-IP            $remote_addr;
                proxy_set_header   X-Forwarded-For      $proxy_add_x_forwarded_for;
                proxy_set_header   X-Forwarded-Proto    $scheme;
        }
 }
```

4、重启 Nginx 服务

```bash
/etc/init.d/nginx restart
```

5、新建虚拟环境，并安装第三方库，包括 Gunicorn

```bash
cd /home/wwwroot/www.example.com/
pip3 install virtualenv
virtualenv -p python3 venv
source venv/bin/activate
pip install gunicorn
pip install -r requirements.txt # 已建立 Python 3 环境，所以不用 pip3 了
deactivate
```

6、在 `/etc/init.d` 新建 Gunicorn 开机启动脚本，如 `example.sh`

```bash
 #!/bin/bash
### BEGIN INIT INFO
# Provides:          uznEnehC
# Required-Start:    $network $remote_fs $local_fs
# Required-Stop:     $network $remote_fs $local_fs
# Default-Start:     2 3 4 5
# Default-Stop:      0 1 6
# Short-Description: Flask
# Description:       Flask Service
### END INIT INFO
cd /home/wwwroot/www.example.com/
venv/bin/gunicorn -w 4 -b 127.0.0.1:1234 main:app # 端口与上面对应
exit 0
```

7、给予脚本相应的权限

```bash
chmod 777 /etc/init.d/example.sh
```

8、设置开机启动

```bash
update-rc.d example.sh defaults
# update-rc.d -f example.sh remove # 删除开机启动，添加 -f 参数强制删除
```

重启服务器试试，这个时候就可以访问了。