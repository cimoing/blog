---
title: "SSH服务增加双因素认证"
date: 2020-06-03T18:31:55+08:00
Description: ""
draft: false
Tags: ["SSH", "2FA", "双因素认证"]
Categories: ["Linux", "安全"]
ref: "https://ubuntu.com/tutorials/configure-ssh-2fa"
DisableComments: false
---

## 简介

**SSH**常被用来远程远程访问Linux系统。由于我们经常用来连接重要的电脑，因此推荐增加另外一层安全防护。这里讲述了**双因素认证(2FA)**的配置方法。

### 什么是双因素认证

多因素认证是一种通过至少两种认证方式来鉴权的方法。最常见也最简单的方式就是双因素认证，通过一个密码和一个基于时间生成代码的APP。

我们这里会使用Google Authenticator应用来生成验证码。

### 准备

* 运行Ubuntu 16.06 LTS或以上版本
* 运行Android或iOS系统的手机
* 一个配置好的SSH连接
* 明白密码被盗之后的危险
* 你不需要了解什么是双因素认证及其是如何生效的


## 安装及配置依赖

### 安装Google Authenticator PAM模块

在终端中输入:
```
sudo apt install libpam-google-authenticator
```

### 配置SSH

为了上SSH使用**Google Authenticator PAM**模块，在``/etc/pam.d/sshd``添加如下内容：
```
auth required pam_google_authenticator.so
```
然后重启``sshd``服务：
```
sudo systemctl restart sshd.service
```

编辑``/etc/ssh/sshd_config``文件，把``ChallengeResponseAuthentication``从``no``改为``yes``：
```
# Change to yes to enable challenge-response passwords (beware issues with
# some PAM modules and threads)
ChallengeResponseAuthentication no # CHANGE THIS TO YES

# Change to no to disable tunnelled clear text passwords
#PasswordAuthentication yes
```

## 认证配置

Google Authenticator是的配置双因素认证相比libpam-oath更加简单。
在终端中执行``google-authenticator``命令。它会提问几个问题，下面是推荐配置：

* Make tokens **time-base**(基于时间生成token): yes
* Update the ``.google_authenticator``(更新.google_authenticator文件) file: yes
* Disallow multiple uses(禁止重复使用): yes
* Increase the original generation time limit(增加原来生成token的时间限制): no
* Enable rate-limiting(开启速率限制，防暴力破解): yes

最后会生成一个二维码，下面有几个用于在无法使用手机的情况下使用的验证码,可以写到纸上或者其它安全的地方。

然后打开Google Authenticator应用并添加密钥(或扫码)就可以使用了。

> 不要使用非加密的服务(如笔记同步服务)去保存密钥。
