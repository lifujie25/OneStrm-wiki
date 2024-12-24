![image](https://github.com/user-attachments/assets/0b19086c-b443-4330-934c-c78bd9eb77ef)# OneStrm-wiki

## 概述

**OneStrm** 融合ONEemby_302、ONEemby_strm的全部功能，并重新梳理程序逻辑和稳定性，更新的UI，实现一个软件即可生成strm和302播放、获取cookie； 程序是在p115的基础上二次开发，建议有能力的大佬们直接使用原生代码，tg@operate115

**为防止滥用，本工具带有授权机制，安装后自行授权（介意绕道），已授权ONEemby_302和ONEemby_strm可联系更换激活码**

tg交流群：[TG交流群]((https://t.me/OneStrm)/⁠)

docker hub地址：https://hub.docker.com/r/lifj25/onestrm

建议结合emby插件：[strm助手](https://github.com/sjtuross/StrmAssistant/⁠)

## 功能说明

配置见[Github Wiki](https://github.com/lifujie25/OneStrm-wiki/wiki#onestrm%E9%85%8D%E7%BD%AE)

### 一、Cookie功能

1. **页面扫码获取cookie** ：可以直接在页面上扫码获取 115 各种端的 cookie，并自动填入

2. **同个115网盘多个cookie**：如多cookie-1掉了，自动切换cookie-2、以此类推（推送通知可直接扫码更新）

3. **支持生成带KID的cookie**

### 二、通知功能

1. **3种通知方式可选**：企业微信应用（代理搭建https://hub.docker.com/r/lifj25/wxchat）、企业微信机器人、Telegram机器人
2. **自定义通知**：执行成功通知、CD2掉盘通知
3. **cookie无效通知**：默认开启cookie无效推送通知，可根据喜好是否需要同步推送一个二维码

### 三、Strm功能

1. **快速拉取网盘文件 **：一次性拉取网盘文件到数据库，第一次拉取速度比较慢，后续更新比较快
2. **生成strm**：根据数据库内容，生成 strm，可自定义前缀或者生成pickcode（推荐）；
3. **自定义下载元数据**：字幕文件、图片文件、其他刮削文件，并可指定树全盘下载还是指定目录下载（建议生成 strm 后本地刮削）；
4. **定时更新**：当前程序有增量更新、全量更新，设置时间后，自己循环更新，做到剧等人
5. **实时监控**：配合cd2，实现实时监控，并生成strm，已重新梳理逻辑，收集3分钟推送一次，超过10个增量拉全库

### 四、302播放功能

1. **网盘+本地库共用**：支持pickcode和路径替换，并支持同个emby存在网盘和本地媒体，自动识别方式，优先推荐strm生成pickcode模式； 

2. **直链播放日志**：是否直链播放,在 web 端可以查看对应日志； 

3. **多版本选择**：修复原ONEemby_302同部电影多个版本无法选择的问题

## 安装脚本

下面仅贴出两种安装方式，对docker熟练掌握的可直接依据两种方式选其一搭建，若对docker不熟悉，建议参考[Wiki](https://github.com/lifujie25/OneStrm-wiki/wiki#onestrm%E9%83%A8%E7%BD%B2)

### Docker CLI方式

```bash
#需要用bridge桥接的网络方式，不要用 host

docker run -d \
  --name onestrm \
  --restart always \
  -p 18003:18003 \ #web管理页面
  -p 18302:18302 \ #302播放地址
  -v 本地保存配置文件路径:/app/config \ # 保存配置文件，左侧路径可自行更改
  -V 本地保存strm和刮削路径:/movie_strm \  # 保存strm 和元数据路径，左右侧路径可自行更改,请设置写入权限
  -e PUID=0
  -e PGID=0
  lifj25/onestrm:latest
```

### Docker compose方式

```yaml
#需要用bridge桥接的网络方式，不要用 host
version: '3.8'

services:
  onestrm:
    image: lifj25/onestrm:latest
    container_name: onestrm
    restart: always
    ports:
      - "18003:18003"  # web管理页面
      - "18302:18302"  # 302播放地址
    volumes:
      - ./config:/app/config  # 保存配置文件，左侧路径可自行更改
      - 本地保存strm和刮削路径:/movie_strm  # 保存strm 和元数据路径，左右侧路径可自行更改,请设置写入权限
    environment:
      - PUID=0
      - PGID=0
```

