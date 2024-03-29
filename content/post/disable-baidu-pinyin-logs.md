---
title: "禁用百度拼音输入法for Linux日志输出"
date: 2020-06-03T10:37:01+08:00
Description: ""
Tags: ["百度拼音输入法"]
Categories: ["Linux"]
DisableComments: false
---

最近百度发布了``百度拼音输入法for Linux``，上手之后甚是喜欢，主要相比搜狗时不时的崩溃更加稳定。
最近通过``journalctl -xfe``查看系统日志时发现会循环出现如下日志内容：  
```
6月 03 10:40:27 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:30 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:33 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:36 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:39 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:42 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:45 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:48 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:51 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:54 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
6月 03 10:40:57 @@@@ fcitx-ui-baidu-qimpanel-ln.desktop[2730]: baidu panel was already started
```
强迫症怎么会允许这种无意义东西出现？经查这是百度输入法的一个保活脚本输出，用于在进程意外崩溃后重启。脚本路径:  
``/opt/apps/com.baidu.fcitx-baidupinyin/files/bin/bd-qimpanel.watchdog.sh``
内容如下：
```
#!/bin/sh

while :
do
  stillRunning=$(ps -ef |grep "/opt/apps/com.baidu.fcitx-baidupinyin/files/bin/baidu-qimpanel" | grep -v "grep")
  if [ "$stillRunning" ] ; then
    echo "baidu panel was already started" 
  else
    echo "baidu panel service was not started" 
    echo "Starting panel ..." 
    /opt/apps/com.baidu.fcitx-baidupinyin/files/bin/baidu-qimpanel
    echo "bd panel was started!" 
  fi
  sleep 3
done
```
简单修改后去除在进程正常情况下的输出：  
```
#!/bin/sh
  
while :
do
  stillRunning=$(ps -ef |grep "/opt/apps/com.baidu.fcitx-baidupinyin/files/bin/baidu-qimpanel" | grep -v "grep")
  if [ ! "$stillRunning" ] ; then
    echo "baidu panel service was not started" 
    echo "Starting panel ..." 
    /opt/apps/com.baidu.fcitx-baidupinyin/files/bin/baidu-qimpanel
    echo "bd panel was started!" 
  fi
  sleep 3
done
```
