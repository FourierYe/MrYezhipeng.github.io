---
title: Hibernate框架Session.createQuery方法抛出java.lang.NoSuchMethodError异常
date: 2019-01-29 18:58:22
tags:
---

用Maven做SSH框架整合模拟登陆页面时，抛出java.lang.NoSuchMethodError异常，解决方案总结：

``` java
session=sessionFactory.openSession();
String hql="from UserManageEntity where uname=? and upassword=?";
Query query= session.createQuery(hql); //这里报错
```

org.hibernate转移到org.hibernate.query
查看是否导入的是org.hibernate.query包而不是org.hibernate包，由于版本更替，Spring已将Session.createQuery方法转移到org.hibernate.query包中。

查看jar包版本是否包含Session.createQuery方法
本人就是遇到此bug，困扰了好几天。下面是我犯得错误，以此为鉴。

``` xml
<!-- Hibernate -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.0.7.Final</version>
</dependency>
```

应该将5.0.7.Final版本改为5.2.9.Final版本
正确配置

``` xml
<!--Hibernate-->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-core</artifactId>
    <version>5.2.9.Final</version>
</dependency>
```

踩的坑多了，你就成了大神。

