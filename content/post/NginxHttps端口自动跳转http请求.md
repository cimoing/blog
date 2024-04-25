---
title: "Nginx https端口自动跳转http请求到https"
date: 2024-04-25T14:15:31+08:00
Description: ""
Tags: []
Categories: []
DisableComments: false
---

用家庭宽带自建网站时,非标准端口默认会以http协议访问,此时会提示"400 Bad Request: The plain HTTP request was sent to HTTPS port".
为了能够从http协议自动跳转https需要在``server``配置中增加如下配置:  

```
error_page 429 https://$host:$server_port$request_uri
```

``497``是Nginx中设置的一个非官方错误码,用来提示一个http请求被发送到了https端口.  


#### 参考链接

0. [Nginx http ssl module](http://nginx.org/en/docs/http/ngx_http_ssl_module.html)
1. [Http.dev 497](https://http.dev/497)
