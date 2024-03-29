---
title: "分布式IM架构"
date: 2021-11-01T14:20:16+08:00
Description: ""
Tags: []
Categories: []
DisableComments: false
draft: true
---

## 设计目标

0. 可扩展
1. 高可用
2. 高并发
3. c1000k

## 基础服务

0. 唯一ID生成器
1. 消息服务器(Redis、RabbitMQ等)

## 关键服务

0. WebSocket服务 保持长连接
1. 数据路由服务 数据转发，把消息或指令发送到对应的服务器
2. 业务逻辑 实际业务逻辑执行

### WebSocket服务

此服务的特点为大内存、大带宽，用于尽可能多的与用户建立连接并进行数据的首发。
次服务要保证稳定性，因为每次服务的重新部署即意味着大量用户的掉线及重连，进而导致服务瞬间流量过高。

每台WebSocket服务器需要一个唯一ID，并监听或订阅对应的消息队列。

#### 连接

* 每个连接有一个全局唯一ID
* 全局唯一ID与机器中的fd存在一一对应关系
* 用户登陆需要包含连接ID，用于绑定

#### 指令

* 给指定连接发送消息 action:send, body:xxxxxx
* 断开连接 action:close

### 数据路由服务

为WebSocket与业务逻辑之间的胶水，WebSocket服务中每个长连接对应一个用户，需要由数据路由服务转发指定用户的消息到指定的WebSocket服务器进行发送。

### 业务逻辑服务

实际执行逻辑，聊天、群聊、聊天记录、消息过滤等逻辑由此处执行。
由于业务逻辑变更相对比较频繁，因此拆分出来之后可减少用户的重连操作。同时变同步为异步，对IM这种高频场景可提高并发性能。

#### 接口

* 登陆 使用连接ID及token进行登陆，登陆后绑定用于及登陆ID (可根据需要增加多平台登陆限制）
* 单聊消息 发送消息给指定用户
* 群聊 遍历群成员发送消息
* 离线消息
* 聊天记录
* ... 扩展逻辑

## 瓶颈

0. 长连接及业务逻辑部分均可以横向扩展，受业务增长影响较小
1. 消息队列服务 在消息量增大的情况下压力会快速增加。

### 解决方案

0. 优化消息大小，例如修改json为protobuf，减小数据包大小
1. WebSocket服务可分别监听不同的消息队列服务器，路由服务根据情况转发即可。
