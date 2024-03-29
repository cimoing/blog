---
title: "禁用Windows下Ctrl+Space快捷键"
date: 2020-10-10T17:16:56+08:00
Description: ""
Tags: ["Windows", "快捷键", "注册表"]
Categories: ["其它"]
DisableComments: false
---

## 操作步骤

1. 打开``注册表`` (``开始->regedit``)
2. 切换至 ``HKEY_CURRENT_USER/Control Panel/Input Method/Hot Keys``
3. ``00000070`` 为``繁体中文``快捷键，``00000010 ``为``简体中文快捷键``
4. 选中其中一项后其中``Key Modifiers``表示用于指定 ``Alt``/``Ctrl``/``Shift``等，默认设置的是``Ctrl``(``02c00000``)
5. 选中其中一项后其中``Virtual Key``表示用于指定第二个精确按键，这里设置的是 ``Space``(``20000000``)
6. 修改``Key Modifiers``中的``02``为``00``
7. 修改``Virtual Key``中的``20``为``FF``
8. 注销并重新登录即可
9. 注意不要在控制台修改语言快捷键，否则需要重新设置
