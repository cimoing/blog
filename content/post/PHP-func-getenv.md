---
title: "PHP函数-getenv"
date: 2021-10-09T15:11:47+08:00
Description: "PHP中环境变量相关"
Tags: [PHP, 环境变量, getenv]
Categories: [PHP]
DisableComments: false
---

## 说明

``getenv(string $varname, bool $local_only = false): string`` 获取一个环境变量的值

* ``$local_only`` 表示是否只读取系统或通过``putenv()``设置的环境变量

## 注意事项

* 默认情况下会读取``sapi``中的环境变量，``fpm``模式下即 ``$_SERVER``
* HTTP请求头中的``PROXY``不会被读取 [#72573](https://bugs.php.net/72573)
* 通过环境变量控制配置的情况下要避免定义``HTTP_``开头的环境变量，避免被远程攻击
* 此方法非线程安全
