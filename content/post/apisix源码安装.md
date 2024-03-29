---
title: "Apisix源码安装"
date: 2021-11-03T15:41:00+08:00
Description: ""
Tags: []
Categories: []
DisableComments: false
draft: true
---

## 0. 安装OpenResty

```shell
# download openssl version 1.1.1l
# 编译并安装OpenResty 需要指定openssl源码路径 --openssl=xxx
# 移动编译后的openssl到系统目录，如 /usr/local/openresty/openssl
```

## 1. 安装apisix

```shell
# 安装lua luarocks
# 修改luarocks配置，添加openssl库路径
# 下载apisix源码到/usr/local/apisix目录
# 在apisix源码目录执行make deps安装依赖
# 执行make install发布执行脚本
# 修改``apisix/cli/apisix.lua``脚本，在 pkg_path后追加 ``.. apisix_home .. "/?.lua"
```

## 2. 启动

``apisix start``
