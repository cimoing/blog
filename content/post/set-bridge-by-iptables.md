---
title: "使用iptables设置中继服务器"
date: 2017-02-24T14:41:32+08:00
Description: ""
Tags: ["iptables", "中继"]
Categories: ["Linux"]
DisableComments: false
---

想设置中继服务器的主要原因是中国电信宽带访问海外服务器太慢了,正好国内有个闲置的阿里云服务器,所以就想通过iptables端口转发的功能实现中继功能。

### 开启系统转发功能

```
vi /etc/sysctl.conf
Change
====
net.ipv4.ip_forward = 1
====
```

### 设置iptables

```
iptables -t nat -A PREROUTING -i eth0 -p tcp –dport [目标端口] -j DNAT –to [目标ip]
iptables -t nat -A PREROUTING -i eth0 -p udp –dport [目标端口] -j DNAT –to [目标ip]
iptables -t nat -A POSTROUTING -p tcp -d [目标ip] –dport [目标端口] -j MASQUERADE
iptables -t nat -A POSTROUTING -p udp -d [目标ip] –dport [目标端口] -j MASQUERADE
```
然后执行**service iptables save**保存配置
