---
title: 我在为 redis 镜像指定启动配置文件时遇到的一个问题
date: 2017-02-08 14:15:23
categories:
- 运维
tags:
- redis
- docker
---
启动 redis 镜像时指定的配置文件 `redis.conf` 中不能可以配置 `daemonize yes`。
<!-- more -->
## 问题

年前在测试机上我就尝试指定外部配置文件给 redis 容器直到昨天。一开始我以为是 `-v` 挂载文件出了问题。

如下的启动参数：

```
docker run -v /docker/redis:/data --name my-redis -d redis redis-server /data/redis.conf
```

在此之前，我已经创建了 `/docker/redis/` 目录并且给出了 `redis.conf` 在这个目录中。以为我怀疑是 `-v` 挂载出了问题，所以我又故意给错配置文件名，将 `redis.conf` 写成 `redis.conf1`, 再次运行如下命令：

```
docker run -v /docker/redis:/data --name my-redis -d redis redis-server /data/redis.conf1
```

报错了，提示找不到 `redis.conf1`。的确在我给出正确的配置文件时，没有报错，no news is good news？到这时我才怀疑到是不是 `redis.conf` 的配置出了问题。经过几次尝试确定了问题出在 `daemonize yes` 这个配置项。

## 启示

一直找不出原因时，我寄希望于在 segmentfault 的[提问](https://segmentfault.com/q/1010000008272753?_ea=1600910)，就在编写并试图说清楚问题时，我做了以上的尝试。这再次证明了提问也是一个整理思路的过程。这里我又想到了[提问的智慧](http://www.jianshu.com/p/60dd8e9cd12f)。

## 感谢

这里我要感谢[东方星痕](lxy520.net)的帮助，谢谢你的解答。
