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


单位网络使用代理服务器上网。在网络--代理服务器中设置代理之后浏览器可以上网。但使用apt-get命令无法更新软件，就连源都链接不上。 

按照网上的设置了 /etc/profile文件，追加：
export http_proxy="http://keystone:keystone@192.168.161.149:808"
export https_proxy="http://keystone:keystone@10.10.198.12:808"
配置完成后执行：source /etc/profile

最终解决方法如下： 
##########################  web代理（wget等命令）	#######################
~ ./.bashrc 下配置：
export http_proxy="http://10.10.198.207:808"
export https_proxy="http://10.10.198.10:808"

##########################	给APT配置代理	#########################
在/etc/apt/下建立一个文件 apt.conf（名称随意），编辑内容如下： 
$ vi /etc/apt/apt.conf
Acquire::http::proxy "http://192.168.1.100:808/";
Acquire::ftp::proxy "ftp://192.168.1.100:808/";
Acquire::https::proxy "https://192.168.1.100:808/";

如果是要使用用户名密码登录代理服务器，需要按以下格式：
http://username:password@yourproxyaddress:proxyport

更新执行： 
sudo apt-get update -c apt.conf
