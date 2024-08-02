---
title: OpenWrt 安装 Bandwidthd PostgreSQL 详细记录
tags:
  - OpenWrt
  - Bandwidthd
  - PostgreSQL
  - 宝塔面板
cover: 'https://s2.loli.net/2024/03/31/8jLc4G2ONhlPnBo.png'
abbrlink: 75939a2b
date: 2024-04-01 09:07:39
---


想找一个 OpenWrt 可以使用的,并且能根据 IP 统计流量的插件，主要看中了 nlbwmmon 和 Bandwidthd 两种。前者统计比较粗略，只有统计期间内的流量合计，没法查看各个期间的使用情况，最终还是决定使用 Bandwidthd。

因为担心重启或者升级后数据丢失，选择了 PostgreSQL 数据库版本，PHP 网页服务端也分开部署，修改起来也方便。不过只找到了 OpenWrt 官方的文档，没有其他资料可以参考，所以踩了不少坑。这里记录一下，方便同样有需求的朋友。

本人技术小白，主要使用宝塔面板来安装，可视化操作更方便。

# 安装和配置 PostgreSQL 数据库

请安装 PostgreSQL v11 版本，v12 及更高版本会报错。同时开启远程访问，修改`postgresql.conf`和`pg_hba.conf`两个文件就行。宝塔面板可在 PostgreSQL 管理器中修改，截止目前 2.5 版本有 bug，无法保存设置，我使用的是 2.3 版本。

然后新建数据库，可以使用命令行。比如，用户名`postgres`，数据库名称`bandwidthd`，密码`12345`。

使用宝塔面板请注意，最好不要在  PostgreSQL 管理器中间新增数据库！不然数据库页面中找不到，后面也无法使用计划任务来备份。

之后是初始化数据表，从 https://github.com/NethServer/bandwidthd 下载`schema.postgresql`这个文件，假如上传到`/www/wwwroot`，之后终端运行：

```bash
su - postgres

psql bandwidthd postgres < /www/wwwroot/schema.postgresql
```

如果提示类似下面的结果，表示成功了。

```bash
root@Debian:~# su - postgres
$ psql bandwidthd postgres < /www/wwwroot/schema.postgresql
CREATE TABLE
CREATE INDEX
CREATE INDEX
CREATE TABLE
CREATE INDEX
CREATE INDEX
CREATE TABLE
CREATE INDEX
CREATE TABLE
CREATE INDEX
CREATE TABLE
```

打开 PgSQL 工具箱查看，发现多了几张表，表明成功了。

![](https://s2.loli.net/2024/03/20/V47o1wnsQkuv93U.png)

# 安装和配置 bandwidthd-pgsql

LEDE 可以在编译的时候直接加到固件中，x86_64 版本的 ipk 可以从 https://archive.openwrt.org/releases/23.05.2/packages/x86_64/packages/ 这个网址下载，其他版本可在上述网站的其他文件夹中找到。

假如把 bandwidthd-pgsql_2.0.1-35-7_x86_64.ipk 上传到 /tmp 目录，运行以下命令：

```bash
cd /tmp
opkg install bandwidthd-pgsql_2.0.1-35-7_x86_64.ipk

# 安装完成后再运行下面的命令
/etc/init.d/bandwidthd enable
/etc/init.d/bandwidthd start
```

之后是修改`etc/config/bandwidthd`配置文件，这里就不再赘述了，可参考官方文档（在文章的最后）。重点注意`option pgsql_connect_string`，官方文件写得不是很清楚，请参考下面我的配置。如果使用默认端口，端口号可以不用写。

```bash
config bandwidthd
        option dev      br-lan
        option subnets          "192.168.1.0/24"
        option skip_intervals   0
        option graph_cutoff     1024
        option promiscuous      true
        option output_cdf       false
        option recover_cdf      false
        option filter           ip
        option graph            false
        option meta_refresh     150
        option pgsql_connect_string    "user = postgres password = 12345 dbname = bandwidthd host = 192.168.1.1 port = 8888"
        option sensor_id       "openwrt-lede"
```

# 安装和配置 PHP 服务端

请安装 PHP v7.0 版本，更高版本会报错，有些函数不支持。然后安装扩展`pgsql`，宝塔面板可在 PHP 设置中搜索安装 。最后重启 PHP。


从 https://github.com/NethServer/bandwidthd 下载`phphtdocs`文件夹中的内容，并将上传到服务器。修改`config.conf`，如下面所示：

```bash
<?
define("DFLT_WIDTH", 900);
define("DFLT_HEIGHT", 256);
define("DFLT_INTERVAL", INT_DAILY);

$db_connect_string = "user = postgres password = 12345 dbname = bandwidthd host = 192.168.1.1 port = 8888";

```

又有需要注意的了，请下载`tag 2.01`，不要下载`tag 2.01-34`和`tag 2.01-35`。如果怕搞错，从 https://github.com/NethServer/bandwidthd/tags 这个网址下载吧。

最后，刷新网页，应该就能看到数据了。下面是网上找的图片，可供参考。

![](https://bandwidthd.sourceforge.net/bandwidthd-top2.png)

![](https://bandwidthd.sourceforge.net/bandwidthd-graphs.png)

![](https://bandwidthd.sourceforge.net/bandwidthd-search.png)


> 参考文章：
> [OpenWrt Wiki Bandwidthd](https://openwrt.org/docs/guide-user/services/network_monitoring/bandwidthd)
