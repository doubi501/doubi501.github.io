subtitle:   Docker配置http（s）代理
date:       2017-07-10
author:   roy
header-img: 
catalog: true
tags:
    - Linux
    - docker
---


> 为docker服务创建一个内嵌的systemd目录
```
$ mkdir -p /etc/systemd/system/docker.service.d
```
> 创建/etc/systemd/system/docker.service.d/http-proxy.conf文件，并添加HTTP_PROXY环境变量。其中[proxy-addr]和[proxy-port]分别改成实际情况的代理地址和端口, 如果还有内部的不需要使用代理来访问的Docker registries，那么还需要制定NO_PROXY环境变量：
```
[Service]
Environment="HTTP_PROXY=http://[proxy-addr]:[proxy-port]/" "HTTPS_PROXY=https://[proxy-addr]:[proxy-port]/"
Environment="HTTP_PROXY=http://[proxy-addr]:[proxy-port]/" "HTTPS_PROXY=https://[proxy-addr]:[proxy-port]/" "NO_PROXY=localhost,127.0.0.1,docker-registry.somecorporation.com"
```
　> 更新配置：
```
$ systemctl daemon-reload
```
> 重启Docker服务：
```
$ systemctl restart docker
```