---
title: "fsck 的简单使用"
date: 2020-05-27T18:46:35+08:00
draft: false
categories: ["Linux"]
tags: ["fsck"]
---

## 命令

```
fsck
```

## 用途

**fsck**命令被用于检查并且试图修复文件系统中的错误。当文件系统发生错误四化，可用fsck指令尝试加以修复。

## 语法

```
fsck [-sAVRTMNP] [-C [fd]] [-t fstype] [filesys…] [—] [fs-specific-options]
```

## 用法

* 检测一个设备 fsck /dev/sda1
* 检测指定目录 fsck /usr
* 可添加-a参数来自动修复文件系统，而不询问任何问题

## 场景

由于文件系统损坏，导致系统启动时一直自动进入到buxybox终端

```
BusyBox v1.18.5 (Ubuntu 1:1.18.5-1ubuntu4) built-in shell (ash)
Enter 'help' for a list of built-in commands.

(initramfs)
```
最终通过 fsck /dev/sda1 检测发现有异常，修复后可正常启动，怀疑是关机时异常导致  
