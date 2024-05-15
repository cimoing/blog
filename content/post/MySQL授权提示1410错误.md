---
title: "MySQL授权提示1410错误"
date: 2024-05-15T11:12:34+08:00
Description: ""
Tags: [MySQL]
Categories: []
DisableComments: false
---

从MySQL8.0及以后,在执行授权并创建用户指令时会提示一下错误:  
```
-- SQL: GRANT ALL PRIVILEGES ON homestead.* TO john@'%';
[42000][1410] You are not allowed to create a user with GRANT
```
正确流程应该是先创建用户,然后再进行授权:
```sql
-- right
CREATE USER john@'%' IDENTIFIED BY 'passwd';
GRANT ALL PRIVILEGES ON homestead.* TO john@'%' WITH GRANT OPTION;
FLUSH PRIVILEGES ;
```

另外,在授予权限时不再可以指定密码,会提示语法错误:
```sql
GRANT ALL PRIVILEGES ON homestead.* TO john@'%' IDENTIFIED BY 'passwd';
-- [42000][1064] You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'IDENTIFIED BY 'passwd'' at line 1
```
