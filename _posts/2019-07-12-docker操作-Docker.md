---
layout:     post
title:      Docker基本命令
subtitle:   Docker基本命令
date:       2019-07-12
author:     hukss
header-img: img/post-bg-universe.jpg
catalog: true
tags:
    - Docker
---
##docker 操作命令

#启动交互式容器
```
docker run -i -t IMAGE /bin/bash

```
参数说明：
-i --interactive=true|false 默认是false
-t --tty=true|false 默认是false

#查看容器
```
docker ps [-a][-l]
```
参数说明：
-a 列出所有容器
-l 列出最新的一个容器

#查看容器详细说明
```
docker inspect 
```
参数:
接上pid或者容器名字

#重新启动已经停止的容器
```
docker start -i Name
```

#删除已经停止的容器
```
docker rm 容器名
```

#守护式容器
在运行交互式容器的时候，按住Ctrl+P Ctrl+Q退出中端

```
docker run -d 镜像名 
```
即：加上-d参数则会使用后台的方式启动容器