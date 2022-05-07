---
tags: [Notebooks/Study]
title: eureka Q&A
created: '2022-04-29T08:07:15.252Z'
modified: '2022-04-29T08:41:17.355Z'
---

# eureka Q&A

## 环境配置
Q1：知名jar包无法find？
A：使用到的方法：
- 父级的pom.xml，点开parent的artifactId，看pom是否下载成功（我的问题是这里没下载成），故问题定位为包的下载问题
- 进一步的，我的镜像配置的是淘宝，.pom中写的“无法访问”，故需要修改settings.xml中的mirrors
  - 修改后的镜像代码附后
  - 修改后还是不行，排查了：
    - maven的环境变量
    - IDEA中maven的settings和.m2/repo文件的路径
    - 修改完路径后，重新reload跑通
  - 实在不行，清理IDEA缓存或者重启电脑再看看

转到了contract-dev分支
父级pom.xml模块部分修改为：
```xml
    <modules>
        <module>product_service</module>
        <module>order_service</module>
        <module>gatewaydemo</module>
        <module>railwayticket</module>
        <module>sql_service</module>
        <module>weather-forecast</module>
        <module>json-parse</module>
        <module>pure_eureka</module>
    </modules>
```

--

## 环境配置完成后

### 基本流程
pure-eureka：http://localhost:8761/
查看信息：http://localhost:8761/eureka/apps
服务注册，weather：http://localhost:8025/v1/weather/forecast?location=%27Beijing%27&time=%27tomorrow%27
网关注册，gatewaydemo：
- http://localhost:9999/weather-forecast/v1/weather/forecast?location=%27Beijing%27&time=%27tomorrow%27
- http://localhost:9999/api/v1/weather/forecast?location=%27Beijing%27&time=%27tomorrow%27

### 常见问题
Q：开启网关的时候：registration failed Cannot execute request on any known server
A：application.properties/yml的defaultZone没有和eureka地址匹配
```yml
eureka:
  client:
    service-url:
      defaultZone: http://localhost:8761/eureka
```

Q：Unable to connect to Redis; nested exception is io.lettuce.core.RedisConnectionException: Unable to connect to localhost:6379

A：安装Redis
http://c.biancheng.net/redis/windows-installer.html
windows：https://github.com/tporadowski/redis/releases

settings.xml[mirrors]
```xml
<mirrors>
<mirror>
    <id>aliyun-public</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun public</name>
    <url>https://maven.aliyun.com/repository/public</url>
</mirror>

<mirror>
    <id>aliyun-central</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun central</name>
    <url>https://maven.aliyun.com/repository/central</url>
</mirror>

<mirror>
    <id>aliyun-spring</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun spring</name>
    <url>https://maven.aliyun.com/repository/spring</url>
</mirror>

<mirror>
    <id>aliyun-spring-plugin</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun spring-plugin</name>
    <url>https://maven.aliyun.com/repository/spring-plugin</url>
</mirror>

<mirror>
    <id>aliyun-apache-snapshots</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun apache-snapshots</name>
    <url>https://maven.aliyun.com/repository/apache-snapshots</url>
</mirror>

<mirror>
    <id>aliyun-google</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun google</name>
    <url>https://maven.aliyun.com/repository/google</url>
</mirror>

<mirror>
    <id>aliyun-gradle-plugin</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun gradle-plugin</name>
    <url>https://maven.aliyun.com/repository/gradle-plugin</url>
</mirror>

<mirror>
    <id>aliyun-jcenter</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun jcenter</name>
    <url>https://maven.aliyun.com/repository/jcenter</url>
</mirror>

<mirror>
    <id>aliyun-releases</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun releases</name>
    <url>https://maven.aliyun.com/repository/releases</url>
</mirror>

<mirror>
    <id>aliyun-snapshots</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun snapshots</name>
    <url>https://maven.aliyun.com/repository/snapshots</url>
</mirror>  

<mirror>
    <id>aliyun-grails-core</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun grails-core</name>
    <url>https://maven.aliyun.com/repository/grails-core</url>
</mirror>

<mirror>
    <id>aliyun-mapr-public</id>
    <mirrorOf>*</mirrorOf>
    <name>aliyun mapr-public</name>
    <url>https://maven.aliyun.com/repository/mapr-public</url>
</mirror>
    </mirrors>
```


一些提问：
1 有哪些包管理工具
2 各种协议的默认打开端口
3 
