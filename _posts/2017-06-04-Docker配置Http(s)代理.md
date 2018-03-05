---
layout:     post                    # 使用的布局（不需要改）
title:      Docker配置Http(s)代理               # 标题 
subtitle:    #副标题
date:       2017-08-06              # 时间
author:     Roy                      # 作者
header-img: img/post-bg-2015.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
    - Docker
    - Linux
---

1. 为docker服务创建一个内嵌的systemd目录
```
$ mkdir -p /etc/systemd/system/docker.service.d
```
2. 创建/etc/systemd/system/docker.service.d/http-proxy.conf文件,在文件中添加HTTP_PROXY环境变量
3. [proxy-addr]和[proxy-port]分别改成实际情况的代理地址和端口, 如果还有内部的不需要使用代理来访问的Docker registries，那么还需要指定NO_PROXY环境变量：
```
[Service]
Environment="HTTP_PROXY=http://[proxy-addr]:[proxy-port]/" "HTTPS_PROXY=https://[proxy-addr]:[proxy-port]/"
Environment="HTTP_PROXY=http://[proxy-addr]:[proxy-port]/" "HTTPS_PROXY=https://[proxy-addr]:[proxy-port]/" "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```
4. 更新配置：
```
$ systemctl daemon-reload
```
5.  重启Docker服务：
```
$ systemctl restart docker
```
