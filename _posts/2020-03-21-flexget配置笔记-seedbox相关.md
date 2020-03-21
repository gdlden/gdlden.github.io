---
layout:     post
title:      flexget配置笔记-seedbox相关
subtitle:   是该好好写点东西了。。。
date:       2020-03-21
author:     hukss
header-img: img/flexget.jpg
catalog: true
tags:
    - seedbox
---
星大脚本装好后的webui端口：
ip：6566
配置实例
```
# Here are some guides
#
# https://ymgblog.com/2018/04/30/396/
# https://npchk.info/linux-flexget-rss/
# https://linkthis.me/2018/02/15/the-note-of-using-flexget/
#
# https://github.com/Aniverse/WiKi/blob/master/Flexget.md
# https://github.com/Aniverse/WiKi/blob/master/How.to.use.RSS.md#flexget-rss
#
# For more usages, check the offical site: https://flexget.com

templates:
  freespace:
    free_space:
      path: /home/hukss
      space: 10240
  qb:
    qbittorrent:
      path: /home/hukss/qbittorrent/download/
      host: localhost
      port: 2017
      username: hukss
      password: 
  tr:
    transmission:
      path: /home/hukss/transmission/download/
      host: localhost
      port: 9099
      username: hukss
      password: 
  de:
    deluge:
      path: /home/hukss/deluge/download/
      host: localhost
      port: 58846
      username: hukss
      password: 
  size:
    content_size:
      min: 6000
      max: 666666
      strict: no
tasks:
  avz:
    limit:
      amount: 3
      from:
        rss: 
    accept_all: yes
    content_size:
      min: 30
      max: 512000
      strict: no
    template:
      - de
    deluge:
      label: pth
      # Limit upload speed to 100 MiB/s in case of being auto-banned
      max_up_speed: 102400
      max_down_speed: 20480
web_server:
  port: 6566
  web_ui: yes
# This is prepared for reverse proxy, do not uncomment it unless you know how it works
# base_url: /flexget

# schedules is disabled by default, you need to enable it or use cron to RSS
schedules: 
  - tasks: '*'
    interval:
      minutes: 3
```

cron方式：
config.yml文件位于
/root/.config/flexget/config.yml

编辑成
```
schedules: no
```
执行
`flexget check`

检查语法错误等待通过之后
`flexget --learn execute`

然后执行
`crontab -e`

添加
`*/3 * * * * /usr/local/bin/flexget --cron execute`
<-完->
