---
title: SpingIOC
date: 2025-04-07
lang: zh
duration: 3min
type: note
---

## Xml 和 注解方式
xml 此时配置主要包括以下内容：
+ 配置文件确定扫描范围

  扫描Ioc/DI注解
  <context:component-scan base-package="com.atguigu.dao,com.atguigu.service,com.atguigu.controller" />
+ xml引入外部配置 

  <context:property-placeholder location="application.properties" />
+ 当使用第三方jar包的类，添加到ioc容器

