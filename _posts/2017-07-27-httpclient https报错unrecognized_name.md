---
title: httpclient https报错unrecognized_name
date: 2017-07-27 14:59:21
tags: [https,httpclient,ssl]
categories: Java
---
 ## 0x10 突然出现的 unrecognized_name
 今天在使用`httpclient`([Apache HttpComponents](http://hc.apache.org/))访问https时候突然出现了一个戳手不及的错误
 ```java
 javax.net.ssl.SSLProtocolException: handshake alert:  unrecognized_name
 ```
 嗯?然后在之前项目是没有问题的,最神奇的时候,有的https可以访问,有的不可以...
 具体原因不是很清楚.
 于是乎[google](http://www.google.com)了下,果不其然,在stackoverflow发现了[
SSL handshake alert: unrecognized_name error since upgrade to Java 1.7.0](https://stackoverflow.com/questions/7615645/ssl-handshake-alert-unrecognized-name-error-since-upgrade-to-java-1-7-0)这个问题,于是乎有了最暴力的解决方案:
```java
设置jvm启动参数 java -Djsse.enableSNIExtension=false xxxx
或者直接使用java的进行设置:
System.setProperty("jsse.enableSNIExtension", "false");
```
然后你会发现,神奇的就好了.

>java 7 introduced SNI support which is enabled by default. I have found out that certain misconfigured servers send an "Unrecognized Name" warning in the SSL handshake which is ignored by most clients... except for Java. As @Bob Kerns mentioned, the Oracle engineers refuse to "fix" this bug/feature.