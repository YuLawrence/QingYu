---
title: 关于Oracle插入纪录如何同时返回主键
date: 2021-08-17 11:29:06
categories:
  - [技术, Oracle]
tags: Oracle
---

> 今天上午开发中遇到的一个问题，之前也确实没有使用，写了好久才解决。

### 1.问题由来

前端需要在调用插入方法后获取相应的 id 主键。使用的是 Oracle 的数据库，Oracle 的主键是采用序列自增的。

### 2.实现思路

```sql
<insert id="id" parameterType="com.lawrence.entity.User" useGeneratedKeys="true" keyProperty="id" >
    <selectKey resultType="java.lang.Integer" order="BEFORE" keyProperty="id">
        select SEQ_USER.nextval as id from DUAL
    </selectKey>
    insert into User(ID,NAME)
    values(#{id},#{name})
</insert>
```

### 3.用到的关键字解释：

- useGeneratedKeys="true" ：使用自动增长的主键
- keyProperty="id" ：将返回值赋给指定的列
- order="before" ：设置 selectKey 中包含的语句先执行

### 4.具体应用

```java
Systemt.out.println(user.getId);      // 此时插入前主键为空值
userMapper.insertUser(user);
Systemt.out.println(user.getId);      // 此时的ID已经为插入后的主键
```
