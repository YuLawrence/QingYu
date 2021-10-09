---
title: Oracle 中 nvl 的用法
author: 禹迹
categories:
  - [技术, Oracle]
tags:
  - Oracle
date: 2021-08-10 08:30:00
---

> 记录一下平时简单开发中用到的 nvl 函数，加深对 Oracle 常用函数的理解。

### 1.nvl()的定义（来源于百科）

[NVL](https://baike.baidu.com/item/NVL)(E1, E2)的功能为：如果 E1 为 NULL，则函数返回 E2，否则返回 E1 本身。但此函数有一定局限，所以就有了 NVL2 函数。

拓展：NVL2 函数:Oracle/PLSQL 中的一个函数,Oracle 在 NVL 函数的功能上扩展，提供了 NVL2 函数。[NVL](https://baike.baidu.com/item/NVL)2(E1, E2, E3)的功能为：如果 E1 为 NULL，则函数返回 E3，若 E1 不为 null，则返回 E2。

### 2.nvl 的格式和用法

nvl(E1,E2);

要注意 E1 和 E2 的类型要一致，nvl 支持的返回值类型有字符型、日期型、日期时间型、数值型、货币型、逻辑型和 null 值。

- 用途一：

```sql
user{id,name}
select id,nvl(name,"张三") from user
```

在对所查询的字段使用 nvl，如果查询出来的结果为空值，则返回”张三“。自己在开发中，对于数据库里一些状态字段的数据，有时有的记录没有值但又需要该字段来进行判断，则可以将字段值为 1 的与字段值为 0 或 null 值的区分开来。

- 用途二：

```sql
user{id,name}
select id,name from user where nvl(id,0) = #{id}
```

一开始使用也是在这里用的多，同事写的查询方法只查了某个值 = 1 的记录，但是数据库里其他很多记录是 null，在前端显示的话就没有数据，排查出来后就是加了 nvl 函数，如果数据库里有该 id 的记录则正常查询，否则返回该字段为 0;

> 后续若有其他理解感悟，再回来补充。若有大佬觉得哪里有问题，请不吝赐教。QAQ
