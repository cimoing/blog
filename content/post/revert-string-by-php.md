---
title: "反转字符串-PHP"
date: 2017-03-26T14:34:01+08:00
Description: ""
Tags: ["字符串", "反转", "正则"]
Categories: ["PHP"]
DisableComments: false
---

反转一个字符串,比如"abc"反转后结果为"cba",看到问题首先想到的是**strrev**函数,此函数在反转由单字节字符组成的字符串是没有问题,但是无法处理Unicode这种多字节字符.因此通用的办法是分割字符串,然后重新组合。

```
$str = "我思故我在 2333";
preg_match_all("/./us", $str, $matches);
echo join(”, array_reverse($matches[0]));
```
这里用到了模式修饰符 **u** 和 **s** ,其中u表示以UTF-8读取正则表达式和目标字符串,因此在反转前还要做下编码检查与转换。**s** 使 **.** 通配符可以匹配换行符。
> 在 **u** 模式下可以使用 **\p{Han}** 匹配中文
