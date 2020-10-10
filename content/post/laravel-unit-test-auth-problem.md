---
title: "Laravel测试用例遇到的问题"
date: 2018-04-11T14:28:57+08:00
Description: ""
Tags: ["Laravel", "Auth", "Test", "测试"]
Categories: ["PHP"]
DisableComments: false
---
最近在对代码进行优化，其中一项是是使用懒加载的方式获取当前登录的用户对象，在执行测试代码时发现模拟的多个用户请求只有一个生效。最终确认是由于测试用例中同一个Controller只初始化一次导致用户信息未更新。

代码片段如下

```
#Controller.php

private $user = null;

public function getUser() {
    if(!$this->user && Auth::id()) {
        $this->user = User::find(Auth::id());
    }
    return $this->user;
}
```

最终修改如下

```
#Controller.php
private $user = null;

public function getUser() {
    if((!$this->user && Auth::id()) || ($this->user && Auth::id() && $this->user->id != Auth::id())) {
        $this->user = User::find(Auth::id());
    }
    return $this->user;
}
```
