---
title: "Composer 开发入门"
date: 2019-07-18T10:18:06+08:00
draft: false
tags: [ "composer" ]
categories: [ "PHP" ]
---

Composer对于分发自己的PHP软件包，方便别人引用来说是很好的一种方式。平时开发中经常会用到Composer来引入三方的优质软件包，例如 Laravel、easywechat等等，如果我们有一些好的想法如果分发出去呢？下面我们就一步步来告诉大家如何实现自己的第一个Composer包。

## 初始化项目

首先创建**composer.json**文件用于生命基本信息。

```
{
     "name": "jake/dev-package",
     "description": "软件包描述",
     "autoload": {
         "psr-4": {
             "Jake\Package\": "src"
         }
     },
}
```
配置好之后就可以在**src**目录编写项目逻辑，这里我们声明了自动载入符合psr-4规范。


## 引用软件包

在上传到仓库之前，我们可以使用本地路径来引入

```
{
    "name": "jake/application",

    ...

    "repositories": {
      "dev-package": {
        "type": "path",
        "url": "relative/or/absolute/path/to/my/dev-package",
        "options": {
          "symlink": true
        }
      }
    }
}
```

配置中的**"type": "path"** 表示引入的是一个本地仓库，url定义了包的路径，路径可以使用相对路径或绝对路径。

虽然设置了type和url后就可以开发了，但是composer会复制包的代码到vender目录，而且每次包发生变更都要执行**composer update**

为了避免更新执行update操作，可以通过设置**"symlink": true**参数让composer创建一个软链接到包所在目录

最后一步操作就是使用**composer require**命令来引入软件包。

```
composer require jake/dev-package @dev
```
