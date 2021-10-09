---
title: "Python虚拟环境"
date: 2021-06-18T16:03:17+08:00
Description: ""
Tags: [Python, venv, 虚拟环境]
Categories: [Python]
DisableComments: false
---

使用``venv``根据需要创建轻量级"虚拟环境"，还可以做到与系统相关目录隔离。每个虚拟环境可以定义自己的Python版本，并且可以拥有独立的Python软件包集。

== 创建虚拟环境 ==

可通过``venv``指令创建：
```python3 -m venv /path/to/new/virtual/environment```

此命令会自动创建目标目录，并生成``pyvenv.cfg``配置文件。
